The gem `turbo-rails` give us an helper that we can use in those cases, the helper is `turbo_frame_tag` and accepts an `id` and a block containing the HTML we want to output.

> Generally you'll use the `dom_id` helper to generate an unique `id` but if you're using an *ActiveRecord* it get's called automatically.

Since in the example app we have a `redirect_to` in response of the `create` and `destroy` action of the `takle_box_items_controller` we know that when a user press the link to add/remove the bait will get redirected to the same page and Turbo Frame will do the rest **getting only the part of the HTML that has the same `id`**.

But we're still sending the **entire page** as a response, it's just that Rails (well Turbo Frame) is smart enough to stop the full page reload and replace just the part of the page we care about.

There is a different approach that allows us to respond with **the HTML for that specific Turbo Frame**.

In order to do that, instead of `redirect_to` we just need to `render @bait` and by convention Rails will render the `_bait.html.erb` partial.

# Breakout
Sometimes you have links inside a Turbo Frame that are not supposed to be rendered inside the frame, so you have to breakout from the frame in order to load them properly.

One example is a link that should `show` show the details of an item, in the frame you can show a form to `edit` it but in this case the link should just load the entire page.

If the response does not contain a Turbo Frame with the same id the existing frame is replaced with blank content and disappear from the page.

If you just wrap the HTML in `show` view in a `turbo_frame_tag` the content of the original frame is changed with the one from the show template and you keep seeing the other frames. But this is not what we want.

In order to make Rails show the `show` view we have to **brakeout from Turbo Frame** adding a `data` attribute to the link that will be clicked:
```ruby
# From 
<%= link_to bait.name, bait %>
```

```ruby
# To
<%= link_to bait.name, bait, data: { turbo_frame: "_top" } %>
```

This will add `data-turbo-frame="_top"` to our link, a simple way to tell Rails to just load the view instead of trying to do its Turbo magic.

# Turbo Streams
The Turbo Stream is useful when we need to change multiple part of a page