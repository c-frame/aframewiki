# Using SolidJS with A-Frame

Author: Vincent Fretin.
Last updated: 2025-02-11.

The final code of this tutorial is available at https://github.com/vincentfretin/my-aframe-solid-app

You can create a [SolidJS](https://www.solidjs.com) SPA (Single Page Application) project with vite and add A-Frame and Networked-Aframe to it.
Those instructions are opinionated to use typescript, tailwindcss 4 and prettier.

Create the project with the following commands:

```sh
npm create vite@latest my-aframe-solid-app -- --template solid-ts
cd my-aframe-solid-app
npm install
npm install --save-dev @vitejs/plugin-basic-ssl tailwindcss @tailwindcss/vite prettier prettier-plugin-tailwindcss
```

Create `.prettierrc.json` file with the following content:

```json
{
  "printWidth": 120,
  "plugins": ["prettier-plugin-tailwindcss"],
  "tailwindAttributes": ["classList"]
}
```

Edit `vite.config.ts` to enable https, listening on your network interface and proxying to naf server:

```js
import { defineConfig } from "vite";
import basicSsl from "@vitejs/plugin-basic-ssl";
import solid from "vite-plugin-solid";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  server: {
    host: "0.0.0.0",
    proxy: {
      "/socket.io": {
        target: "http://127.0.0.1:8080/socket.io",
        changeOrigin: true,
        ws: true,
      },
    },
  },
  plugins: [basicSsl(), tailwindcss(), solid()],
});
```

See https://vite.dev/guide/ and https://tailwindcss.com for more information.

Then include A-Frame script tag and other components you need in the `index.html` file. The easiest way is to grab the content of the `index.html` file of the [naf-project](https://glitch.com/edit/#!/naf-project) glitch and paste it in the `index.html` file of your project and put back the two lines before `</body>`:

```html
<div id="root"></div>
<script type="module" src="/src/index.tsx"></script>
```

and replace

```html
<script src="/easyrtc/easyrtc.js"></script>
```

by

```html
<script src="https://unpkg.com/open-easyrtc@2.1.0/api/easyrtc.js"></script>
```

Also remove `import "./App.css";` in `App.tsx` and remove the `App.css` file.
Replace the content of `index.css` by:

```css
@import "tailwindcss";
```

in `App.tsx` add a button for example:

```js
function App() {
  return (
    <>
      <div class="pointer-events-none absolute bottom-4 left-4 z-10 gap-2 [&>*]:pointer-events-auto">
        <button
          onclick={() => {
            const el = document.querySelector("[environment]");
            if (!el) return;
            // @ts-ignore
            if (el.getAttribute("environment").preset === "forest") {
              el.setAttribute("environment", "preset: arches");
            } else {
              el.setAttribute("environment", "preset: forest");
            }
          }}
          class="cursor-pointer rounded-xl border-4 border-black/80 bg-white px-4 py-1 font-bold text-black/80 hover:border-[#ef2d5e] hover:text-[#ef2d5e]"
        >
          Change environment
        </button>
      </div>
    </>
  );
}
export default App;
```

Add the networked-aframe server:

```sh
npm install networked-aframe
npm install --save-dev concurrently
```

Grab the content of `server.js` from glitch and put it in your project in a `server.cjs` file at the root. Note the cjs extension here needed because we're in a ESM node project declared with the `"type": "module"` in `package.json` file and the server code is in CommonJS format.
You also need to change `app.use(express.static("public"));` by `app.use(express.static("dist"));` in the `server.cjs` file that will be used when run with `npm start`, that's also what is used with hosting service like glitch.

Create public/js directory and put the content of `public/js/spawn-in-circle.component.js` from glitch in it.

Modify the `package.json` scripts:

```json
"start": "node server.cjs",
"dev": "concurrently vite \"node server.cjs\"",
```

For development, run

```sh
npm run dev
```

For production, run

```sh
npm run build
```

and test if all is working with

```sh
npm start
```

To deploy to Glitch, see next section.

To deploy to a production server, you need to deploy the `dist` folder
and run `pm2 start server.js` for example with nginx in front plus certbot to create a letsencrypt certificate.
See https://github.com/networked-aframe/networked-aframe/issues/244 for more details.

See [naf-valid-avatars](https://github.com/networked-aframe/naf-valid-avatars) as an example of an UI for networked-aframe written with SolidJS.

I advice not to use [SolidStart](https://start.solidjs.com/) even with CSR (Client-side rendering as known as SPA with `{ ssr: false }`) because you don't have access to the `index.html` file so making hard to add scripts tags and template tags (for networked-aframe) and following other A-Frame examples.

## Deploy to Glitch

Deploying to Glitch requires to type some commands.

Note that Glitch only supports NodeJS 16 (in Feb 2025) and Vite 6 requires minimum NodeJS 18 so you can't run `npm run build` in the Glitch Console.

Create a new Glitch project with the template glitch-hello-website, edit the subdomain in Settings with a name you want.

On your machine in your project, set up a git repo:

Add to `package.json` just before the ending `}`:

```js
"engines": {
  "node": ">=16"
}
```

and remove dist from the `.gitignore` file.

Create your repo on GitHub and follow the instructions there. Mainly:

```sh
git init
git add .
git commit -m"first commit"
git branch -M main
git remote add origin <your github ssh url>
git push -u origin main
```

The above steps are only needed once, you can skip them for the next time you want to deploy.

From your machine, create a build and push:

```sh
git rm -rf dist
npm run build
git add dist
git commit -m"build dist"
git push
```

Open the Glitch Console and type:

```sh
git remote add github <your github https url>
git fetch github
git reset --hard github/main
refresh
```

It's deployed!

## Rendering the scene via a SolidJS component (advanced)

You can go further and use [Solid router](https://github.com/solidjs/solid-router) and render your scene with a SolidJS component on a specific route.
You can also use SolidJS signals to update some components of your scene.
The only gotcha is to use `attr:` on your components for them to be reactive. This applies to any web components, see the [mention in SolidJS documentation](https://www.solidjs.com/docs/latest/api#attr___).

Add the SolidJS router library and meta library that will handle the title tag:

```sh
npm install @solidjs/router @solidjs/meta
```

Create or modify all the following files in the `src` directory.

`App.tsx`:

```js
import { MetaProvider, Title } from "@solidjs/meta";
import { Router, Route } from "@solidjs/router";
import { lazy } from "solid-js";

const Home = lazy(() => import("./pages/Home"));
const Room = lazy(() => import("./pages/Room"));

function App() {
  return (
    <MetaProvider>
      <Router
        root={(props) => {
          return (
            <>
              <Title>My A-Frame project</Title>
              {props.children}
            </>
          );
        }}
      >
        <Route path="/" component={Home} />
        <Route path="/room" component={Room} />;
      </Router>
    </MetaProvider>
  );
}
export default App;
```

`pages/Home.tsx`:

```js
import { A } from "@solidjs/router";

function Home() {
  return (
    <main class="absolute inset-0 flex flex-col items-center justify-center gap-4 bg-[#ef2d5e] text-white">
      <h1 class="text-3xl">Hello A-Frame!</h1>
      <A href="/room" class="underline">
        Go to room
      </A>
    </main>
  );
}
export default Home;
```

`signals.tsx`:

```js
import { createSignal } from "solid-js";

export const [primitive, setPrimitive] = createSignal("box");
```

`pages/Room.tsx`:

```js
import { Scene } from "../Scene";
import { UI } from "../UI";

function Room() {
  return (
    <>
      <Scene />
      <UI />
    </>
  );
}

export default Room;
```

`Scene.tsx`:

```js
import { onCleanup, onMount } from "solid-js";
import { primitive } from "./signals";

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

export function Scene() {
  onMount(() => {
    // Because a-scene is in a div, we need this additional style otherwise the VR button is not visible.
    document.querySelector("#root")!.setAttribute("style", "position: absolute; inset: 0;");
  });
  onCleanup(() => {
    document.querySelector("#root")!.removeAttribute("style");
    // Remove class that was added by aframe, otherwise we can't scroll when we go back to a non- A-Frame page. (not needed in aframe 1.7.0)
    document.querySelector("html")!.classList.remove("a-fullscreen");
  });
  return (
    <a-scene
      renderer="physicallyCorrectLights: true;"
      networked-scene="
          room: basic;
          adapter: wseasyrtc;"
    >
      <a-entity id="cameraRig">
        <a-entity
          id="player"
          networked="template: #avatar-template; attachTemplateToLocal: false;"
          camera
          position="0 1.6 0"
          spawn-in-circle="radius:3"
          wasd-controls
          look-controls
        >
          <a-sphere class="head" visible="false" random-color></a-sphere>
        </a-entity>
      </a-entity>

      <a-entity environment="preset: arches"></a-entity>
      <a-entity light="type: ambient; intensity: 1.0"></a-entity>

      <a-entity attr:geometry={`primitive: ${primitive()}`} position="0 2 0"></a-entity>
    </a-scene>
  );
}
```

`UI.tsx`:

```js
import { For } from "solid-js";
import { primitive, setPrimitive } from "./signals";

export function UI() {
  return (
    <div class="pointer-events-none absolute bottom-4 left-4 z-10 flex gap-2 [&>*]:pointer-events-auto">
      <button
        onclick={() => {
          const el = document.querySelector("[environment]");
          if (!el) return;
          // @ts-ignore
          if (el.getAttribute("environment").preset === "forest") {
            el.setAttribute("environment", "preset: arches");
          } else {
            el.setAttribute("environment", "preset: forest");
          }
        }}
        class="cursor-pointer rounded-xl border-4 border-black/80 bg-white px-4 py-1 font-bold text-black/80 hover:border-[#ef2d5e] hover:text-[#ef2d5e]"
      >
        Change environment
      </button>
      <For each={["sphere", "box"]}>
        {(choice) => (
          <button
            onclick={() => setPrimitive(choice)}
            class="cursor-pointer rounded-xl border-4 bg-white px-4 py-1 font-bold text-black/80 hover:border-[#ef2d5e] hover:text-[#ef2d5e]"
            classList={{
              "border-[#ef2d5e]": primitive() === choice,
              "border-black/80": primitive() !== choice,
            }}
          >
            {choice}
          </button>
        )}
      </For>
    </div>
  );
}
```

Remove `<title>` and `<a-scene>` tags in `index.html`, keep the `<template>` tags.
So just keep in `index.html` `<body>` the following:

```html
<body>
  <template id="avatar-template">
    <a-entity class="avatar">
      <a-sphere class="head" scale="0.45 0.5 0.4"></a-sphere>
      <a-entity class="face" position="0 0.05 0">
        <a-sphere
          class="eye"
          color="#efefef"
          position="0.16 0.1 -0.35"
          scale="0.12 0.12 0.12"
        >
          <a-sphere
            class="pupil"
            color="#000"
            position="0 0 -1"
            scale="0.2 0.2 0.2"
          ></a-sphere>
        </a-sphere>
        <a-sphere
          class="eye"
          color="#efefef"
          position="-0.16 0.1 -0.35"
          scale="0.12 0.12 0.12"
        >
          <a-sphere
            class="pupil"
            color="#000"
            position="0 0 -1"
            scale="0.2 0.2 0.2"
          ></a-sphere>
        </a-sphere>
      </a-entity>
    </a-entity>
  </template>

  <div id="root"></div>
  <script type="module" src="/src/index.tsx"></script>
</body>
```

## Typescript types for aframe

You can install `@types/aframe` to have some types:

```
npm install --save-dev @types/aframe
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
  const dispose = render(() => <UI id={id} uiEl={uiEl} />, root);
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

## Dynamically loading components (webpack)

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
