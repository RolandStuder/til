# Reconnect Action Cable on changed connection identifiers

Careful if ever you use an identifier in your action cable connection that will change over time! Look at:

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user, :current_account
```

If you use `current_account` in a [reflex](https://docs.stimulusreflex.com/rtfm/reflex-classes) that is very convenient.
But what if your application is set up in a way that you can switch accounts in your session? Turbolinks will persist the connection!

**So the `current_account` used in the view, and the `current_account` can get out of sync.**

## Strategy force reconnect on clicking links that may switch account

One way to do approach this, is to use `data-turbolinks="false"` on links that might switch to another account, or use some stimulus controller to
cut the connection, as Jumpstart Pro does. It works for one tab scenarios, but requires you to explicitely mark links that
may switch the account.

## Problem second tab!

If the user has two tabs open, things can get really messy:

* Tab 1 and Tab 2, are on account A. Tab 1 switches to another account, cuts its own connection, has now account B. If B now clicks on an link
the next view might render with current_account B, but the connection will still have current_account A.

## OK, then let's cut ALL connections

So can't we just all the connections upon an account switch from the server? Well we can, but this won't solve the problem fully, 
because if we do, and take the same scenario as above:

* Tab 1 and Tab 2, are on account A. Tab 1 switches to another account, cuts ALL connections, it now has account B. 
But Tab 2 still may show data from account A, but will have a connection with current_account B. So again out of sync…

## Two strategies:

### Do not rely on current_account in the connection at all

Depending on what you do, it might be better to just not rely on such objects that can change in the session.
If you need different accounts, maybe just make them available through path-parameters.

### Let the DOM decide when to reconnect.

We can delegate the responsability to the view to know when it needs a new connection, by:

* Attaching a stimulus controller, that will handle it
* Having a data-attribute, that has a string with current identifiers that the connection should have.
* Saving the identifier-string on the window (we loose the state of the stimulus controller on a new visit


So in your layouts you will have something like:

```html
  <body data-controller="dynamic-connection" data-dynamic-connection-identifiers-value="<%= current_account.id %>">
```

And the stimulus controller might look something like this:

```js
// Reconnect ActionCable after switching accounts

import ApplicationController from './application_controller'
import consumer from "../channels/consumer"

export default class extends ApplicationController {
  static values = {
    identifiers: String
  }

  connect() {
    super.connect()
    if ( !document.documentElement.hasAttribute('data-turbolinks-preview') ) {
      if (window.dynamicConnectionOldIdentifiersValue &&
        window.dynamicConnectionOldIdentifiersValue != this.identifiersValue &&
        consumer.connection.isActive()) {
          consumer.connection.reopen()
      }
      window.dynamicConnectionOldIdentifiersValue = this.identifiersValue
    }
  }
}

```

This way you get full controll over when a view will reconnect to get the right connection.

Note that you can put anything into that data-attribute, `ids` `cache_key_with_versions`, whatever you need
to make sure you have the right objects in your connection.


### How do I see that this is working?

Glad you asked, it is quite hard to see what is happening, you can use this version of the controller with some
debugging statements:

```js
import ApplicationController from './application_controller'
import consumer from "../channels/consumer"

export default class extends ApplicationController {
  static values = {
    identifiers: String
  }

  connect() {
    super.connect()
    if ( !document.documentElement.hasAttribute('data-turbolinks-preview') ) {
      console.log("old view identifiers: ", window.dynamicConnectionOldIdentifiersValue)
      console.log("new view identifiers: ", this.identifiersValue)

      if (window.dynamicConnectionOldIdentifiersValue &&
        window.dynamicConnectionOldIdentifiersValue != this.identifiersValue &&
        consumer.connection.isActive()) {
          console.log("reconnect")
          consumer.connection.reopen()
      }
      setTimeout(function() { this.stimulate("Debug#log")}.bind(this), 2000)
      window.dynamicConnectionOldIdentifiersValue = this.identifiersValue
    }
  }
}
```

Together with a reflex like this:

```ruby
class DebugReflex < ApplicationReflex
  def log
    cable_ready.console_log(message: "current_account connection: #{current_account.id}")
    morph :nothing
  end
end
```
