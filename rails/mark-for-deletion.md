# Mark for destruction

If you want to destroy some associated records, but wait until the parents saves, you can use mark for destruction

https://apidock.com/rails/ActiveRecord/AutosaveAssociation/mark_for_destruction

> Marks this record to be destroyed as part of the parent’s save transaction. This does not actually destroy the record instantly, rather child record will be destroyed when parent.save is called.
>
> Only useful if the :autosave option on the parent is enabled for this associated model.

