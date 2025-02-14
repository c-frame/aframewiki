# Controls

How does your user interact with your app?

First off, if you haven't read [the A-Frame docs on the topic](https://aframe.io/docs/master/introduction/interactions-and-controllers.html), you should.

Broadly, we'll divide this into movement and interaction.

## Spec

Originally, in the WebVR spec, VR gamepads were put as a derivative under the HTML5 gamepad API. When the WebVR spec was reorganized into the WebXR spec, VR controllers were put into an entirely new category with their own API, which you can read about here.

## Movement

Classically there are a few go-to options:

### 6dof tracking

This 'just works' if you have a 6dof headset, but the user will be limited to the size of their space if you don't add in other options.

### Thumbstick Motion ("Smooth Locomotion")

This probably 'just works' with the built-in `wasd-controls` component, but I need to verify that.
The standard has been [aframe-extras movement-controls component](https://github.com/c-frame/aframe-extras/tree/master/src/controls). A newer option is [aframe-locomotion](https://github.com/mrxz/aframe-locomotion).

### Thumbstick Rotation ("Snap Turning")

Classically this is an 'advanced VR user' control scheme only--it's uncomfortable for most users and can induce nausea if you control the camera in some way other than head movement.

That said, it's become common in 'advanced VR' apps to have 'snap turning'. The aframe-extras movement-controls (gamepad-controls) doesn't implement it. The [Blink Controls](https://jure.github.io/aframe-blink-controls/) component implements it. You could also implement it yourself by dissecting that library or by listening to [thumbstick events](https://aframe.io/docs/master/introduction/interactions-and-controllers.html#listening-for-button-and-axis-events). There's also [aframe-locomotion](https://github.com/mrxz/aframe-locomotion), which adds comfort features like vignette when moving.

### Teleportation

There may be more than one good teleportation library in A-Frame land, but as far as I know the most recently updated one is [Blink Controls](https://jure.github.io/aframe-blink-controls/).

### Flying

This can be implemented via the thumbstick with [aframe-extras movement-controls component](https://github.com/c-frame/aframe-extras/tree/master/src/controls). Manipulating the guide stick of a glider is done in [Elfland Glider](https://dougreeder.github.io/elfland-glider/city/)

### Climbing

I don't know of anyone who has implemented this in A-Frame yet.

### Via Hand Tracking API

How do you move when you don't have a joystick? Good question. [This library](https://github.com/gftruj/aframe-hand-tracking-controls-extras/tree/master/components) supports teleportation using Hand Tracking gestures.

## Navmesh

If you want your character to not walk through walls, you'll probably want to use a navmesh. [Here](https://github.com/AdaRoseCannon/aframe-xr-boilerplate/blob/glitch/simple-navmesh-constraint.js) you'll find a reference to a current approach and also the nav-mesh component in aframe-extras.

## Interaction

In general, you're going to be listening for events. That's described in the docs [here](https://aframe.io/docs/master/introduction/interactions-and-controllers.html#listening-for-button-and-axis-events).

### A-Frame Built-ins

A-Frame comes with pretty great 3dof and 6dof controller support out of the box. You should read [their documentation](https://aframe.io/docs/master/introduction/interactions-and-controllers.html#vr-controllers) if you haven't. By default it will inject a model that matches your controller into space, and listen for all the correct events.

### Laser Controls

An extremely common way of interacting with content is using [laser controls](https://aframe.io/docs/master/introduction/interactions-and-controllers.html#adding-laser-interactions-for-controllers). This handles 3dof and 6dof control schemes equally well, if that matters for you.

### Grabbing Things

If you're going to be grabbing things, stretching things, or throwing things, you'll probably want to look into the [Super Hands](https://github.com/c-frame/aframe-super-hands-component) component.

A guide on making things able-to-be-grabbed in your app can be found [here](https://github.com/c-frame/aframe-super-hands-component/issues/188).
