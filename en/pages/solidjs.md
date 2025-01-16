# Use SolidJS with aframe

The following are notes from Vincent Fretin.

You can use [SolidJS](https://www.solidjs.com) (without SolidStart/vite for now, so no SSR) with aframe and networked-aframe.
You need to create `index.html` and put script tags (aframe and other components) and template tags (for networked-aframe) in it.

See [naf-nametag-solidjs](https://github.com/networked-aframe/naf-nametag-solidjs) example.

## Rendering the scene

You can go further and use [Solid router](https://github.com/solidjs/solid-router) and render your scene with a SolidJS component on a specific route.
The only gotcha is to use `attr:` on your components for them to be reactive. This applies to any web components, see the [mention in SolidJS documentation](https://www.solidjs.com/docs/latest/api#attr___).

You can render the scene like this:

```js
<Scene scene={scene()} />
```

`scene()` is a signal that is an object with sceneName and other things specific to my app.

```js
const Scene: Component<Props> = (props) => {
  onCleanup(() => {
    // Remove class that was added by aframe, otherwise we can't scroll. (not needed in aframe 1.7.0)
    document.querySelector("html").classList.remove("a-fullscreen");
  });
  return (
    <Portal>
      <a-scene>
        <a-entity
          attr:scene-switcher={props.scene.sceneName}
          shadow="cast:true;receive:true;"
        ></a-entity>
      </a-scene>
    </Portal>
  );
};
```

I'm still using webpack with SolidJS, no ssr.
I'm trying to convert my project to SolidStart so to vite, but I struggle to import dynamically my scripts and add template tags for naf. (2023-04-13)

## Typescript types for aframe

To avoid having errors in vscode, you can add an `aframe.d.ts` file in your project with the following content:

```js
declare module "solid-js" {
  namespace JSX {
    interface IntrinsicElements {
      "a-scene": any;
      "a-entity": any;
      "a-assets": any;
      "a-asset-item": any;
      "a-mixin": any;
      "a-sphere": any;
      "a-torus": any;
      "a-gltf-model": any;
      "a-light": any;
    }
  }
}
```

You can also install `@types/aframe` to have some types:

```
npm install -D @types/aframe
```

Example:

```js
import type { Entity } from "aframe";
const cameraRig = document.getElementById("cameraRig") as Entity;
cameraRig.setAttribute("movement-controls", "enabled", false);
```

## Click on button with aframe-htmlmesh

If you use [aframe-htmlmesh](https://github.com/AdaRoseCannon/aframe-htmlmesh), be aware this syntax doesn't work with SolidJS:

```js
<button
  onClick={() => {
    AFRAME.scenes[0].exitVR();
  }}
>
  Exit Immersive
</button>
```

This works:

```js
<button
  // @ts-ignore
  onClick="AFRAME.scenes[0].exitVR();"
>
  Exit Immersive
</button>
```

or

```js
<button
  // @ts-ignore
  onClick={`AFRAME.scenes[0].exitVR();`}
>
  Exit Immersive
</button>
```

but if you introduce a variable in the template literal it becomes a function and won't work.

I know SolidJS delegates pointer/touch/mouse/keyboard events and the transformed js has an hint about the click event being delegated.
That js:

```js
const createUI = (uiEl: HTMLElement) => {
  const root = document.createElement("div");
  const id = "myui";
  document.body.appendChild(root);
  const dispose = render(() => <FaseVendingUI id={id} uiEl={uiEl} />, root);
};
```

becomes

```js
const createUI = (uiEl) => {
  const root = document.createElement("div");
  const id = "myui";
  document.body.appendChild(root);
  const dispose = (0, web /* render */.XX)(
    () =>
      (0, solid /* createComponent */.a0)(UI, {
        id: id,
        uiEl: uiEl,
      }),
    root
  );
};
(0, web /* delegateEvents */.z_)(["click"]);
```

so I guess the issue is related to that, but I don't really understand how that works.

I found the following fix to work, but I'm not sure if it's a proper fix to contribute.
You can replace [here](https://github.com/AdaRoseCannon/aframe-htmlmesh/blob/fcedc6d86dcafc122183d518984f45c972e7b154/src/HTMLMesh.js#L527)

```js
element.dispatchEvent(new MouseEvent(event, mouseEventInit));
```

by

```js
if (event === "click") {
  // For SolidJS UI to work when onClick is a function and not a string,
  // we need to call element.click(), a MouseEvent won't work.
  // SolidJS may render new children synchronously if we call element.click()
  // and a new button may be clicked if it appears at the same spot looking
  // like we did a double click. So be sure to use a setTimeout here so the
  // click event is triggered when we finished iterating over the DOM elements.
  setTimeout(() => element.click(), 0);
} else {
  element.dispatchEvent(new MouseEvent(event, mouseEventInit));
}
```

and the first syntax with a function like `onClick={() => { AFRAME.scenes[0].exitVR(); }}` will work properly.

## Dynamically loading components

That's a note about dynamically requiring components and dependencies with wepback.
I have a single webpack/solidjs/tailwindcss project fully using ES modules for all my experiences.
Each experience load a json file that declare the entities with their components to be used.
When loading an experience, I'm doing a first pass on the json to gather used components,
then if it's a component not currently registered, I'm loading them all in parallel asynchronously.
That way I don't load the js of components I don't use in an experience.
I found out by trying it, webpack is automatically creating a mapping of my components in the
`./components/` directory so that's nice for security, it only allow those modules to be imported
when you do something like this:

```js
async function loadComponent(componentName) {
  try {
    return await import(`./components/${componentName}.js`);
  } catch (e) {
    console.warn(`./components/${componentName}.js does not exist`);
    return null;
  }
}
```

What it generates is the following:

```js
var map = {
  "./environment.js": [4094],
  "./flag-waving.js": [77214, 7214], // 7214 is three/addons/geometries/ParametricGeometry.js
};
function webpackAsyncContext(req) {
  // fail right away if the component is not a whitelisted module
  if (!__webpack_require__.o(map, req)) {
    return Promise.resolve().then(() => {
      var e = new Error("Cannot find module '" + req + "'");
      e.code = "MODULE_NOT_FOUND";
      throw e;
    });
  }

  // load dependencies of the component in parallel and load the component
  var ids = map[req],
    id = ids[0];
  return Promise.all(ids.slice(1).map(__webpack_require__.e)).then(() => {
    return __webpack_require__(id);
  });
}
webpackAsyncContext.keys = () => Object.keys(map);
webpackAsyncContext.id = 23797;

async function loadComponent(componentName) {
  try {
    return await __webpack_require__(23797)(`./${componentName}.js`); // __webpack_require__ will use the defined webpackAsyncContext above
  } catch (e) {
    console.warn(`./components/${componentName}.js does not exist`);
    return null;
  }
}
```
