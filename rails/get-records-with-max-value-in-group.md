# Get the records with max value by group

Say, we want to last created product for every category. Sadly `Product.group(:category).having('MAX(created_at) = created_at')`
does not work with Postgres.

So instead we are gonna use `DISTINCT ON` to isolate the categories and just order the records in a way, that we get the max value:

```ruby
Product.select("DISTINCT ON (category) id, category, created_at").order(:category, created_at: :desc)
```

explanation: 
* `DISTINCT ON (category)` makes sure that we only get one distinct record on that column
* `order(:machine_sn, time: :desc)` Postgres demands that the first column of `ORDER BY` is 
the same as the one used in `DISTINCT ON`, then we order by the thing we want maxed. 

So from the combination of the right ordering and having only distinct records we get directly the records with the max values.

Note that as we don't select all attributes in the select AR will only have the value of those attributes,
so add what you need or just put select("DISTINCT ON (machine_sn) " + MachineState.atributes_name.join(", "))
or something like that.
