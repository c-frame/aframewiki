# Do Open Source!

A-Frame is intentionally a small, focused library that does its core mission with a few core utilities, to make it maintainable and give it a small footprint. It relies on open-source community components to make it a full-featured ecosystem. That's where you come in!
WebXR, and WebGL, are fast-moving, cutting edge frontiers of the web. A-Frame is also based on THREE, which is an aggressively moving target. As a result, components that aren't maintained, often fall behind and break. Most recently, A-Frame 1.2.0, because it tracks forward with THREE updates, saw many packages fail because of an performance improvement focused deprecation of its core `Geometry` in favor of `GeometryBuffer`.
**This page attempts to highlight places in the A-Frame ecosystem where open-source contributions for updates and maintenance would help us all.**

- It's ideal to **file an issue**, and then **contribute a pull request**, as its existing traffic and presence on the web (e.g., its npm listing) will make it easier to find.
- If the maintainer doesn't accept it because of lack of maintanence, **the pull request and issue can point users who investigate the package towards your new fork**, which then becomes the de-facto home of the component. Maintain it or not, either way someone else can fork off of your work later if needed.
  Contributions like these stand out and get you noticed in the community, and can often be very low effort.

## A-Frame Core

- Docs always need updating! Just peruse the [A-Frame documentation](https://aframe.io/docs/master/introduction/), and contribute inline by clicking the "edit this page" button on the right, as you read, if you notice anything that has fallen behind.
- [A-Frame inspector](https://github.com/aframevr/aframe-inspector) is an incredible tool we all use constantly in our projects that has several console warnings and errors that show up, and has had a few features fall by the wayside. Some updates here would be a huge help everyone in the community.
- [A-Frame School](https://aframe.io/aframe-school/) is still a common first point of contact with A-Frame. While it's still extremely useful, a few parts have fallen slightly out of date--this gives a poor impression to newbies, so cleaning this up could be high leverage in bringing new coders into the fold. It seems the source for it can be found [here](https://github.com/aframevr/aframe-school/blob/master/content.md).

## Dev Tools

This is where we all get a huge return--help yourself immediately and everyone else by proxy

- [Geometry Merger](https://github.com/supermedium/superframe/issues/298) is an extremely useful project that needs 1.2.0 updates
- Many other [supermedium](https://github.com/supermedium/superframe) projects, take your pick.
- reviving [angle](https://github.com/ngokevin/angle) could be cool

## Networked A-Frame

The Go-To resource for multiplayer VR experiences. It's actually in a pretty good state right now.

- Check the [issues](https://github.com/networked-aframe/networked-aframe) page
- Create and update the examples further

## Other components

- Add your suggestions here!

# Three To A-Frame ideas

Another way to contribute is to convert existing THREE.js projects into A-Frame components for easy use. Here's some suggestions:

- [unreal bloom](https://threejs.org/examples/webgl_postprocessing_unreal_bloom.html)
- [surface sampling](https://tympanus.net/codrops/2021/08/31/surface-sampling-in-three-js/)
- [parallax height map?](https://techbrood.com/threejs/examples/webgl_materials_parallaxmap.html)
