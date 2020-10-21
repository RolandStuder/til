# My stimulus controller is not reconnecting after a reflex - Why? And what do I do now?

As Stimulus Reflex uses morphdom, only the differences in the DOM get changed,
however `connect()` only is called when a element with the `data-controller` attribute
enters the DOM. So there is often no disconnect/connect for your stimulus controllers.

## So what options do I have?

* If you don't need the elements to be updated by the reflex, 
  you can use `data-reflex-permanent` to make sure behavior added
  in your `connect` method does not disappear.
* If your added eventListeners disappear, you can use the `data-action="event->controller#action"`
  notation, which never fails.
* If those don't work, you need to make an explicit call to your controller using the Reflex lifecycle events.

## Using Reflex lifecycle events.

Extract your behaviour from the `connect`

```js
connect() {
  this.enhance();
}

enhance() {
  doStuff();
}
```

Then use the `stimulus-reflex:finalize` event (currently only on #master)

```html
<div data-controller="beautify" data-action="stimulus-reflex:finalize@window->beautify#enhance">
```
