# Postprocessing

As of version 1.4.1 (January 2023), A-Frame doesn't natively support three.js postprocessing, but there are ways to make it work.
Here's an [example](https://glitch.com/edit/#!/anaglyph-postprocessing?path=index.html) showing the anaglyph postprocessing effect that Diarmid did a little while ago.
Similar techniques will probably work for other postprocessing effects.

Really old [aframe-effects](https://github.com/wizgrav/aframe-effects) is also available (and seeking a maintainer);

Newer with a custom build of aframe 1.4.2 that has a threejs patch for postprocessing in VR: [Akbartus's A-Frame-Component-Postprocessing](https://github.com/akbartus/A-Frame-Component-Postprocessing)
As of three r172, it sill requires those changes https://github.com/supermedium/three.js/pull/20
See also postprocessing VR issue that tracks VR support https://github.com/pmndrs/postprocessing/issues/677

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

