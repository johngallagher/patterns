# Don't use safe navigation operator unless `nil` is expected

Don't use the safe navigation ("lonely") operator to paper over an unexpected `nil`. This is _not_ the root cause of the bug and while doing so may suppress it, it's only going to make things harder to understand.

The safe navigation operator _is_ appropriate, however, when the receiver could _legitimately_ be `nil`.

## Bad

````ruby
foo.bar # => raises NoMethodError. "OH NO! better do this..."
````

````diff
-foo.bar
+foo&.bar
````

## Good

````ruby
foo.bar if foo.present? # => "hmm this is a little redundant, let's do this..."
````

````diff
-foo.bar if foo.present?
+foo&.bar
````
