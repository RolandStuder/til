# Keep the damn state on the server

Stimulus Reflex tries to make your life easier, by handling state on the server, don't fight that. 
Do not be afraid to add a db column that stores that state, even if you don't need it down the line.

Of course you could look into keeping state in the cache or the session, but don't overthink, throw 
that state onto the db if you must, because it will make stuff easier.

If need be, you will still be able to push this state into some other persistence layer…
