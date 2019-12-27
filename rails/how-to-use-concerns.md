# How to use concerns

> TL;DR. Don't use Rails concerns.

Concerns are a great way to share reusable functionality between classes. They come with drawbacks however as discussed [here](https://medium.com/@carlescliment/about-rails-concerns-a6b2f1776d7d).

Almost every problem using a concern solves, can be solved in more explicit ways. For example with inheritence, or object-oriented code.

## Good

We should use concerns to:
- Share concrete, discrete functionality between classes. An example might be a concern called `Notifable`. Such use-cases are usually rare.

## Bad

We should not use concerns to:
- Make a class "smaller", as this is purely cosmetic
- Avoid placing code at the correct layer of our MVC architecture
- Abstract logic that belongs in a PORO
- Operate any business logic

## Further reading

https://medium.com/@carlescliment/about-rails-concerns-a6b2f1776d7d
