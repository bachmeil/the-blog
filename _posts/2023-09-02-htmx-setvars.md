---
layout: post
title: Setting variables in htmx
---
I've been using [htmx](https://htmx.org/) recently for an app that runs in my browser on localhost. Although I had seen htmx a long time ago, I'm not a web developer, so I didn't care to understand what it does. As it turns out, it makes it easy to write a single page application. As it further turns out, single page applications have the advantage of holding state, which is a lot easier than having to pass the entire state of the app between the client and server, because the page is never refreshed. Javascript variables can be set in the browser and passed to the server as needed. At least that's my understanding of a new concept.

This led me into a problem that I couldn't find addressed in the htmx documentation. Although it's different from the problem I was working on, suppose you're managing notes you're taking on two topics you're learning. You're learning about htmx and quantum physics at the same time. Those are not closely related to one another, so you want to click on a link to set which of the topics you're learning at that time. When you click on one of them, the application sets that as the project, and you query, view, create, and edit notes from that project.

How do you set the `project` variable on a click before making a request to the server for the list of notes on that project? Something like this will handle the request:

```html
<a hx-get="notelist?project=quantum" hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href="">Quantum Physics</a>
```

That doesn't change the state (the value of `project`), so each time you add a new note, you'll have to specify the project. And that would kind of suck.

The first improvement would be to clean this up by using `hx-vals` rather than modifying the URL:

```html
<a hx-get="notelist" hx-vals='{"project":"quantum"}' hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href="">Quantum Physics</a>
```

Then with `hx-vals`, you can set the value of variable `project` by adding a second (unused) parameter:

```html
<a hx-get="notelist" hx-vals='{"project":"quantum", _:project="quantum"}' hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href="">Quantum Physics</a>
```

That's straightforward, but it also involves (i) duplication of the name, and (ii) creating and passing an unused request parameter. The waste of an unused paramter probably doesn't matter, but if, for instance, the server iterates over all parameters, it's going to cause problems.

We can solve that by adding a Javascript function to our page:

```
function setProject(s) { project = s; return s; }
```

And then writing the link like this:

```html
<a hx-get="notelist" hx-vals='js:{project:setProject("quantum")}' hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href="">Quantum Physics</a>
```

That's a clean solution, and in fact, it might be the best solution. The `js:` enables evaluation of the expression as Javascript. The one I prefer so far is this:

```html
<a hx-get="notelist" hx-vals='js:{project:(() => {project="quantum";project})()}' hx-trigger="click" hx-target="#content" hx-swap="innerHTML" href="">Quantum Physics</a>
```

The only reason I like it better is because everything is contained inline, so I can see exactly what will do. I find it moderately more complex if I have the function separate from the link. The disadvantage is the verbosity this creates.

Disclaimer: I haven't tested the code. It's pulled from elsewhere and modified, so there may be typos. I'm confident you can see how to do things from the post.