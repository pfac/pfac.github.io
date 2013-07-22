---
layout: post
title: "A Joining Spree Along The Rails"
date: 2013-07-22 23:01
comments: true
categories:
- "Ruby on Rails"
- ActiveRecord
- Inner Join
---
How to use ActiveRecord to perform a chain of nested `INNER JOIN` operations.

<!-- more -->

## TL;DR

"__I want to know all the distinct _option values_ of the _option type_ "gender", but only for the _products_ associated with a given _category___"

translates to

``` ruby
Spree::OptionValue.joins(
  :variants => { :product => :classifications }
).where(
  :option_type_id => 1,
  :spree_products_taxons => { :taxon_id => 2 }
).uniq
```

assuming that the given _category_ has the taxon ID 2 and that the _gender_ option type has ID 1.

The Spree 1.3 models:

``` ruby
module Spree
  class OptionValue < ActiveRecord::Base
    belongs_to :option_type
    has_and_belongs_to_many :variants, :join_table => 'spree_option_values_variants'
  end

  class OptionType < ActiveRecord::Base
    has_many :option_values
  end

  class Variant < ActiveRecord::Base
    belongs_to :product
    has_and_belongs_to_many :option_values, :join_table => :spree_option_values_variants
  end

  class Product < ActiveRecord::Base
    has_many :classifications, :dependent => :delete_all
    has_many :taxons, :through => :classifications
    has_many :variants
  end

  class Classification < ActiveRecord::Base
    self.table_name = 'spree_products_taxons'
    belongs_to :product
    belongs_to :taxon
  end
end
```

The schema:

``` ruby
ActiveRecord::Schema.define do
  create_table "spree_option_values" |t|
    t.integer  "option_type_id"
  end

  create_table "spree_products_taxons" |t|
    t.integer "product_id"
    t.integer "taxon_id"
  end
end
```


## Explain

Lately, I've working on this project for an online store in Rails, and for that I'm using Spree. In this store, every product belongs to a category, and every product has the option type _gender_ (man or woman). In every view's header there is a main navigation bar with a button for each category. Each button also has a dropdown menu for the available genders, so the user can easily see all the products for man in category A, for example.

The dropdown menus are not the same for all the categories. In other words, `CategoryA` has both `man` and `woman` in its dropdown menu, while `CategoryB` has only `woman`. As such, the dropdown is hard-coded for each button, which means that the navigation menu is completely hard-coded. This is ugly, hard to maintain and unnecessary, so I decided to change it to show the genders for which there are in fact products in each category.


### Models and Schema

As you can see in the __TL;DR__ section:

- An `OptionValue` has and belongs to many `Variants` (with the join table `spree_option_values_variants`)
- An `OptionValue` belongs to an `OptionType`
- A `Variant` belongs to a `Product`
- A `Product` has many `Classifications` (which use the table `spree_products_taxons`)
- A `Classification` also belongs to a `Taxon`
- A `Taxon` belongs to a `Taxonomy`

This store only has one taxonomy named _Category_. All the categories are taxons that belong to this taxonomy.

In order to make the menu dynamic I have to be able to obtain the gender values used among the products of a given category. In other words, __I want to know all the distinct _option values_ of the _option type_ "gender", but only for the _products_ associated with a given _category___.


### SQL

If I was using SQL directly, my life would be easier. Let's assume that my _gender_ option type has de ID 1 and that my taxon (category) has the ID 4. I can easily translate what I want into a single query:

``` sql
SELECT DISTINCT spree_option_values.*
  FROM spree_option_values, spree_option_values_variants, spree_variants, spree_products_taxons
  WHERE spree_option_values.option_type_id = 1
    AND spree_option_values.id = spree_option_values_variants.option_value_id
    AND spree_option_values_variants.variant_id = spree_variants.id
    AND spree_variants.product_id = spree_products_taxons.product_id
    AND spree_products_taxons.taxon_id = 4;
```

Yet, using ActiveRecord I access my records usually using some kind of `<class>.where(<conditions>)`. How do I get to that result?


### Join

Formally, what happens in that previous SQL query, as intuitive as it might be, is a set of consecutive `INNER JOIN` operations. Using that operation explicitly, the same query can be rewritten as:

``` sql
SELECT DISTINCT spree_option_values.*
  FROM spree_option_values
  INNER JOIN spree_option_values_variants
          ON spree_option_values.id = spree_option_values_variants.option_value_id
  INNER JOIN spree_variants
          ON spree_variants.id = spree_option_values_variants.variant_id
  INNER JOIN spree_products_taxons
          ON spree_products_taxons.product_id = spree_variants.product_id
         AND spree_products_taxons.taxon_id = 4
  WHERE spree_option_values.option_type_id = 1;
```

This is the way ActiveRecord allows to do this kind of searches, through the `join` method. Let's skip to the answer:

``` ruby
Spree::OptionValue.joins(
  :variants => { :product => :classifications }
).where(
  :option_type_id => 1,
  :spree_products_taxons => { :taxon_id => 2 }
).uniq
```

The `join` method uses the __associations__ names to perform the joins, instead of using the table names. This is why instead of using `:spree_products_taxons` we use `:classifications`. In this specific case, the join creates a chain: we want to chain the variants of an option value, using the product of each of those variants to get to the taxons.

Lastly, notice the `where` clause. When referring to the fields of the initial table (option values), using the field normally is enough. For the remaining tables, the table name and a hash of conditions is used.
