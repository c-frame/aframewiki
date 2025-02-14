# Best Practices

## Events or Public Functions?

Q: If I want to invoke some functionality on a component from outside teh component, is it better to do that by emitting an event that the component listens to, or via a function call?
A: I generally prefer emitting events, over direct function calls.
Some benefits:

- If the other component is not initialized yet, you don't get a crash.
- If you want to fan info out to multiple entities it is much easier.
- It may not be obvious that changing a function within a component might break things outside that component, whereas events are clearly an external interface, so you're much less likely to imagine you can change them without breaking things.
- Easier for testing individual components (due to looser coupling).

That said, there are cases where a function call direct to a component is justified. There's a few examples in core A-Frame, e.g. on [Raycaster](https://aframe.io/docs/master/components/raycaster.html#methods).
Typically the main reason to use a function rather than events for inter-component comms is when you need to get an answer/data back. Maybe also for performance reasons (honestly don't know what the performance differences are, but I have heard occasional concerns expressed about performance issues with very high volumes of DOM events).
If it's a 1-way communication, and not happening super-frequently, I think I'd pretty much always go for an event.

to add: discussion on garbage collection, differences coding in aframe vs. 'normal' javascript, component interfaces differing from normal functions

- this is full of great little best practices: https://discoverthreejs.com/tips-and-tricks/
- https://webglinsights.github.io/tips.html
- https://medium.com/@michael.andrew/6-things-you-havent-optimised-in-your-webvr-content-272d74d541f0
