---
title: Let the Text Be the Star of the Show
description: It’s sure bright in here! Give your eyes a few seconds to acclimate, and then say hello to our refreshed blog. For months now, I’ve wanted to put a bit more attention and effort into this space. As you will surely notice over the next few minutes of reading, I’m not the most elegant of writers, but it’s nonetheless something that I enjoy (and miss) doing; typically at a cafe close to my house, with a cup of coffee by my side.
author: Jeffrey Way
publish_date: '2023-09-13 12:00:00'
---

It’s sure bright in here! Give your eyes a few seconds to acclimate, and then say hello to our refreshed blog. For months now, I’ve wanted to put a bit more attention and effort into this space. 
As you will surely notice over the next few minutes, I’m not the most elegant of writers, but it’s nonetheless something that I enjoy doing; typically at a cafe close to my house, with a cup of coffee at my side. On that note, give me a moment to refill my cup...

![Preparing this blog post.](https://laracasts.nyc3.digitaloceanspaces.com/blog/images/let-the-text-be-the-star-of-the-show/writing.jpeg)

## The Problem

Up until this point, the Laracasts blog was largely a portal for site news and the occasional rambling on my part. But I hope to change that.

{info}
Unfortunately, like every developer instinctively knows, before we can prepare new content, we must first overhaul the entire application. **It’s a rule, folks**. I’m just following it.
{/info}

The first thing you’ll notice is the, well, white. It’s a far cry from the dark palette of the core Laracasts website, and that’s on purpose. Going over some older posts from the previous iteration of the blog, though the design was generally fine and clean, it didn’t necessarily make for an enjoyable reading experience. When I’m reading technical blogs, I want to see:

{panel}
- A light background color. It’s oddly softer on the eyes.
- Large, easy-to-read text for my aging vision.
- Adequate room for large code snippets.
- Plenty of “stop scrolling” callout points.
{/panel}

Our original blog design failed at every point above. So we rolled up our sleeves and got to work  on something **both simpler and better**.

I created a new Trello card (how we track all Laracasts projects, big and small) for our designer, Adrian. I had two primary requests of him:

1. **Unconstrained:** Make the blog its own thing, separate and free from the primary Laracasts website. He needn’t be limited by, for example, the existing Laracasts color theme or layout. In fact, I wanted to completely extract the blog into its own sub-domain and Laravel install. Its new home would be accessible through https://blog.laracasts.com.
2. **Minimal:** Laracasts is a very “designed” website, which I love. But, on the other hand, there’s something beautiful about a laughably minimalistic approach; one that lacks images or embellishments entirely. I asked him to “*make the text the star of the show.*”

![Screenshot of the Trello card for this redesign.](https://laracasts.nyc3.digitaloceanspaces.com/blog/images/let-the-text-be-the-star-of-the-show/trello.png)

Developers and designers sometimes have a tendency to over-complicate things. It affects us all, despite our best efforts. The developer might think, “*How can I get some more classes and abstractions up in here. This blog needs to be SUPER maintainable.*” The designer thinks, “*How can I change this up entirely into something new that bucks the norm?*”

We’re constant tinkerers. Projects are never truly finished. And when we do push to production, it’s of course with the understanding that we’ll add such-and-such feature in the next update. The unintended consequence, however, is you often end up, over time, with an overly verbose website or product. For this new iteration of our blog, **I wanted simple, simple, simple**.

{tip}
I once visited my local independent movie theater’s website to purchase movie tickets for later that day. Due to a bug on their end, none of the images and CSS was rendering properly. Instead, I was presented with plain text and unstyled form controls.

**Here’s the wild thing**: I kind of liked it! I was in and out in seconds. I wasn’t waiting for assets to load and render. I wasn’t scrolling in search of the oddly hidden credit card form I needed to make my purchase. It was all there at the top. My tickets were processed and purchased in seconds.

I hope to keep this in mind as I work on future projects. <abbr title="You ain't gonna need it.">YAGNI</abbr> applies to almost everything in our lives.
{/tip}

So, with that spirit in mind, if a feature or particular embellishment wasn’t essential to the basic act of reading, it was stripped away. We realized that this meant no sidebar; one column is fine and dandy. Also, why do we continue to display “share this article on social media” icons, when few people actually click them? Next, no annoying pop-up modals, no ads, no sponsorships, no thumbnails. Simple, simple, simple. And fast. (More on that later.)

Now that we know what to remove, what should we emphasize instead? Well, when I visit a programming blog, I want to immediately see:

{panel}
- What’s new since I last visited (fun tip, you can click the little square next to the `--latest` label at the top of the home page to condense the results for quicker scanning)?
- A list of tags for instantly filtering the site to what I’m most interested in learning about.
- Simple search functionality. We’re not dealing with thousands of articles. It needn’t be overly complex or designed.
{/panel}
- 
And…that’s it. Beyond that, all of our energy should be put toward presenting the text in a clean and friendly way.

## Stop the Scroll

When I was younger, I managed a programming education blog called [Nettuts+](https://code.tutsplus.com/) for a half-decade. Before taking on that role, I had very little experience with blogging or presenting web content. But, very quickly, **you learn that readers are scrollers**. Few people visit a blog post and read from top to bottom. Instead, they scan. 

> Their finger unconsciously reaches for that scroll wheel, and off they go in search of a “stop point.”

{tip}
A “**stop point**” is exactly what you think it is. In fact, you’re looking at one right now. It’s an embellishment that requests your attention. Go ahead, try it out. From the top of this page, scroll until something jumps out of you. It might be a prominent ordered list; it might be an image; it might be a styled definition panel; it might be a callout tip; it might be a block quote.
{/tip}

The funny thing is: preparing a blog post requires that you switch between various modes (like Vim). When you’re getting your thoughts down on paper, you’re not thinking about how to get the user to stop scrolling. Those are entirely different concerns. And if you think the ladder is unimportant, ask yourself how many times you’ve successfully made it to the end of a completely unstyled two-thousand word Twitter/X post. My current count is zero. My eyes glaze over around paragraph two. How about you?

## The Stack

Developers joke about rebuilding their blogs every year, but it’s never without good reason. As it turns out, a blog is simple - yet real-life - enough that it presents the perfect opportunity to experiment with new technologies or tooling. When your day job requires that you tend to a single long-standing product, you don’t always have the opportunity and luxury to start fresh. A personal blog is a wonderful solution to this problem.

While Laracasts, itself, is an SPA built on top of Vue and Inertia.js, this blog instead uses a mix of Livewire 3 and Alpine.js sprinkles. Yep, a completely different toolset that required many visits to the Livewire documentation to refresh and clarify my understanding (which had grown foggy in the years since I’d last used it).

I’m happy to reveal that it was an incredibly smooth learning process. **Livewire 3 really is a joy to work with.**

Here’s an example of a full-page component for this very page you’re reading.

```
<?php

namespace App\Livewire\Posts;

use App\Models\Post;
use Livewire\Component;

class Show extends Component
{
    public Post $post;

    public function render()
    {
        return view('livewire.posts.show', [
            'related' => $this->post->related()
        ]);
    }
}
```

{definition}
Livewire allows you to assign components directly to a route in your Laravel application. These are called “**full-page components**”. You can use them to build standalone pages with logic and views, fully encapsulated within a Livewire component. - *Livewire Docs*
{/definition}

Yep, that’s it! Think of this class as the Livewire-equivalent to a typical Laravel controller. And, in cases where you don’t need to pass additional data to the view, even the `render()` method is optional.

You see that `$post` property above? We don’t need to manually fetch it from the database, of course, because we can still leverage Laravel’s useful [route-model binding](https://laravel.com/docs/10.x/routing#route-model-binding) feature. In this example, Livewire recognizes that the property name, `$post`, matches the route wildcard name, `{post}`. Here’s the corresponding route:

```php
Route::get('/posts/{post:slug}', App\Livewire\Posts\Show::class);
```

Notice how we’re routing directly to a Livewire component.

## Livewire Fits My Brain

One notable thing that I appreciate about Livewire is how it doesn’t require that I throw away my current conventions and ways of doing things. My general approach to organizing components within a Vue app translates to Livewire quite nicely. Here’s a couple of examples to illustrate what I mean:

### 1. Newsletter Signup

Scroll down and check out that newsletter sign up at the bottom of this page (paste your email while you’re there). If I were using Vue, I would place that markup and code within a `Newsletter.vue` single-file component. Using Livewire, yep, pretty much the same thing. I created a Livewire component, called `Newsletter.php`. Here’s the actual code:

```
<?php

namespace App\Livewire;

use Livewire\Attributes\Rule;
use Livewire\Component;

class Newsletter extends Component
{
    #[Rule('required|email')]
    public string $email;

    public bool $subscribed = false;

    public function subscribe(\App\Newsletter $newsletter): void
    {
        $this->validate();

        $newsletter->addSubscriber($this->email);

        $this->subscribed = true;
    }
}
```

In Livewire, all `public` properties are made available to the corresponding template. In this case, we’re exposing the `$email` and `$subscribed` variables to the view. Next, similar to Vue, I can bind an input’s value to the `$email` property by leveraging `wire:model`, like so:

```
<input type="email" wire:model="email" required>
<button type="submit">Subscribe</button>
```

Easy! Notice how I don’t have to learn an entirely new way of doing things. This instantly feels familiar to me.

### 2. Progress Bar

Have a look at the top of the screen. Notice that blue progress bar that updates as you scroll the page? It’s a fun little feature that provides a bit of feedback on your current progress through the article.

Once again, using Vue, I might create a `ProgressBar.vue` component that is responsible for listening for the `scroll` event and adjusting the dimensions of the progress bar in response.

Using Livewire, I’ll just do it inline. This does require a bit of JavaScript, but luckily, Livewire ships with Alpine.js out of the box. As Forrest Gump might say, Livewire and Alpine go together like peas and carrots. In fact, every Livewire component is automatically an Alpine component.

Here’s the view for my `ProgressBar` Livewire component.

```html
<div
  class="bg-panel-200 w-full h-2 fixed top-0 left-0 flex z-10"
  x-data="{
    updateProgress() {
      let windowHeight = window.innerHeight;
      let fullHeight = document.body.clientHeight;
      let scrolled = window.scrollY;

      let scrollableHeight = fullHeight - windowHeight;
      let progress = (scrolled / scrollableHeight) * 100;

      // Min of 2, Max of 100.
      progress = Math.min(Math.max(progress, 2), 100);

      $el.firstElementChild.style.width = `${progress}%`;
    }
  }"
  x-init="updateProgress()"
  @scroll.passive.window="updateProgress"
>
  <div class="bg-gradient-to-r from-blue-dark to-blue-light"></div>
</div>
```

Though Livewire 3 allows me to write as little JavaScript as possible, in the rare situations when a sprinkling of JavaScript proves to be the better option for a particular task, it’s as easy as possible to integrate.

## I Get It

Okay, I get it. You’re looking at an incredibly basic one-column blog. It’s not the most exciting thing in the world. But that’s the point. We don’t need to reinvent the way folks consume content. 

{info}
There’s a reason why many of us often prefer Apple’s bare bones “Reader Mode” when consuming content on the web. As frustrating as it may be to see folks throw away your design work, it’s hard to deny that "Reader Mode" often makes for a more enjoyable reading experience.
{/info}

We wanted to put a bit of that no-nonsense spirit into the new Laracasts blog design. I hope you agree that it’s an improvement. Stay tuned for far more frequent updates. If you’re a fan of RSS, [here's the link you're looking for.](https://blog.laracasts.com/feed)

Until next time!
