title: "On Model.select(\"name\", \"(#{avg_sql}) avg_salary\") becomes magically available as a method in the ActiveModel::Relation"

---

Given a select subquery:

```ruby
# use a .select as .average executes the query immediatley, which is not what we want on a subquery
avg_sql = Employee.select('avg(salary)').to_sql

Employee.select(
  '*',
  "(#{avg_sql}) avg_salary",
  "salary - (#{avg_sql}) avg_difference"
)
```

I wondered how the hell to I get the values for `avg_salary` and `avg_difference`.
I was looking how go get the result as a hash instean of an ActiveModel::Relation.
Turns out ActiveRecords magic gives you the methods automatically so call

```ruby
Employee.select(complex_query).first.avg_difference
```
This is amazing as I still have access to the Model methods and I get the new methods.
