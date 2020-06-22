# Remember to include the controller name in the stimulus target.

Already tripped me up more that once. If you use a stimulus controller like… 

```haml
.div{data: {controller: "swap"}}
  .div{data: {target: "swap.out", action: "click->swap#swap"}}
    Swap me
  .div{class: "hidden", data: {target: "swap.in", action: "click->swap#swap"}}
    Swapped-in
```

… then be sure to use `target="controller.targetName"`, not just `target="targetName"`

For completeness sake here is the controller that goes along with it.

```javascript
import { Controller } from "stimulus"

export default class extends Controller {
    static targets = [ "in", "out" ]
    
    swap() {
        console.log("hello")
        this.inTarget.classList.toggle("hidden")
        this.outTarget.classList.toggle("hidden")
    }
}
```
