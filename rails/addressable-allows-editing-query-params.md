# You got a url string and want to change some query params?

In Rails you are covered by [Addressable::URI](https://www.rubydoc.info/gems/addressable/Addressable/URI)

```ruby
uri = Addressable::URI.parse("http://example.com/articles?sort=ASC&page=2")
uri.query_values["sort"] = "DESC"
uri.query_values = (uri.query_values || {}).merge({some_other_thing: true})
```
Makes changing url with query params much easier.

**Note: you can't mutate the query params individually, you have to set the entire hash:**

```ruby
uri = Addressable::URI.parse("http://example.com/articles?sort=ASC&page=2")
# THIS DOES NOT WORK
uri.query_values["sort"] = "DESC"
uri.to_s # => "http://example.com/articles?sort=ASC&page=2"
```
