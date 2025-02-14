# FAQ

## WARNING: Multiple instances of Three.js being imported.

This can happen when you're importing some three addon that is not included in the aframe build like this import for example:

```js
import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader.js';
```

If you don't use any build tool, what you can do is grab the addon file from the exact three.js version that is used by the aframe version you use, for example aframe 1.4.2 it's [r147](https://github.com/mrdoob/three.js/tree/r147) then modify all the imports to use the THREE global variable instead, like we did for the [orbit-controls component](https://github.com/supermedium/superframe/blob/master/components/orbit-controls/lib/OrbitControls.js#L1-L5).

Or if you use webpack and added the `three` dependency pinning the exact version in `package.json` like `"three": "0.147.0"` if you use aframe 1.4.2, you should define three as an external THREE, see [webpack config in aframe-cursor-teleport](https://github.com/c-frame/aframe-cursor-teleport/blob/master/webpack.config.js#L14-L17).

For rollup, that's similar, see [aframe-htmlmesh rollup config](https://github.com/AdaRoseCannon/aframe-htmlmesh/blob/main/rollup.config.js#L19-L21).
