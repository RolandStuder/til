Unpoly / Intercooler like behaviour is pretty easy to do with Stimulus…

After playing around with StimulusReflex and Unpoly.js, everything felt a bit quirky.
It felt like I was fighting them to achieve what I wanted (Inline editing, and inline-adding new things)

So I looked into StimulusJS to find whether I would find it easier when I had more control over the API.

In the view:

* add the conroller
* add the target (do not forget that the target needs the controllername `inline-new.newForm`)
* add the action

```haml
.new-group{data: {controller: 'inline-new'}}
  .new-form{data: {target: 'inline-new.newForm'}}
    = link_to "+ Add new category", new_menu_list_menu_group_path(@menu_list), remote: true, data: {action: 'ajax:success->inline-new#form'}
```

In `inline_new_controller.js` (be sure to end the filename on `_controller.js`)

```javascript
import { Controller } from "stimulus"

export default class extends Controller {
    static targets = ["newForm"]

    form(event) {
        let [data, status, xhr] = event.detail;
        this.newFormTarget.innerHTML += xhr.response;
    }
}
```

In the ActionController respond with just the html you need.

Small footprint but much more control. I think this is the way to go.

ps: For some reason `request.xhr?` just doesn't work.
