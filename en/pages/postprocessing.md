# Postprocessing

As of version 1.4.1 (January 2023), A-Frame doesn't natively support three.js postprocessing, but there are ways to make it work.
Here's an [example](https://glitch.com/edit/#!/anaglyph-postprocessing?path=index.html) showing the anaglyph postprocessing effect that Diarmid did a little while ago.
Similar techniques will probably work for other postprocessing effects.

Really old [aframe-effects](https://github.com/wizgrav/aframe-effects) is also available (and seeking a maintainer);

Newer with a custom build of aframe 1.4.2 that has a threejs patch for postprocessing in VR: [Akbartus's A-Frame-Component-Postprocessing](https://github.com/akbartus/A-Frame-Component-Postprocessing)
The threejs [patch](https://github.com/supermedium/three.js/commit/d575277f70b20b3881cc7ac5d15962fe9009232d) (in the super-r173-5 branch) is now included in A-Frame 1.7.0 super-three 0.173.4 and later [master build](https://cdn.jsdelivr.net/gh/aframevr/aframe@51505e3687967ab0e01954589c6de1dd0bbbaeae/dist/aframe-master.module.min.js) Feb 28, 2025 with super-three 0.173.5 that includes a fix for the white scene when exiting VR.
In A-Frame 1.7.0, an example using threejs postprocessing with bloom effect that is working in VR: https://aframe.io/aframe/examples/showcase/post-processing/

For pmndrs postprocessing package, it's not currently working in VR, see [this issue](https://github.com/pmndrs/postprocessing/issues/677) that tracks VR support.

See also [this discussion started in June 2018](https://github.com/aframevr/aframe/pull/3645) about introducing a postprocessing API in A-Frame.

Hubs integrated (October 2022) in their [scene renderer](https://github.com/mozilla/hubs/pull/5742) the [postprocessing library](https://www.npmjs.com/package/postprocessing)
but they don't use the A-Frame renderer anymore, so finding a good API for A-Frame from the discussion above is still needed.
Some tweets about the bloom effect in Hubs:

- https://twitter.com/MozillaHubs/status/1572571349796765697 and the stream [Hubs Dev Stream: Post Processing in Hubs](https://www.youtube.com/watch?v=1-ca5qKivpY)
- https://twitter.com/MozillaHubs/status/1587429257453735937
- https://twitter.com/MozillaHubs/status/1588516422568869894

Selective bloom effect:

- [Using custom props in blender on meshes to use the bloom effect on them](https://www.youtube.com/watch?v=er02gtD8asA) or [using emission strength to go >1.0](https://twitter.com/AshConnell/status/1625242297410678784)
- There are also some tips on this issue: https://github.com/pmndrs/postprocessing/issues/496
