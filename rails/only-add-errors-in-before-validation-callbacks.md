# Only add errors when they are actual validations

Sometimes it feels inviting to use `@record.errors.add` to add information about why a record fails to do something.
In my example I had a BulkOperation that kinda works like a job and is persisted.
When failing I added an error with `@record.errors.add(:base, message: some_log_data)`.

This is a bad idea, I unit tested it and though all was good.

But then couldn't mae it work in my controllers / views the way I wanted!?

It is simply the wrong tool. The erros are validation errors. Calling `invalid?`, `valid?`, `save!`… on the model will
rerun validation and remove attached errors. So your custom errors might just unexpetedly dissapear.

So if you want to add errors, only ever add custom erros as validation
(maybe also in a `before_validation` callback, but probably better not)
