---
layout: post
title:  "Change column type and convert data in Rails"
date:   2020-11-24 23:43:00 +0000
categories: rails migration database
---

Wrote this post because I couldn't find an article in the World Wide Web on how to change the column type **and** convert its data.✝

So let's say we have a **Record** model with boolean column **master**, where a `true` values means that it's a _master release_ and `false` means that it's not. Records that aren't master releases can either be _re-issues_, _special editions_, _remixes_. We decided to model this info using just one column, and store it as `string`s.

Active Record's migration method `change_column` allows us to change a column's name and type. However it doesn't provide a way to also do data conversion.☦ My suggestion to implement this has three steps:
1. Add the column — **release_type** — with the new data type;
2. Convert boolean values from **master** column into respective string in **release_type** column;
3. Remove **master** column.

The migration code looks like this:
```ruby
def up
  add_column :records, :release_type, :string

  cast_values = <<-SQL
    UPDATE records
    SET release_type = 'Re-Issue'
    WHERE master = FALSE;

    UPDATE records
    SET release_type = 'Master Release'
    WHERE master = TRUE;
  SQL
  ActiveRecord::Base.connection.execute(cast_values)

  remove_column :records, :master
end
```

The `down` method would reverse this logic and it's left for the reader as exercise.

---

✝ Must confess that I bumped into many articles changing `integer` columns into `text`, or `datetime` into `date`. Although data conversion is implied it is something that Rails can handle on its own.

☦ [This article](https://makandracards.com/makandra/18691-postgresql-vs-rails-migration-how-to-change-columns-from-string-to-integer) does suggest that one can provide a SQL statement for data conversion. However I couldn't find any official documentation to support that.
