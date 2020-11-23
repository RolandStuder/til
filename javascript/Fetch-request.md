# Fetch needs you to say how to parse its readable stream.

The modern way to make asynchronous request seems to be `fetch`.
After seing fetch returns a readable stream, I tried to google and was
thorougly confused as to why I should have to write my own reader.

Turns out, you don't, you can use

```javascript
fetch("example.com", {
    method: "POST",
    body: new FormData(this.formTarget)
}).then(response => {
    return response.text() // <- here you say how to handle the stream
}).then(content => {
    this._replaceIframeContent(content)
})
```

The docs are quite hidden on 
[MDN using Fetch](https://developer.mozilla.org/de/docs/Web/API/Fetch_API/Using_Fetch),
scroll all the way down `body`
