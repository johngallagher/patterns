---
categories: RSpec
name: Use descriptive real world data
---

Let's try to use real world descriptive data or something that is more intuitive for sample data in our specs.
It helps in improving clarity and expectation of the spec.

## Use of realistic data

## Bad

````ruby
  BlogPostCategory.new(name: "mac and cheese")
````

## Good

````ruby
  BlogPostCategory.new(name: "Real Estate Marketing")
````

## Use of descriptive data

## Bad

````ruby
alice = create(:user, name: "Alice")
alice.friends << create(:user, name: "Bob")
create(:user, name: "Mary")

visit friends_path(alice)
expect(page).to have_text("Bob")
expect(page).to_not have_text("Mary") # Who is Bob/Mary and how are they related again?
````

## Good

````ruby
user = create(:user)
user.friends << create(:user, name: "Friend of User")
create(:user, name: "Not Friend of User")

visit friends_path(user)
expect(page).to have_text("Friend of User")
expect(page).to_not have_text("Not Friend of User")
````
