# Multi-user experiences over the network

A-Frame, like the rest of WebXR, has no built-in synchronization between clients.
So, when two users attempt to grab an object at the same time, in general it's possible for one client to calculate that one user has grabbed the object first, while another client calculates a different user.

Different technologies have different tradeoffs, so begin with the end in mind.

## Technologies

### [Networked A-Frame (NAF)](https://github.com/networked-aframe/networked-aframe)

Straightforward synchronization of the attributes of A-Frame entities, over WebRTC and/or WebSockets.
Requires [running your own synchronization server](https://github.com/networked-aframe/networked-aframe/blob/master/docs/getting-started-local.md#setup-the-server).

Can do anything, but has a reputation for being tricky to get working the way you want it.

Supports multiple instances of a room configuration. Room system not updated to latest version.

### [Hubs](https://hubsfoundation.org/)

A complete shared virtual world implementation including voice and text chat, avatar selection, media playback, and (under development) Behavior Graphs allowing users to import interactive objects.
Good support for personal computers, mobile and headsets.
Content management tools (Spoke & Blender Exporter) allow building worlds without code.

Requires you to run your own server on a Kubernetes cluster (which can be managed by your provider).
Uses forked, ancient versions of A-Frame and NAF, so compatibility with 3rd-party components is far from guaranteed.

Supports multiple instances of a scene, and changing the scene of a room.

### [A-Frame Croquet Component](https://github.com/NikolaySuslov/aframe-croquet-component)

[Croquet OS](https://croquet.studio/docs/) is real-time synchronization-as-a-service (and not restricted to WebXR).
You don't run your own synchronization server; large traffic volumes would require payment.
It's designed around keeping state in its own Model objects, which have a number of restrictions so each client maintains a bit-identical simulation.
Web apps which keep all state and modeling in Croquet Models avoid the who-grabbed-it-first problem.
It keeps snapshots, so the state of a session is maintained, even after all users have gone off-line.
It has its own *Worldcore* library to handle the scene graph and rendering.
It also has a Unity interface in beta.

However, you don't have to use Worldcore, and can use the [A-Frame Croquet Component](https://github.com/NikolaySuslov/aframe-croquet-component) to synchronize the attributes of A-Frame entities.
Off-the-shelf A-Frame components maintain state internally, so don't gain the full advantage of Croquet OS.
(And if you're re-writing a component, you should consider using Worldcore.)

It's super-easy to [throw together a multi-user experience](https://github.com/NikolaySuslov/aframe-croquet-component#how-to-share-an-entity-in-an-a-frame-scene-with-other-users) (without voice chat), but as an immature library, you might have to code and submit a PR to get advanced capabilities.

Supports multiple instances of a room configuration.

### [Colyseus Multiplayer Framework](https://docs.colyseus.io/)

Has half-a-dozen SDKs, including JavaScript/TypeScript. Is used by a BabylonJS example.

Self-host it on your own servers for free, or use their commercial Colyseus Cloud service


## Comparison Table

|                            | aframe-croquet-component | networked-aframe                                                        | Colyseus      | Hubs w/ custom A-Frame client                                               |
|----------------------------|--------------------------|-------------------------------------------------------------------------|---------------|-----------------------------------------------------------------------------|
| server setup               | none                     | easy                                                                    | none or ?     | medium (Kubernetes cluster required)                                        |
| maintained                 | yes                      | yes for core and janus adapter, easyrtc adapter has longstanding issues | yes           | yes                                                                         |
| A-Frame versions           | 1.3.0 - 1.6.0            | 1.4.1                                                                   | ?             | forked, primitives removed, new features won't use A-Frame                  |
| code maturity              | immature                 | mature for core and janus adapter                                       | mature?       | mature                                                                      |
| instanced rooms            | yes                      | yes, but not updated to latest                                          | yes           | yes                                                                         |
| synced attributes          | position rotation scale  | by NAF schema                                                           | by JavaScript | by NAF schema                                                               |
| object mutation & deletion | anyone                   | owner                                                                   | by JavaScript | owner                                                                       |
| private sessions           | automatic                | manual, add auth to the easyrtc server and JWT for janus                | by JavaScript | automatic                                                                   |
| voice chat                 | no                       | yes                                                                     | no?           | yes                                                                         |
| user avatar selection      | no                       | no                                                                      | no            | yes                                                                         |
| custom network messages    | yes                      | yes                                                                     | ?             | yes                                                                         |
| state preserved w/o users  | yes                      | no                                                                      | ?             | no, but you can pin objects to keep them                                    |
| synced random seeds        | yes                      | no                                                                      | ?             | no                                                                          |
| example                    | https://xalot.surge.sh/  | https://naf-examples.glitch.me/                                         |               | https://hfk-virtual.space/ZBoNiw6/aussenbereich?newLoader=#Waypoint-Eingang |

* synced random seeds: these are commonly used to procedurally generate the same world on each client.
