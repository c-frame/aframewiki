# Augmented Reality

A little bit real, and a little bit beyond.

WebVR became WebXR as it was realized that the future wasn't fully virtual. Currently less utilized, but AR in the browser is also a thing, and it's also a thing with A-Frame.

The canonical demo is [this one](https://immersive-web.github.io/webxr-samples/immersive-ar-session.html?usePolyfill=0) from [the webxr samples](https://immersive-web.github.io/webxr-samples/) [source](https://github.com/immersive-web/webxr-samples/tree/master). I think [this](/augmented-reality)https://medium.com/naver-fe-platform/webar-with-webxr-api-part-2-dc76b20767fb) is an explainer related to that demo.

You can read an article that's fairly recent and gives some history on the topic of AR in WebXR [here](https://medium.com/naver-fe-platform/webar-with-webxr-api-part-1-e191a2dc7177).

You can see a quick example of AR on the web if you google Rottweiler on your phone--just click 'view in 3d'.
<br/>

<div>
<!-- <img src="/whatsapp_image_2021-02-07_at_01.57.54.jpeg" style="max-height: 600px;" />
<img src="/whatsapp_image_2021-02-07_at_23.30.02.jpeg" style="max-height: 600px;" /> -->
</div>

## A-Frame Built-In

A-Frame comes with AR support built in that works out-of-the-box with ARcore compatible devices. You can read about it [here](https://aframe.io/blog/webxr-ar-module/), and see a demo with A-Frame 1.0.3+ [here](https://glitch.com/edit/#!/xr-spinosaurus).

You can see an updated demo with 1.3.0+ that shows real time lighting [here](https://medium.com/samsung-internet-dev/use-new-augmented-reality-features-with-just-a-few-lines-of-code-with-webxr-and-aframe-c6f3f5789345)!

## AR.js

([github](https://github.com/AR-js-org/AR.js)) ([docs](https://ar-js-org.github.io/AR.js-Docs/))
AR.js is the go-to open source framework option. It also is [officially compatible with A-Frame](https://aframe.io/blog/arjs3/), and has been [for a while](https://aframe.io/blog/arjs/).

It features tracking using a traditional marker, using an image _as_ a marker, or use gps coordinates as a marker. According to articles from 2017, it also supports markerless tracking for [tango compatible phones](<https://en.wikipedia.org/wiki/Tango_(platform)>) and certain iOS devices ([1](https://medium.com/arjs/ar-js-supports-tango-on-a-frame-too-2c098de4df34) [2](https://medium.com/arjs/announcing-tango-support-for-ar-js-373572fec69e)). However, tango is outdated tech--these days, no special hardware is necessary for ARcore and ARkit, and as seen in the these demos, markerless AR on relatively modern smart phones should be possible on Chrome and in iOS. The limitation is likely that the tracking is currently done by [another library](https://github.com/artoolkitx/jsartoolkit5) that hasn't added those features. It also looks like AR.js might not be so keen to move towards ARCore/ARKit tech because those are proprietary tech from Google/Apple that e.g. Firefox won't be able to implement, as far as I understand it.

Sidenote: it's notable that in theory, 3-dof psuedo AR is possible by just projecting a VR scene with camera background. There will be no hit-testing, no accurate lock onto the ground or surfaces, and no scaling. But as you tilt the phone left/right/up/down in place, it will effectively work as AR. This can be an easy, highly compatible, relatively straightforward way to implement simple AR concepts in certain limited use cases.

#### Resources

- [Walkthrough](https://ar-js-org.github.io/AR.js-Docs/#getting-started) to get a hello-world running of AR.js + A-Frame.
- [Docs](https://ar-js-org.github.io/AR.js-Docs/)
- [Tutorials](https://ar-js-org.github.io/AR.js-Docs/#tutorials) featured in the above docs, which currently include [build a location-based app](https://medium.com/chialab-open-source/build-your-location-based-augmented-reality-web-app-c2442e716564) and [build a peak-finder](https://ar-js-org.github.io/AR.js-Docs/location-based-tutorial/).

#### Community

I'm still looking for it. You can check the **AR** channel on the A-Frame slack--not a super active channel, but it's at least something, and people at least hang out in the general slack itself. AR.js's github lists the link to a [Gitter Chatroom](https://gitter.im/AR-js/Lobby) as of this writing, but it seems to not be very active. They advise asking questions on [stack overflow](https://stackoverflow.com/search?q=ar.js), and reporting issues and feature requests on their github.

## A-Frame AR

This is a tiny, relatively unknown demo library, but it does seem to accomplish what may be most desired--simple integration of 6dof AR utilizing the WebXR spec with A-Frame. I haven't seen anyone talk about this other than one of the authors on his [blog post](https://jsantell.com/web-ar-prototypes/), but the demos appear to be working.

## MindAR.js

MindAR is an opensource web augmented reality library which is compatible with A-Frame and Three.Js. It has solid supports Image Tracking and Face Tracking. To learn more visit its [GitHub page](https://github.com/hiukim/mind-ar-js).

## Martins.js

MARTINS.js is a GPU-accelerated Augmented Reality engine for the web, which is compatible with Chrome, Edge, Firefox and Opera browsers. It supports mainly image tracking functionality. To learn more visit its [GitHub page](https://github.com/alemart/martins-js).

## SimpleAR

New open-source web-based augmented reality library, which is based on WebAssembly(WASM). It supports image tracking without pre-training stage. it is compatible with major web frameworks such as A-Frame, Three.js and Babylon.js. It is currently under active development. To learn more visit its [GitHub page](https://github.com/akbartus/Simple-AR).

## Model Viewer

For many simple cases (especially store-based concepts), the [Model Viewer library](https://modelviewer.dev/docs/#augmentedreality-attributes) will be a good solution to placing a model in AR. It falls back well on iOS devices.

## 8th Wall

8th wall features 6dof markerless tracking / SLAM. But it's paid proprietary closed-source software. I don't know much about it beyond that, but it's often considered the premium option for some commercial use cases. Others can add more info here.

## DIY

Given that there's no well developed free open source framework that seems to leverage ARcore/ARkit right now, it's the wild west. Here are some sources you might draw from:

- [Example](https://github.com/boehm-e/webxr_threejs_AR) of an ARCore + Three.js AR diy demo from 2020.
- [Experiments documented by Google](https://experiments.withgoogle.com/collection/ar), not sure if these are web based or not.
- [WebXR AR Experiments by Google](https://blog.google/products/google-ar-vr/webxr-experiments/), these seem more recent and are WebXR

## iOS

You'll need to download the WebXR viewer at this stage, although rumors based on Apple's jobs listings suggest that they may be adding webXR support in the future.

## ARcore

- [Google's overview site](https://arvr.google.com/ar/)
- [SDK's from Google](https://developers.google.com/ar)
- [Supported devices](https://developers.google.com/ar/discover/supported-devices)

## Learning

- Google has [a free course/codelab](https://codelabs.developers.google.com/codelabs/ar-with-webxr#0) on AR in WebXR. It isn't related to A-Frame, as far as I know. It's from October 2020. [Here](https://developers.google.com/web/updates/2018/06/ar-for-the-web) is an article from them that references it from 2018, though.
- Coursera has [a free course](<](https://www.coursera.org/lecture/develop-augmented-virtual-mixed-extended-reality-applications-webxr-unity-unreal/marker-less-ar-with-webxr-viG8C)>) on the topic of AR (includes webXR, but also unity and unreal).
- Babylon.js is an alternative to A-Frame (WebGL wrapper that doesn't rely on THREE.js), and [this is their page](https://doc.babylonjs.com/divingDeeper/webXR/webXRARFeatures) on the topic.
- [An old walkthrough](https://urish.medium.com/web-powered-augmented-reality-a-hands-on-tutorial-9e6a882e323e) on implementing an AR in A-Frame demo with cross compatibility in 2018.

## Discussion

- An [overview of AR](https://www.marxentlabs.com/what-is-markerless-augmented-reality-dead-reckoning/) from an industry personality (Oct 2020)
- An [article](https://hacks.mozilla.org/2019/01/augmented-reality-and-the-browser%E2%80%8A-%E2%80%8Aan-app-experiment/) covering some thoughts on designing AR experiences.
