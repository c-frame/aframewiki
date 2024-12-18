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

If you use [aframe-htmlmesh](https://github.com/AdaRoseCannon/aframe-htmlmesh), be aware this syntax doesn't work in VR:

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
I think SolidJS expect a PointerEvent and not a MouseEvent, something like that.
I tried to emulate `pointerdown` followed by `pointerup` but it didn't work.
I found the following fix to work, but I'm not sure if it's a proper fix to contribute.
You can replace [here](https://github.com/AdaRoseCannon/aframe-htmlmesh/blob/fcedc6d86dcafc122183d518984f45c972e7b154/src/HTMLMesh.js#L527)

```js
element.dispatchEvent(new MouseEvent(event, mouseEventInit));
```

by

```js
if (event === "click") {
  // For SolidJS UI to work when onClick is a function and not a string
  element.click();
} else {
  element.dispatchEvent(new MouseEvent(event, mouseEventInit));
}
```

and the first syntax will work properly in VR.
