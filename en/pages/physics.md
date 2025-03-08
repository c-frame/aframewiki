# Physics

There are a few different options for physics in A-Frame.
See [Picking an engine](https://c-frame.github.io/aframe-physics-system/#picking-an-engine)

## aframe-physics-system

[aframe-physics-system](https://github.com/c-frame/aframe-physics-system) is the de-facto physics library of A-Frame. It supports both [ammo.js](https://github.com/kripken/ammo.js/) and [cannon.js](https://github.com/pmndrs/cannon-es) under the hood.

[ammo](https://github.com/kripken/ammo.js/) stands for "Avoided Making My Own js physics engine by compiling [bullet](https://pybullet.org/wordpress/) from C++". So, as you might guess, it's a high performance engine that is arguably AAA quality. It is implemented in WASM with a JS interface.

[cannon](https://github.com/pmndrs/cannon-es) is a physics library built natively in javascript that is considered the easiest to implement and the most well documented.

- [Here's an article](https://medium.com/samsung-internet-dev/game-physics-on-the-web-in-aframe-628fbf7c32a3) by [Ada](https://twitter.com/adarosecannon) and a small demo project describing and showing how ammo can be used with aframe-physics-system.
- [Using Ammo with aframe-physics-system](https://github.com/c-frame/aframe-physics-system/blob/master/AmmoDriver.md)

### Examples that use aframe-physics-system:

[Christmas Scene](https://diarmidmackenzie.github.io/christmas-scene/) (Ammo)

## PhysX

[C-Frame physx](https://github.com/c-frame/physx)

Compare PhysX vs. Ammo performance [here](https://c-frame.github.io/physx/examples/pinboard/ammo-vs-physx.html)

### Examples that use the same base code

- [Ada's XR Starter Kit](https://github.com/AdaRoseCannon/aframe-xr-boilerplate)

### Author's demo:

- [Zach's demo 1](https://codepen.io/zach-capalbo/pen/Pobeppd?editors=1000)
- [Zach's physics playground](https://fascinated-hip-period.glitch.me/)
- [Import physics from blender in your glb](https://twitter.com/zach_geek/status/1370198868323934209)

[More about PhysX engine](https://en.wikipedia.org/wiki/PhysX)

## physics-lite

There is a basic physics implementation that can be seen working with 1.3.0 [here](https://glitch.com/~physics-lite) that, althought it does not appear to be actively maintained, is still working. This was a random discovery, more info welcome on its use. Source [here](https://github.com/disasteroftheuniverse/SuperQuest).

## Other links

A good overview of physics engine being discussed in the THREE wiki is [here](https://discourse.threejs.org/t/preferred-physics-engine-cannon-js-ammo-js-diy/1565/9).

There is also the option of using the AMMO driver instead of the CANNON driver. On that topic, here's a good quick pointer to some links from a pull request on [Networked A-Frame](https://github.com/networked-aframe/networked-aframe/pull/270):

> There is a more complex example by @diarmidmackenzie using a [modified version of super-hands](https://github.com/diarmidmackenzie/aframe-super-hands-component) compatible with the ammo driver at https://black-and-white-friends.surge.sh/pages/
> See also
> https://github.com/diarmidmackenzie/aframe-super-hands-component/blob/master/examples/physics/index-ammo.html that is available at https://terrific-minute.surge.sh/examples/physics/index-ammo.html

- [A mildly dated walkthrough demo](https://kellylougheed.medium.com/make-a-webvr-ball-pit-with-a-frame-physics-bce2d40557d7)
