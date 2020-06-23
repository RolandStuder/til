# Long compilations times? Webpack Dev Server helps

So, including tailwind vai webpacker, makes your compilation times really go up.
8+ seconds of compilation for every change in your CSS or Javascript.
That is no way to live.

So for development run then webpack-dev-serve

```
./bin/webpack-dev-server
```

This will only compile the necessary bits, also does not wait for a request to 
