# LifeCycle events are only triggered for the corresponding stimulus controller

First be sure to use th right `import`s and to `register`!

`tooltip_controller.js`
```javascript
import { Controller } from 'stimulus'
import StimulusReflex from 'stimulus_reflex'

export default class extends ApplicationController {
  connect() {
    StimulusReflex.register(this)
  }
}
```

Then if you trigger an event from the template

```html
<button data-reflex=click->tooltipReflex#someAction">
```

It will call lifecycle events like:

```javascript
export default class extends ApplicationController {
  afterReflex() {
    var replaceElement = this.element.outerHTML
    this.element.outerHTML = replaceElement
  }
}
```

But not

```html
<button data-reflex=click->someOtherReflex#someAction">
```

