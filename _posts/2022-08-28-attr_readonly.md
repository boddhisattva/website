---
layout: post
title: "How to make sure a Rails Active Record model attribute value is set once but cannot be updated again - attr_readonly"
date:  2022-08-28 07:02:00 +02:00
categories: 
  - tech
  - rails
---

Recently I had to work on a task to add `updated_at` and `created_at` timestamps to an existing model(and its related table) and the related service class was using `upsert_all` (for performance reasons) to create/update new instances of that model and that class required adding these timestamps.

The app is currently on Rails 6 and with Rails 6 there is no auto update to `created_at` and `updated_at` timestamps via an `upsert_all`(This has changed with Rails 7 - more [here](https://blog.kiprosh.com/rails-7-adds-new-options-to-upsert_all/) ).

One way of setting these timestamps for this model that uses the `upsert_all` command is to do it manually for each record by manually passing `created_at` and `updated_at` as one would normally pass other model attributes as part of strong params in a controller and it’s related create/update actions.

Setting the `created_at` and `updated_at` using `Time.current` was how we were setting these timestamps for record insertions of the model in question via `upsert_all` but how do we handle updates with `Time.current` as we we wouldn’t want the value of `created_at` to change to the current time when we update an existing record using `upsert_all`.

**Solution**: **`attr_readonly`.**


As in the docs: [Attributes](https://api.rubyonrails.org/v6.0.4/classes/ActiveRecord/Attributes.html) listed as readonly will be used to create a new record but update operations will ignore these fields.

**Sample code usage**:

```ruby
class ModelBeingInsertedAndLaterUpdated < ApplicationRecord
  attr_readonly :created_at
end
```
