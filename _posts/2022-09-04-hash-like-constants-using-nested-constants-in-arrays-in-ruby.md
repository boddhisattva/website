---
layout: post
title: "Access Hash like constants in Ruby using nested constants in arrays"
date:  2022-09-04 07:02:00 +02:00
categories:
  - tech
  - ruby
  - rails
---

* Normally if we want to represent nested constants in a Ruby class(like say a Rails model) we might usually consider using a Hash for something we're introducing from scratch

something like:

```ruby
  CURRENCIES = {
    DE: '€',
    UK: '£',
    US: '$'
  }
```

* What if one already have an existing constant in an Array form in Ruby as part of let's say an existing Rails model class and this constant is used in enumerous files throughout your project but you'd like to represent it in the above hash form to denote which currency is for which country?


```ruby
class Payment < ApplicationRecord
  ACCEPTED_CURRENCIES = ['€', '£', '$']
end
```


We could do the following to retain the original array constant(instead of refactoring it to a hash and making changes in all the files where the constant is accessed) and additionally introduce a mapping of each country to a specifc currency through the below code:

```ruby
class Payment < ApplicationRecord
  ACCEPTED_CURRENCIES = [
    DE = '€',
    UK = '£',
    US = '$'
  ]
end
```

**Bonus learning**:

You can even access each of these above mentioned nested constants(like DE, UK etc.,) in the same scope as the outer constant(ACCEPTED_CURRENCIES) of the class

Example:


```ruby
  2.7.3 :014 > Payment::DE
 => "€"
 2.7.3 :015 > Payment::ACCEPTED_CURRENCIES
 => ["€", "£", "$"]
```

**Credits**: The Q&A here - https://stackoverflow.com/a/40830199/272398
