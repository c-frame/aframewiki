# Environment

Setting the scene around you is a feature of most projects. Here's some thoughts on doing so.

## Quick Start

Here's two packages that auto-generate environments for you, with tweakable params:

### aframe-environment-component

Source: https://github.com/supermedium/aframe-environment-component
This is a classic from the early days that gives a wide range of vibes with a lot of tweakable params. Most quick demos you see rely on this package. High performance, and extremely easy to integrate. Two lines of HTML, and you have a stage. Developed by core contributors at supermedium.

### aframe-enviropacks

Source: https://www.npmjs.com/package/aframe-enviropacks
A more recent contribution featuring more attractive textures. Haven't worked with it yet, but a lot less 'basic' looking than the OG.

## Sky

Most projects will want some kind of sky.

## Full Sky System

### A-Starry-Sky

[source](https://github.com/Dante83/A-Starry-Sky)
(see there for examples)
A GPU intense, but incredibly accurate and beautiful sky system.

### a-super-sky

[source](https://github.com/kylebakerio/a-super-sky)
[example](https://glitch.com/edit/#!/a-super-sky-demo?path=index.html%3A1%3A0)
Based on the sun-sky shader, as well as code pulled and integrated from the aframe-environment component, this repo gives an out-of-the-box **_animated_** sky with sun, moon, stars, and a mixture of shadow-casting sun and ambient lighting with color and intensity varying accordingly.

### supermedium's sun-sky component/shader

[source](https://github.com/supermedium/superframe/tree/master/components/sun-sky/)
[variation](https://github.com/aframevr/aframe/blob/b164623dfa0d2548158f4b7da06157497cd4ea29/examples/test/shaders/shaders/sky.js) in the aframe repository's examples that may be an updated version. This version seems to be identical to that used in the aframe-environment-component as well.
[example](https://supermedium.com/superframe/components/sun-sky/)
Classic project with beautiful visuals and high performance. Canonical examples are using older versions of A-Frame, but there are no real compatibility issues with newer versions. The animated example is currently incompatible with modern a-frame, which led to the development of a-super-sky; note that this sky shader lives on in the aframe-environment-component

## Just sky

### background

[documentation](https://aframe.io/docs/1.7.0/components/background.html)
Sets a basic color background.

### a-sky

[documentation](https://aframe.io/docs/1.7.0/primitives/a-sky.html) &nbsp;
A large sphere with a background color or equirectangular 360° image.

### simple-sun-sky

[source](https://github.com/DougReeder/aframe-simple-sun-sky) &nbsp;
[example](https://dougreeder.github.io/aframe-simple-sun-sky/example.html) &nbsp;
Lightweight, simple gradient + sun. Includes directional light source.

### a-sky-background

[source](https://github.com/mrxz/fern-aframe-components/tree/main/sky-background) &nbsp;
[npm](https://www.npmjs.com/package/@fern-solutions/aframe-sky-background) &nbsp;
[example](https://aframe-components.fern.solutions/sky-background/) &nbsp;
A gradient or an equirectangular skybox that renders a fullscreen triangle covering the far plane.

## Just stars

### aframe-star-system-component

[source](https://github.com/handeyeco/aframe-star-system-component)
Works for pre A-Frame 1.2.0. To get the same effect > 1.2.0, see the example of stars used in `a-super-sky` or `aframe-environment-component`.

## Terrain

## Land

### Mountain

[source](https://github.com/supermedium/superframe/tree/master/components/mountain/) &nbsp;
[npm](https://www.npmjs.com/package/aframe-mountain-component) &nbsp;
Uses Perlin noise to create a height map, create a shaded texture from that height map using a `<canvas>`, and using the height map to create vertices on a BufferGeometry.

### a-atoll-terrain

[source](https://github.com/DougReeder/aframe-atoll-terrain) &nbsp;
[npm](https://www.npmjs.com/package/aframe-atoll-terrain) &nbsp;
[example](https://dougreeder.github.io/aframe-atoll-terrain/example.html) — reload to see variations.

A circle of high-resolution Perlin noise terrain near the origin, surrounded by a low-resolution sea or plain that stretches to the horizon.
Optionally, the high-resolution area can include a central plateau or sea.
Hillsides opposite the sun are in shadow.

## Water

### a-ocean

[source](https://github.com/c-frame/aframe-extras/tree/master/src/primitives) &nbsp;

### ada-water

[source](https://github.com/kylebakerio/ada-water) &nbsp;
[Example](https://ada-water.glitch.me/) &nbsp;
[original article](https://medium.com/samsung-internet-dev/generating-a-water-effect-part-1-svg-and-canvas-2ad07060cc0d) &nbsp;
A very cheap large ocean based on the shader tutorial by Ada Rose.

### a-water

[source](https://github.com/Dante83/a-water) &nbsp;
[example](https://code-panda.com/pages/projects/a_ocean/v_0_1_0) &nbsp;
An infinite procedural ocean with animated rolling waves. Requires a powerful GPU.

## Buildings

### aframe-shader-buildings

[source](https://github.com/DougReeder/aframe-shader-buildings)
Add a city's worth full of low-poly buildings, cheap!
Now supports textures and box texture for windows, and updated for compatibility with A-Frame 1.6.0.

## Fog / Dust / Rain

### fog

[documentation](https://aframe.io/docs/1.7.0/components/fog.html) &nbsp;
The built-in fog component.

### fix-fog

[source](https://github.com/mrxz/fern-aframe-components/tree/main/fix-fog) &nbsp;
[example](https://aframe-components.fern.solutions/fix-fog/) &nbsp;
Improves the fog effect in VR, most noticeably in case of dense fog.

### a-dust

[source](https://github.com/DougReeder/aframe-dust-component) &nbsp;
[npm](https://www.npmjs.com/package/aframe-dust-component) &nbsp;
[example](https://dougreeder.github.io/aframe-dust-component/example.html) &nbsp;
Provides visual feedback on the user's motion, which is useful when flying or moving in unearthly spaces.
Also adds atmosphere — pink fairy lights for a paradise, black ash for a hellscape.

### particle-system rain preset

[source](https://github.com/c-frame/aframe-particle-system-component) &nbsp;
[example](https://c-frame.github.io/aframe-particle-system-component/examples/rain/) &nbsp;

## Environmental sound

### sound

[documentation](https://aframe.io/docs/1.7.0/components/sound.html) &nbsp;
See [the FAQ](https://aframe.io/docs/1.7.0/introduction/faq.html#where-can-i-find-assets) to find sources for environmental sound.
