# If you want to access your local rails app from a phone or another device…

It will not work, just accessing the ip. For security reason by default it won't accept outside connections.
So you need to start your server with a `-b` flag like

```
bundle exec rails s -b 0.0.0.0
```
