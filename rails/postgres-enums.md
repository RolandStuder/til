# Using Postgres Enums has its caveats

So I tried to use Postgres Enums, to not have just random integers in the db for our enums, see the migration:

```ruby
def up
  execute <<-SQL
    CREATE TYPE person_relationship AS ENUM ('personal', 'professional');
  SQL
  add_column :persons, :relationship, :person_relationship
end
```

## First, you need to declare enum in the model as a Hash

```ruby
  enum relationship: {
    personal: :personal,
    professional: :professional,
  }, _suffix: true
```

With numerous possible values for enums, this doesn't look nice. Maybe this gem may solve this in the future:
https://github.com/bibendi/activerecord-postgres_enum/issues/9

## Second: You can't use schema.rb

```ruby
# Could not dump table "persons" because of following StandardError
#   Unknown type 'person_relationship' for column 'relationship'

Rails doesnt' know how to handle that, so `rails db:schema:load` won't work.

You can set `config.active_record.schema_format = :sql` and it will instead 
create a `db/structure.sql `

## So drawbacks…

Using the enum type would mean better data in the db, but it comes at the cost of abanding the schema.rb file.
If you go for it, definitely checkout some gems, in particular: https://github.com/bibendi/activerecord-postgres_enum
    
