# Hotwire

## Turbo Drive
It watches for link clicks and form submissions and instead to send a `document` request sends a `fetch` and it takes the full response from the servers, merges the `head` and replace the `body` from the one it got back from the server.

By default all links and form are handled by Turbo Drive but we can disable it by sending a `data-turbo` attribute set to `false` like this:
```ruby
<%= link_to "Second page", second_page_path, data: { turbo: false } %>
```

## Turbo Frames
It allows you to update part of the page by sending small part of the document (the HTML) as response. Basically you chose what part of the page needs to be updated.

We have first to declare a part of the page that will be updated, for example if a login form is a Turbo Frame when we send it the server will respond back with just that part of the page.

## Turbo Streams
This uses WebSockets under the hood and let's you stream to multiple browsers the changes made by a form, it does not support yet GET requests.