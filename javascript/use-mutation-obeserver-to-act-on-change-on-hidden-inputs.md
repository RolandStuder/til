Use `mutationObeserver` to trigger events/execute funnctions on change of hidden inputs.

If you have a library that works with hidden inputs, you will not get the `change` or `input` events
you might be expecting.

You can however observe changes thank to `MutationObserver`

```js
let element = document.querySelector("input[type=hidden]")

let observer = new MutationObserver((mutations, observer) => {
  if(mutations[0].attributeName == "value") {
    // do something here
  }
});

observer.observe(element, {
  attributes: true
});
```
