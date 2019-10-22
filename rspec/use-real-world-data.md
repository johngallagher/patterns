# Use real world data

Let's try to use real world data or something that is more intuitive for sample data in our specs.
It helps in improving clarity and expectation of the spec.


## Bad

````ruby
  BlogPostCategory.new(name: "mac and cheese")
````

## Good

````ruby
  BlogPostCategory.new(name: "Real Estate Marketing")
````
