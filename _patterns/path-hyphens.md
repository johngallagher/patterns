---
categories: Rails
name: Hyphenated URL paths
---

We prefer paths with hyphens rather than underscores.

## Bad

```rb
    resources :renewal_discounts
```

## Good

```rb
    resources :renewal_discounts, path: "renewal-discounts"
```
