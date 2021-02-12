# `rails db:reset` does not migrate

The other day, I was confused that I had trouble seeding my database, while it was fine for my team member. 
Turns out that I was doing `rails db:drop db:create db:migrate:with_data db:seed` while he was smarter than me
and used `rails db:reset db:seed` which is not only shorter but faster. My issue was, that some data_migration 
was not playing well with the seeding.

So I learned `db:reset` loads the schema from `schema.rb` without doing the migrations.
