# How do forms interact with model / scope / params and all this jazz…

This is a dump of some discord odin answer I gave. I hope to over time make this an article that is a good introduction into forms, though https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/ruby-on-rails/lessons/form-basics sounds good
I think people need an aritcle though that revisits how things interact with each other, and what is convention and what is required.



Basically try to understand what is happening, when you do a form_with model: @flight do |form| it does 3 things (among others), unless otherwise specified:

1. the form is grouping params in [:flight] (the model name)
2. the form tries to read the current values from the model form.text_field :from, will prefill the value @flight.from (that is also why it complains if tries accessing @flight.passenger_count when no such method exists, adding the attr_accessor alleviates that
3. it sets a url by default, to flights_path or flight_path(@flight) depending on whether it is persisted or not.

So basically you are using Flight to encapsulate your FlightSearch attributes. There are two way around that: Actually create a FlightSearch model (does not need an underlying database model), but the easier way is to just use params.

You can scope a form like this: form_with scope: :flight_search do |form|, then you can use whatever fields you need, because it will not try to prefill them from a model.

If you want to have prefilled value, you can always do so manually:

<%= form.text_field :from, value: params.fetch(:flight_search, :from)

