# `.without` nifty helper method

ActiveSupport has a nice helper method for Arrays `.without`.
Can be used on ActiveRecord::Relation so you can exclude one or many results from the relation.
Very practical if you have a query that gets many records, but you want to exclude the current record on a show page.
