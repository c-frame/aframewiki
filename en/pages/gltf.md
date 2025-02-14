# Working with GLTF & GLB 3D assets

The de-facto 3d model file format of the web.

### GLTF vs. GLB?

They're the same thing, but a gltf maintains a file/folder reference structure, while a glb is a derivative format that compresses all files into one; it's the equivalent of saving your website by minifying all assets and injecting them directly into the HTML instead of keeping them as a folder of various files.
GLBs are easier to move around and work with because you don't need to maintain the directory relationship between all the files, it's just a single file to drop in. On the other hand, when you have a gltf it's easier to edit it with other tools. We'll refer to both as GLTF in this article, and anywhere you see GLTF you can also use GLB.

## What is GLTF?

It's a file format that is ideal for using 3d models on the web. It uses JSON. It's pretty full featured and supported these days.

### limitations

_todo_

## Making GLTFs

- play around: https://threejs.org/editor/
- serious: blender

## GLTF tools

- https://thirdroom.io/pipeline
- https://gltf.report/
- https://gltf-viewer.donmccurdy.com/
- https://gltf-transform.donmccurdy.com/cli.html
- [meshOptimizer](https://github.com/zeux/meshoptimizer)
- https://github.khronos.org/glTF-Sample-Viewer-Release/
- https://sandbox.babylonjs.com/

## Optimizing GLTF assets

stub, but ideally we add detailed info on all of these:

- [reduce draw calls](https://answers.unity.com/questions/14578/whats-the-best-way-to-reduce-draw-calls.html)
- decimate geometry
- merge geometry
- use vertex colors
- use a texture atlas
- use power-of-two size textures
- bake lighting
- **use [KTX2](ktx2.md)**
- use [meshOptimizer](https://github.com/zeux/meshoptimizer) or [zencompress](https://paradowski.com/stories/introducing-zencompress-for-gltf)
- use LOD

### some resources

stub, ideally we put all this info in this article:

- https://toji.github.io/webxr-scene-optimization/
- https://www.soft8soft.com/docs/manual/en/introduction/Optimizing-WebGL-performance.html
- https://kb.kaon.com/optimizing-large-3d-models-for-the-web
- https://www.youtube.com/watch?v=1ToEPOHN1no
- https://www.youtube.com/watch?v=9-EfkeXW1MY&t=1s
- https://www.youtube.com/watch?v=vLJGO_upYEc
- https://www.youtube.com/watch?v=6ONoVEcyMqI
- https://www.8thwall.com/glb

## Textures

They make your models look good.

- Get some good creative commons one for free: https://www.sharetextures.com/
- learn the absolute basics in this [super short tutorial for total noobs](https://www.youtube.com/watch?v=sH4qi3oUEks)

## Changing some values of a material

- See this example of a [material-values component](https://github.com/akbartus/A-Frame-Component-GLTF-Manipulator/issues/2)
  to change values like color, texture, metalness, roughness of a material after the glb is loaded.

## Animations

- https://www.mixamo.com/#/
- https://github.com/c-frame/aframe-extras
- https://github.com/c-frame/aframe-extras/blob/master/src/loaders/animation-mixer.js

## Free models

- Polyhaven
- SketchFab
