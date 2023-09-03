---
layout: post
title: Disappearing links with htmx
---
I was genuinely puzzled by a problem with htmx, and I could not find it addressed anywhere in the documentation. For unknown reasons, the links would just not work - they'd always reload the page rather than sending a request to the server. I even checked the server, and the request was for a new copy of the page, not the request that was supposed to appear.

Eventually I figured out what was happening. I was inserting html that used htmx into the page with a manual request. That doesn't work.

The solution? Set up a link to the request exactly as if you were going to click it manually. But with an id and no description. So something like this:

```
<a id="foo" hx-post="theurl" hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href=""></a>
```

Then add Javascript to the bottom of the page that calls it after running a Javascript function:

```
<script>
function refreshPage() {
    // Put the main code here
    document.getElementById("foo").click();
}
</script>
```

That will put the output of your request into the `content` div, and it have links that actually work.