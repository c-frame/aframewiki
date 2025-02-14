# Physics

There are a few different options for physics in A-Frame. This article is intended to help you to choose between them, and offers more info and usage examples for all the various options.

## Picking an engine

Which engine you pick will depend a lot on your specific requirements. Currently there are 3 options for A-Frame physics that may be worth considering:

- [a-frame-physics-system](https://github.com/c-frame/aframe-physics-system) with Cannon driver
- [a-frame-physics-system](https://github.com/c-frame/aframe-physics-system) with Ammo driver
- [physx](https://github.com/c-frame/physx) (which uses Nvidia PhysX as its driver). Note that there is no current plan to integrate PhysX into aframe-physics-system, but it may be a better choice for some projects.

Since each driver has a slightly different component interface and schema, it will require some significant updates to your code to switch from one driver to another, so it's worth taking some time up-front to consider which driver is most likely to suit your needs.

At a high level:

- The Cannon driver is the easiest to use, but as a native JavaScript solution, it has the worst performance
- The Ammo driver is harder to use, but has significantly better performance
  - Ammo is a WASM build of the Bullet physics engine, which is a widely used open source physics engine.
- PhysX has the best performance (approx. 2x faster than Ammo.js), and is easy to use for simple use cases, but is more of an unknown quantity in terms of integration with A-Frame for more complex functions like constraints and APIs
  - The PhysX physics engine is the default physics engine used by both Unity and Unreal Engine.

For each of these drivers, there is the potential for specific limitations that could be problematic. These could be limitations

- in the physics engine itself
- in the version of the physics engine being used (which may not be the latest version)
- or, in the integration of the phsyics engine with aframe-physics-system (or physx).

See "Driver-specific limitations" below for a list of known driver-specific limitations.

## Driver-specific limitations

This is a list of limitations that has been observed with particular drivers (and also with physx). It's intended to provide a checklist to help developers to choose between physics drivers for a particular project, so they don't pick a driver that turns out to be missing some feature that is fundamental for their application.

This list is probably incomplete, so if you find an additional significant limitation, please add it to this list.

### Cannon.js

- Can't handle collision with fast moving bodies, as it does not offer Continuous Collision Detection (CCD)
- No support for collision filtering - all object pairs
- Restitution (bounciness) is a global property, rather than per-body
- No stateful collision data - just a stream of events that starts when collision is happening, and stops when collision ends (compare Ammo, which offers distinct collide-start, collide-end events and collision state that can be queried at any time; no idea yet what PhysX offers here...)

### Ammo.js

- No support for off-center attachment of constraints to bodies (integration issue?)

- No support for slider constraint (integration issue?)

### physx

- Very few examples - a gap that needs filling!
- Other limitations not known - suspect few limitations in the engine itself, but potentially many in terms of integration.

## aframe-physics-system

[aframe-physics-system](https://github.com/c-frame/aframe-physics-system) is the de-facto physics library of A-Frame. It supports both [ammo.js](https://github.com/kripken/ammo.js/) and [cannon.js](http://schteppe.github.io/cannon.js/) under the hood.

[ammo](https://github.com/kripken/ammo.js/) stands for "Avoided Making My Own js physics engine by compiling [bullet](https://pybullet.org/wordpress/) from C++". So, as you might guess, it's a high performance engine that is arguably AAA quality. It is implemented in WASM with a JS interface.

[cannon](https://github.com/pmndrs/cannon-es) is a physics library built natively in javascript that is considered the easiest to implement and the most well documented.

- [Here's an article](https://medium.com/samsung-internet-dev/game-physics-on-the-web-in-aframe-628fbf7c32a3) by [Ada](https://twitter.com/adarosecannon) and a small demo project describing and showing how ammo can be used with aframe-physics-system.
- [Using Ammo with aframe-physics-system](https://github.com/c-frame/aframe-physics-system/blob/master/AmmoDriver.md)

### Examples that use aframe-physics-system:

[Christmas Scene](https://diarmidmackenzie.github.io/christmas-scene/) (Ammo)

## PhysX-js

The A-Frame PhysX implementation was something put together by Zach Capalbo for his Vartiste project. While PhysX-js should be the most performant option on machines with a high end GPU, the integration with A-Frame is still early in development.

If you are starting today, this is now the recommended repo for A-Frame physX:
[C-Frame physx](https://github.com/c-frame/physx)

Compare PhysX vs. Ammo performance [here](https://c-frame.github.io/physx/examples/pinboard/ammo-vs-physx.html)

### Examples that use the same base code (pulled from different sources):

- [Stemkoski's demo](https://stemkoski.github.io/A-Frame-Examples/quest-physics.html)
- [Ada's XR Starter Kit](https://github.com/AdaRoseCannon/aframe-xr-boilerplate)

### Author's demo:

- [Zach's demo 1](https://codepen.io/zach-capalbo/pen/Pobeppd?editors=1000)
- [Zach's physics playground](https://fascinated-hip-period.glitch.me/)
- [Import physics from blender in your glb](https://twitter.com/zach_geek/status/1370198868323934209)

[Documentation](https://vartiste.xyz/docs.html#physics.js)

[More about PhysX engine](https://en.wikipedia.org/wiki/PhysX)

## physics-lite

There is a basic physics implementation that can be seen working with 1.3.0 [here](https://glitch.com/~physics-lite) that, althought it does not appear to be actively maintained, is still working. This was a random discovery, more info welcome on its use. Source [here](https://github.com/disasteroftheuniverse/SuperQuest).

## Physics in 1.2.0

Note: We have now working versions of cannon and ammo starting in A-Frame 1.4.1 with [c-frame/aframe-physics-system](https://github.com/c-frame/aframe-physics-system).

Physics has been heavily affected in A-Frame 1.2.0 with the THREE Geometry deprecation which broke a number of packages. [There is an unofficial fork that seems to be working well with 1.2.0 here](https://github.com/gearcoded/aframe-physics-system/blob/master/dist/aframe-physics-system.js). ([source](https://github.com/n5ro/aframe-physics-system/issues/187#issuecomment-792048570)).

aframe-physics system in the past relied on **cannon** instead of **ammo**.

[THREE source](https://github.com/donmccurdy/three-to-cannon#api)
Found in [A-Frame Physics System](https://github.com/n5ro/aframe-physics-system)

Easy to use, well documented, but performance may be an issue in some cases. Can produce very good results. Cannon engine itself is generally regarded as having superior documentation.

[Discussion on problems with 1.2.0 and aframe-physics-system](https://github.com/n5ro/aframe-physics-system/issues/187)
[A fork of a-frame physics system supposedly working with a-frame 1.2.0](https://github.com/n5ro/aframe-physics-system/issues/187#issuecomment-792048570)
[Ongoing pull request and work on updating it in physics-system](https://github.com/n5ro/aframe-physics-system/issues/189)

A good overview of physics engine being discussed in the THREE wiki is [here](https://discourse.threejs.org/t/preferred-physics-engine-cannon-js-ammo-js-diy/1565/9).

There is also the option of using the AMMO driver instead of the CANNON driver. On that topic, here's a good quick pointer to some links from a pull request on [Networked A-Frame](https://github.com/networked-aframe/networked-aframe/pull/270):

> There is a more complex example by @diarmidmackenzie using a [modified version of super-hands](https://github.com/diarmidmackenzie/aframe-super-hands-component) compatible with the ammo driver at https://black-and-white-friends.surge.sh/pages/
> See also
> https://github.com/diarmidmackenzie/aframe-super-hands-component/blob/master/examples/physics/index-ammo.html that is available at https://terrific-minute.surge.sh/examples/physics/index-ammo.html
> (link taken from the [discussion about the different super-hands forks with ammo compatibility](https://aframevr.slack.com/archives/C3WGUL4K0/p1614673717007000))

- [A mildly dated walkthrough demo](https://kellylougheed.medium.com/make-a-webvr-ball-pit-with-a-frame-physics-bce2d40557d7)
