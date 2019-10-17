# Avoid using instance variables in view partials

![](../aux_assets/images/aaron_view_instance_vars.png)

## But why?
It’s too easy to presume that certain instance variables are available in the context your partial is called in.

When you first extract a partial, often for organisational reasons, it is typically used in only one place. When you reuse the partial somewhere else in your application, the controller action in which it is eventually called must correctly assign the same instance variable from the original controller action.

As your application becomes more complex, a view might contain several partials—each expecting their own instance variables—so you’ll need to assign several instance variables in the controller action. The context of these assignments will be a long way away from the nested partial in which they’re used.

If you rely on the “Rails magic” early on, you often pay in complexity later on. This is a pattern of many of the issues that people have with Rails’ HTML rendering environment. If you avoid using instance variables in your partials, they become simpler to reuse, even as your application grows in complexity

## Bad

```erb
<%# app/views/album/show.html.erb %>

<% @album.songs.each do |song| %>
  <h2><%= song.name %></h2>
  <%= render "character", collection: song.characters,
                          as: :character %>
<% end %>
```

```erb
<%# app/views/album/_character.html.erb %>

<p><%= character.name %></p>
<div class="popup">
  Other Songs: <%= character.features_on_tracks(@album.songs).to_sentence %>
</div>
```

Not too obvious that the `_character` partial would need an `@album` instance variable defined, right?
Since all instance vars have `nil` value assigned by default, the exception might not be raised if we miss it

## Good

```erb
<%# app/views/album/show.html.erb %>

<% @album.songs.each do |song| %>
  <h2><%= song.name %></h2>
  <%= render "character", collection: song.characters,
                          as: :character,
                          locals: { songs: @album.songs } %>
<% end %>
```

```erb
<%# app/views/album/_character.html.erb %>

<p><%= character.name %></p>
<div class="popup">
  Other Songs: <%= character.features_on_tracks(songs).to_sentence %>
</div>
```

In this case an exception is always raised when the `songs` local variable is missed
