# Development

## dev components

- [in-VR stats](https://github.com/kylebakerio/vr-super-stats)
- [in-VR console](https://github.com/kylebakerio/a-console)
- [controller log data](https://stemkoski.github.io/A-Frame-Examples/quest-controllers.html)
- [log-all-events](https://github.com/kylebakerio/log-all-events/)
- [debug-cursor](https://github.com/supermedium/superframe/tree/master/components/debug-cursor/)
- [desktop-vr-controller](https://diarmidmackenzie.github.io/aframe-components/docs/desktop-vr-controller.html)

## debugging tools

- [aframe-inspector](https://github.com/aframevr/aframe-inspector)
- [three debugger](https://github.com/oslabs-beta/BACE#readme)
- [cannon debugger](https://github.com/pmndrs/cannon-es-debugger)

### Emulating different VR devices in your browser

For Quest 2, the latest option is [The WebXR Quest Emulator](https://github.com/meta-quest/immersive-web-emulator).

For more broad usage, the [WebXR Emulator Extension](https://chrome.google.com/webstore/detail/webxr-api-emulator/mjddjgeghkdijejnciaefnkjmkafnnje?hl=en) is a great resource, but as of this writing the canonical version in the store is fairly out of date. However, you can get a modern version with recent models supported by locally installing the version from [this pull request](https://github.com/MozillaReality/WebXR-emulator-extension/pull/278).
_@vincentfretin on slack:_

> ```bash
> git clone git@github.com:fe1ixz/WebXR-emulator-extension.git
> cd WebXR-emulator-extension
> npm install
> npm run build
> ```
>
> in chrome, in extensions, click on the "load non packaged extension" button and select the WebXR-emulator-extension folder

## Opening an url on your headset

### hmd.link

For all headsets, you can copy and paste an url on [hmd.link](https://hmd.link), then on the headset access hmd.link and click.

You can add a bookmarklet to easily use the service, see
[article](https://timmykokke.com/blog/2023-10-29-hmd.link-bookmarklet/).

```js
javascript:(function()%7Bwindow.location.href%20%3D%20%22https%3A%2F%2Fhmd.link%3F%22%2Bwindow.location.href%7D)()
```

### Meta Quest Web Launch

For Meta headsets, see [Meta Quest: Use Web Launch to Send Links to Headsets from the Web](https://developer.oculus.com/documentation/web/web-launch/)

You can create a bookmarklet in your browser to open on Quest:

```js
javascript:(function()%7Blet sendToQuestUrl=new URL("https://oculus.com/open_url/");sendToQuestUrl.searchParams.set("url",window.location),window.location.href=sendToQuestUrl;%7D());
```

For this create a bookmark and modify the URL field with the code above.

### With adb during development

If you use adb during development, you can create a npm script to open your
project like this in `package.json`:

```json
"open-quest": "C:\\Tools\\Android\\platform-tools\\adb.exe shell am start -a android.intent.action.VIEW -d 'http://localhost:8080'"
```

and run `npm run open-quest` to open the url on your Quest.

## HTTPS

WebXR can only run over HTTPS.
It's super -easy to run a local HTTP server using node.js (http-server) or python (python -m http.server), but not quite so easy to set up an HTTPS server.
Here are some of handy solutions (more details of some of these below)

- Develop in the cloud, using a platform like glitch.com
- Develop locally, and set up an HTTPS tunnel to your local PC using ngrok
- Develop locally, but push your code to the cloud using a service like surge.sh.
- Run your own web server on a Raspberry Pi
- Use adb-reverse, so that your VR device thinks it is talking to localhost. See: https://medium.com/@lazerwalker/how-to-easily-test-your-webvr-and-webxr-projects-locally-on-your-oculus-quest-eec26a03b7ee

## Glitch.com

Great for starting out--lets you code and immediately serve the code live, making it easy to see updates instantly and easy to test multiuser features, as well as easy to launch on your oculus quest.
This is also the preferred platform for requesting help. Making a glitch makes it easy for others to see if there's a simple solution and view the errors themselves, and run local experiments for potential fixes, with the minimum amount of work. This means you're more likely to get help.

### Basic workflow (in-browser dev)

- Go to glitch.com
- start a new project
- 'view in new tab'
- profit

### Advanced workflow (local dev)

Glitch also integrates with github, making it possible to upload assets, and to develop locally and push your changes up as desired.

- connect glitch to your github/gitlab
- clone down from glitch as a remote itself
- make a branch, make changes, push to glitch remote
- open terminal in glitch site, merge in branch to master
- run 'refresh' in the glitch terminal
- profit
  ([Source](https://support.glitch.com/t/possible-to-code-locally-and-push-to-glitch-with-git/2704/2))
  This workflow is pretty seamless after your first pass through.

## Alternative Home Raspberry Pi Development Environment

As an alternative to using Glitch or a paid server in the cloud, or in addition to it. It is possible to develop, test and host on a Raspberry Pi Model 4B 8GB on your own home network for what you are already paying your internet service provider monthly (hence essentially free, minus cost of Pi hardware). Requires opening ports on your router for outside internet access (sometimes a little tricky, depending on your networks setup).

- [Long and detailed article explaining how](https://michael-mcanally.medium.com/setting-up-a-raspberry-pi-as-a-home-metaverse-server-for-your-vr-headset-12632ac1b871)

## ngrok

nrgok (https://ngrok.com/) is a really useful tool that can set up a publicly accessible HTTPS tunnel to an HTTP port on your local system.
So you can just server pages at http://localhost:8000 (or whatever), and use ngrok to make this available over HTTPS.
Only problem with ngrok is that the URLs are not easy to type in. Solutions:

- if you use the paid ngrok service, you can use custom subdomains
- use a URL shortener service to make easy-to-memorize urls
- send the url via FB to on a chat on desktop, then open the link from chat in the headset
- Quest 2 only: use the Oculus Developer Hub app on Windows, connect the Quest with a USB cable, and copy/paste the ngrok URL into the Oculus Developer Hub page, where you can force the headset to open a browser page.
  ngrok URLs last for a few hours (longer on the paid version), and once you have the URL in the browser history, you can go unthethered (unless you need tethering for debugging).
  ngrok and surge are both free for the basic service (which is good enough for me).

## surge.sh, netlify

surge.sh (https://surge.sh/) is a simple utility, with a free version, that can be used to temporarily publish a website in the cloud, where it can be retrieved over HTTPS.
If you want to push client-side only stuff to a custom subdomain, for free and with great minimalist tooling, it's a great option.
Netlify is another similar product.

## Dealing with Quest Browser Caching

_kylebakerio_ on slack:

> Just wanted to mention, I've found for me the lowest friction option for cache busting on the oculus quest 2 has been using nrgok on a local server and then putting my ngrok url through a free url shortener and memorizing the shorturl. So far I prefer shorturl.at for giving me the easiest urls to memorize. I just do that once per day, basically, though I can always do it again to really kill the cache if I have some problem.
> Of course, getting the adb wifi bridge going accomplishes this task as well, but is just a bit more friction. When I finally decide I really need to see the logs, I take a minute and set that up, and it isn't bad either.
> Just thought I'd follow up.
> _bai_ on slack:
> If you connect your quest to your desktop/laptop via USB or using adb wifi, you can inspect the remote tab via dev tools, and also set it to disable cache while dev tools is open

### More topics to be covered:

- Using ADB with your quest
- [Build VR in VR](https://github.com/supermedium/aframe-super-shooter-kit), a project that may have fallen behind on maintenance but is a fascinating reference demo and possibly still useful. [video](https://www.youtube.com/watch?v=RW3enib2X94)
