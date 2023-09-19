---
title: The Best of Both Worlds
description: I’ve been thinking about writing this blog post for months. It was the light at the end of an increasingly long tunnel. "When this project is finally done,” I’d tell myself, "I’ll set aside one Friday afternoon, find a seat at the coffee shop nearest to my home, and write an announcement - to the small portion of the world who cares about such things - that Laracasts is officially an SPA."
author: Jeffrey Way
publish_date: "2021-06-18 22:14:46"
---

I’ve been thinking about writing this blog post for months. It was the light at the end of an increasingly long tunnel. 

> “When this project is finally done,” I’d tell myself, “I’ll set aside one Friday afternoon, find a seat at the coffee shop nearest to my home, and write an announcement - to the small portion of the world who cares about such things - that Laracasts is officially an SPA (single page application).

To tell you the truth, I never thought I’d write or say that. Like many, I have my own set of frustrations when it comes to SPAs. Despite their obvious and immediate appeal, I’d always return to the same question: “_Is the complexity trade-off worth the effort?_” That question was stuck in a sort of purgatory until earlier this year when I began experimenting with [Inertia.js](http://inertiajs.com).

## 3 Ways to Organize a Website

There are a few common ways to construct a typical web application these days.

### 1. Server-Side

The first option is to render and manage everything on the server-side. This is the traditional way that you’ve experienced around the web for years and years. Submit a form, and wait as the page posts to the server, processes the data, and renders a new page. Of course the downside to this approach stems from the fact that, for each page load, all of its assets - scripts, stylesheets, images, etc. - must be re-loaded. Though browsers and servers have made great caching strides over the years to make this process as efficient as possible, it’s still unavoidably time-consuming.

### 2. Client-Side

Option number two is to instead render on the client-side. With this approach, the server-side portion of your application functions more as an API backend. It communicates with the database and responds to incoming AJAX requests. With an SPA, until the JavaScript kicks in, you’ll see nothing but a blank white page. This is what we mean when we refer to rendering on the client-side. It’s all JavaScript - from the routing to the communication. And yet, again, the downside to this approach is…well, JavaScript. It has been trendy for as long as I’ve been a developer to make fun of JavaScript; I’m not one of those people. In fact, in its current state, I very much enjoy working with the language. But, we’ve all experienced the downsides of a JavaScript-driven application.

{tip}
*   Raise your hand if you’ve ever clicked a button only to find that …nothing happened. No response. No feedback. Nothing.
*   Blink once for “_yes_” if a browser extension you installed accidentally broke the JavaScript on a particular website.
*   Press “_like_” if you’ve ever considered retiring from web development after getting lost in JavaScript bundler documentation hell.
*   Clap your hands if you’ve ever turned a simple web form into a confusing mess of JavaScript event listeners, validation hooks, and AJAX requests.
{/tip}

### 3. JavaScript Sprinkles

Option three is what I imagine most developers in the Laravel space have embraced. Or maybe “_embrace_” isn’t the correct word. It’s the option they “_fell into_.” My guess is you’ll instantly recognize this one. It’s that awkward combination of traditional server-side rendering with JavaScript sprinkled on top. With this approach, the initial page is rendered entirely by the server. But then your JavaScript hooks into the HTML and “_activates_” it to make things a bit more interactive.

Laracasts was built in this way. I didn’t exactly choose or “_embrace_” this particular option. It just sort of happened. Back in 2013 when Laracasts originally launched, it wasn’t so simple to build a reliable SPA.

I speak from experience when I say that this option is the worst of both worlds. Sure, it starts out reasonably enough. Intercept a form submission here, make a button dynamic over there. But, over time, it gets worse and worse. Nod your head if you’ve ever shamefully duplicated server-side authorization and validation checks on the client-side?

## Fast-Forward

All of the options above come with their own respective sets of pros and cons. Trade simplicity for interactivity. Trade responsiveness for consistency.

Over the years, I’ve kept my eyes on this space. Perhaps I’d build an SPA for future or side projects; but to do the same for Laracasts would effectively require a complete rewrite of the entire codebase. I don’t have the courage or energy for that.

But then I learned [Inertia.js](http://inertiajs.com). Instantly, it was clear to me that Jonathan (Inertia’s creator) had encountered - and solved - the exact same problems as me. How do you leverage the benefits of an SPA without giving up the luxuries and conveniences of a server-side framework like Laravel? By luxuries, I’m referring to things like routing, authorization, validation, and controllers.

Now I know what you’re thinking…

> “Oh look, yet another new trendy framework. No thanks. Hard pass.”

While I’m sympathetic to this view, I don’t necessarily think it’s valid in the case of Inertia. It’s not a massive framework that takes over my application. It’s not a replacement for Laravel _or_ Vue. It doesn’t require months of education to learn. Instead, “_glue_” is a better way to think of Inertia. It’s the glue that connects a server-side framework like Laravel or Rails to a client-side framework like Vue or React. Trust me: you can learn this in a day.

> Inertia allows me to flip my “_the worst of both worlds_” comment from earlier into “_the best of both worlds._”

With this approach, I’m able to continue reaching for the power of Laravel, while also embracing the speed and interactivity of a client-side app.

*   I still define routes in my `routes/web.php` file.
*   I still use traditional Laravel controllers.
*   I still use middleware.
*   I still reach for authorization gates.
*   I still exclusively use Laravel’s validation logic.
*   I still redirect in the usual way.

What _has_ changed, however, is that there is now just one Blade file in my entire codebase. And that file is the primary layout file. Every view has been converted to a dedicated Vue component.

Now, it’s beyond the scope and intention of this blog post to talk more deeply about Inertia, but I encourage you to [follow along with my first introduction to Inertia here on Laracasts.](https://laracasts.com/series/learn-inertia-with-jeffrey)

## A Big Update

This recent update to the Laracasts codebase is the product of three months of work. It was a slow, daily process of converting Blade views to Vue components. One after the other. Rinse and repeat. If nothing else, it’s a fun - yet tedious - exercise in porting logic from one language and framework to another.

But it wasn’t just the Inertia conversion that I worked on. As part of this project, we also took the opportunity to upgrade from PHP 7 to PHP 8, and from Vue 2 to Vue 3. The Vue upgrade particularly demanded countless visits to the Vue 3 migration guide.

So to say that I was a bit nervous about deploying this update is an understatement.

## Test Confidence

I had it marked on my calendar for weeks. Tuesday, June 15th would be the day we deploy this big update. Sure, all the tests were passing, but as we all sadly know from experience, this doesn’t mean that the application works perfectly. If it did, there would never be bugs. No, the only thing that green checkmark means is that the tests you wrote pass. It says nothing about the the tests you _didn’t_ write!

*   Your passing test suite didn’t account for the odd Redis warning in your production environment that triggered failures in the queue.
*   Your passing test suite didn’t account for that accidental production database record fragment with a foreign key that points to a related record that doesn’t exist.
*   Your passing test suite didn’t bother ensuring that a particular timestamp was visually formatted in the correct way.
*   Your passing test suite didn’t check for a slight flickering when a modal is opened in Google Chrome (but not Firefox and Safari).
*   Your test suite **did** confirm that users who subscribed to a particular series are sent email notifications when new episodes are released. But it **didn't** confirm that exactly one email is sent, rather than two. (Though now it does. 👀)

You get the idea. Unless you have the budget to hire a dedicated QA team to manually test your app in countless configurations, bugs are going to happen. The best you can do is get your suite to green, and then fill in the missing integration tests as they pop up. When you rewrite your entire front-end and deploy it to thousands of users, you're going to instantly receive a nice list of all the little things you missed. And that’s okay.

## The Next Steps

Now that this SPA conversion is complete, the door is wide open for a number of fun and important updates to the site that we’ve had on the back burner for a number of months. I’m excited to start working on them.

Ironically, one of my first tasks is to add SSR (server-side rendering) back into the mix. Around and around we go. 👀
