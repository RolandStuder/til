# Searchkick with custom fields

If you want to use [searchkick](https://github.com/ankane/searchkick) for custom fields (where you don't know all possible field names), you need to make sure to index them:

```ruby
# model class

def search_data
 {
   custom_fields: some_jsonb_column # {year: 1916}
 }
end
```

Flattening the fields is not an adequate solution, as [indexes are limited to 1000 fields](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-settings-limit.html)

Nested indexes / objects can be queries with the dot notation `custom_fields.field_name`. But doing `SomeModel.search(where: {"custom_fields.some_field": 1})` sadly will yield no results. This is because Searchkick just does not support the `object` out of the box (though it support `nested`).



## So you need a custom mapping

```ruby
    searchkick merge_mappings: true, mappings: {
      properties: {
        custom_fields: {type: "object", dynamic: true}
      }
    }
```

## But you also need advanced search

Custom mappings require you to use advanced search, so `SomeModel.search(where …)` will actually not yield any results for object queries. So you need to query with pure elastic search notation:

```ruby
SomeModel.search(body: {query: {bool: {filter: [{term: {"custom_fields.year": {value: 1916}}}]}}})
```

## PS: Nested queries work out of the box

```ruby
# post.rb
def search_data
 {
   author: author
 }
end
```

Can be queries with `Post.search(where: {"author.name": "Tom"})
