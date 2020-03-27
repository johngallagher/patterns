---
categories: CSS
name: "Use text-transform: uppercase"
---

Some screen designs provided by the design team may include text in all-caps - usually as minor headings.

While the design may call for all-caps, use title or sentence case in the page's content, and use the CSS property `text-transform: uppercase` to turn it into all-caps.

The use of all-caps is an _ornamental_ design choice, and therefore should be separated from the content. Additionally, some screen readers may misinterpret parts of all-caps text as initialisms (e.g. "US" as "U.S." instead of "us").

### Bad
````html
<div class="related-content">
  <h2 class="related-content-heading">CHECK OUT THESE OTHER ARTICLES</h2>
</div>
````

### Good
````html
<div class="related-content">
  <h2 class="related-content-heading">Check out these other articles</h2>
</div>
````
````scss
.related-content-heading {
  text-transform: uppercase;
}
````

## Further Reading

* [WebAIM Guidelines](https://webaim.org/techniques/fonts/#caps)
