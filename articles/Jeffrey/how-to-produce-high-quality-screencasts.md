---
title: How to Produce High-Quality Screencasts
description: I’ve kept a somewhat scatter-brained version of the following text as a Laracasts instructor onboarding Trello card for years now, but it occurred to me that it might prove useful to others who hope to create educational content.
author: Jeffrey Way
---

I’ve kept a somewhat scatter-brained version of the following text as a Laracasts instructor onboarding Trello card for years now, but it occurred to me that it might prove useful to others who hope to create educational content.

I’ve of course extended that Trello card with plenty of fluff, and no less than three pop culture references for your reading pleasure. So, if you’re ready, let’s make like [Leeroy Jenkins](https://www.youtube.com/watch?v=mLyOj_QD4a4) and do this.

{panel}
[\- Jump to all recommended screencasting purchases.](#all-recommended-products)
{/panel}

What Went Wrong?
----------------

Okay, look, if you want to crank out a screencast as quickly and cheaply as possible, you could hook up your eight dollar “wired headphones with built-in mic” to your computer, open Quicktime, and choose “_New Screen Recording_” from the _File_ menu. And, yeah, that would do it! Upload that bad-boy to Youtube, and wait for the traffic to come rolling in.

…But, wait, a week passes, and you only have two views. One of those is you, and we suspect the other is from your mom. How come? Where’s the traffic?

### Supply and Demand

Things have changed. In 2006, due to sheer lack of supply, I would often begrudgingly watch blurry 480p technical screencasts with audio that sounded as if the speaker was trapped alongside Dorothy in her tornado-fueled spinning home. Noise, wind, and what appeared to be laughing witches in the background.

{info}
But that was 2006. To put things into perspective, at that time, Netflix streaming wasn’t yet a thing - and wouldn’t be for another two years. Hulu wasn’t a thing. Amazon Prime wasn’t a thing. Youtube existed, yes, but was still in its infancy and mostly consisted of cat clips and “my day at the zoo” videos, recorded on two-megapixel cameras.
{/info}

Supply and demand is the key here. Ask me how many times I’ve endured a noisy, low-resolution screencast in the last five years. The answer is, of course, zero times. In the year 2022, the supply is through the roof. Everyone has a hot new course or six-hour A-Z training video on YouTube. It is no longer acceptable or appropriate to publish a lazy, low-quality screencast and expect anything more than, well, two views.

> If you’re serious about this, then you’ll need to put your money where your mouth is. As they say, if you’re going to do it, then do it right.

The minimum barrier to entry is a microphone and screencast editing software. Let’s start with the mic.

Audio Considerations
--------------------

I get it. Your AirPods include a built-in microphone, and you sometimes use it for Zoom calls. Great, but let’s be better than that. Audio quality for technical education is paramount. Purchase the best-possible microphone that your budget will allow for.

You generally have two categories to choose from: USB-powered and XLR.

### 1. USB Microphones

A USB microphone provides the simplest, plug-and-play entry. They often even include a built-in stand for your desk. You’ll find a wide array of options in this category. You might consider the following options:

*   **[Rode NT-USB](https://amzn.to/3Ayw7OF)**
*   **[Blue Yeti X](https://amzn.to/3TnIIg6)**

Either of these will offer a significant leap over using your computer's built-in microphone. And...yeah! This might be enough, depending on your intended audience! I’m a big fan of simplicity. This path fits the bill.

### 2. XLR Microphones

The second category requires more complexity and a heftier financial investment. But the improved audio quality is not insignificant. The gold standard for podcasting is:

*   **[Shure SM7B](https://amzn.to/3pZTITC)**

This microphone usually hovers around $399 USD, so it’s not cheap. And, worse, you’re not done taking out your wallet. You may notice, after your first test recording, that the volume output level it produces is surprisingly quiet. To fix this, you’ll likely want to also pick up an external gain booster. Try one of these:

*   **[Triton Fethead](https://amzn.to/3AYDCjc)**
*   **[Cloudlifter](https://amzn.to/3R6zD9X)**

Either option will do the trick (I use a Fethead), and are easy to use. Plug your XLR cable into them, and forget about it. You’ll now have an extra 20 decibals of gain to work with. Problem solved.

But, sorry, you’re still not done. You now have your new microphone and Fethead in hand, but you instantly notice that it includes... an XLR cable. So you awkwardly hold that cable up to the back of your computer in search of a suitable port, and wonder how you’re going to plug this dang thing in. Okay, that’s the next step.

### Audio Interfaces

The solution to this XLR cable problem is to purchase an audio interface. Think of these in the same way you would think of a PHP adapter class. The job of an audio interface is to convert, or adapt, an input source - your XLR mic - into a format that your computer will recognize. I’d recommend picking up:

*   **[Focusrite Scarlett 2i2](https://amzn.to/3R323lm)**

The Focusrite Scarlett is incredibly popular, and for good reason, too.

So here’s how it works. Plug the microphone into your new audio interface, and then connect the audio interface to your computer through the provided USB cable. Success! You’ll now find your new input source if you access the “Audio Input” window for your OS, usually in your settings area.

Great! Onward to the next problem.

### Mic Stand

Your old USB-powered microphone probably included its own stand. This new SM7B does not. Now, you could try balancing it on a stack of books on your desk, but let’s not. I’d recommend picking up a swivel boom stand that can be attached to your desk. That way you can push it away when you’re not recording.

*   **[Heil Sound PL-2T](https://amzn.to/3wJ4u4c)**

### Audio Setup Complete

So, yeah, your audio setup can get pricey very quickly. If your budget is minimal, stick with a USB-microphone. Who knows if you’ll still be recording videos six months from now. Keep it cheap and simple until you’re certain that the additional expense and complexity is worth the effort. Like programming, complexity must be introduced through necessity.

Record Your Screen
------------------

If you’re a Mac user, Quicktime is a perfectly suitable option for recording your screen. But, of course, at some point, you’ll need to edit your screencast. I’m going to assume that you’re not a master record-in-one-go speaker. Few of us are. Most of my completed videos contain dozens upon dozens of edits and cuts. So it sounds like you’ll require dedicated software to edit out those umms, ahhs, and pauses. Here are some popular options.

*   **[ScreenFlow](https://www.telestream.net/screenflow/overview.htm?utm_source=bing&utm_term=screenflow&utm_campaign=Multi-Varient_brand_sf&utm_medium=cpc&utm_content=v1q88y5t_dc%7Cpcrid%7C82807117833237%7Cpkw%7Cscreenflow%7Cpmt%7Cbe)**
*   **[Camtasia](https://www.techsmith.com/video-editor.html)**

Note that these applications are specifically designed for screencasting. Prepare a new recording - usually 1080p or 1280x720 HiDPI is sufficient for programming content - select your new microphone from the audio input tab, and you’re off to the races!

Screencasting Techniques, Mistakes, and Deal-Breakers
-----------------------------------------------------

Over the years, I’ve produced and edited _a massive amount of programming-specific content_, created by myself and others. When you do this, you slowly develop an eye for what works, and what doesn’t work; what keeps the viewer’s attention, and what makes them drift off.

As you work on this, you, too, will become sensitive to certain mistakes or ticks that might annoy the viewer.

### 1\. Start With “How”

A high-quality microphone and suitable screen resolution are essential prerequisites. But the real value of your video will depend on one thing: you. How will you present information to the user? Don’t skip over this important step.

{tip}
*   **University:** The school-like approach, where you speak over a series of slides. I generally discourage this for programming content, but it’s an option.
*   **Rapid:** Intentionally quick-paced format. How much information can we crank into a 3-minute video? There’s a trade-off here, of course, but it’s an interesting choice.
*   **Unedited Streams:** Long-form streams, no edits, intended to simulate actual work-flow. This style is popular in the programming world, but I generally avoid it.
*   **Youtuber:** Includes on-camera work, fast cuts, background music, and a quicker pace. This requires a significant amount of effort and potential scripting, but produces an enjoyable video that is geared for high amounts of traffic.
*   **A-Z:** In depth training, broken into chapters or sections. Each chapter can be its own video, or merged into a multi-hour compilation video.
{/tip} 

There’s nothing that says you must choose one - and only one - option from the categories above. But keep in mind that if you intend to regularly produce content, it might prove useful to be consistent with your style. It will ultimately become a key part of your overall branding.

### 2\. The Light is Green

You have around ten seconds to grab the viewer’s attention. This doesn’t mean you must do something crazy to pique their interest (though that works), but it does signal that you should get going right from the start. **Are you sure that you need a logo sting, followed by a painfully-slow transitioning slide that displays the title of your video**, followed by another slide with your name and photo, and then finally, for good measure, an extra five seconds of silence before you start speaking, “Hello…”? What if you instead said “Hello” in the first second? Cut the slides and pause. Or, even better, what if you skipped over the greeting entirely and jumped right into the subject matter of the video?

Generally, I like to adopt a “don’t tell…show” approach. I promise you: that five-minute slide at the beginning of your video - where you outline what you’ll cover in the screencast - isn’t necessary. Viewers will fast-forward. Particularly for education content for programming, slides are almost always the wrong choice in this author's opinion. Restrict their usage to situations where a diagram or comparison is paramount for the viewer’s understanding. Otherwise, remove them.

> Don’t tell... show.

Of course, rules are meant to be broken (including this one). But, when in doubt, imagine that you’re back in school, and the teacher throws a dense slide onto the projector. Now count how many seconds you last before your eyes begin to glaze over.

### 3\. Space-Fillers and Quirks

Editing your own voice can be a jarring experience. You’ll quickly be introduced to your own quirks and idiosyncrasies - many of them, not so great.

The occasional “ummm…” is perfectly fine, and makes you human. But like many things, ensure that it doesn’t get out of hand. I’ve edited more than one video where I was forced to, one by one, remove the author’s continuous use of uhh’s and ahh’s. If you annoy the viewer, they change the channel. It’s as simple as that.

Other examples might include smacking your tongue against the roof of your mouth, or explaining a concept while you rapidly shake the mouse cursor absentmindedly.

Pay attention to your go-to “space-fillers” or fidgeting and make a deliberate effort to avoid them. And if you can’t, well, that’s what your editing software is for.

### 4\. Today, Junior!

As I record, I like to imagine that there’s an imaginary viewer saying, “_Come on, come on. I don’t have all day._” No, don’t speed through the subject matter, but always be respectful of the viewer’s time and attention. That’s part of the job. If, in a video, it took you 45 seconds to scroll to the top of a PHP file and import a namespace, is there a way to instead do it in 5 seconds? Would leveraging the automatic namespace imports that your editor or IDE provides be more respectful of the viewer’s time? Yes, of course it would. You’re competing with Netflix right now for the viewer’s attention. Don’t give them 40 seconds to consider switching back. Try to be sensitive to small time sinks like this. 45 seconds is a long time to watch a person import a class.

The bulk of my personal screencasting struggles revolve around this idea.

> Am I respecting the viewer’s time? Is this portion going on too long? Have I made too many mistakes? Am I speeding through this and losing their attention? Did I not explain that part well enough? Did I over-explain it?

It’s not an easy task, and I’ve screwed up countless times. Still, the most important thing is to continuously ask yourself the question.

### 5\. Focus, Daniel-san

Another mistake I often fix during the editing process is when I lose focus and begin discussing things that don’t immediately relate to the main subject matter of the lesson. Suddenly, I’ve veered off on a five minute tangent, possibly losing the viewer in the process. Again, this is a give-and-take problem. If you’re too focused and robot-like, you risk losing your friendly connection with the viewer.

> Sometimes, an unrelated tip or anecdote is the primary thing the viewer remembers from the video.

So don’t always drive in a straight line, but also don't take every single off-ramp for a “personal story” aside.

Each viewer has a problem that they need your help to solve. In some less technical cases, that problem is simply boredom. YouTubers often come across as if they’ve had one too many Red Bulls because they’re desperately trying to maintain your attention. But in the context of a programming video, the viewer's problem is usually more specific. “I need to know how to do _this_ thing.” If you repeatedly pull away from filling that need, they’re going to switch to a more focused alternative video.

### 6\. To Script or Not to Script

Many newcomers to screencasting will default to reaching for a script. They’ll think to themselves, “_Well, I’m giving a presentation. And when I’m on stage, I speak from a script that I’ve memorized. So I should do the same for a screencast._”

This **might** be the right way to go, depending on which style of content (listed above) you choose. If your goal is to create an on-camera TED-like talk, then, trust me, those speakers have prepared and rehearsed their scripts hundreds of times. Do the same. But if you instead intend to create relaxed technical education content, I would strongly encourage you to not do this. Viewers can tell.

Here’s a thought experiment. Once again, you’re back in high school. Your favorite History teacher is giving a lecture on, say, the initial invasion of Poland that started World War 2. As you picture the teacher in your head, are they reciting directly from the History book? I hope not. By all means, outline, prepare and practice what you’ll discuss in the video. But when it’s time to press the red button, put the script away.

### 7\. Tell Me Why

I often notice that programming teachers on Youtube will tell you what they’re doing, but rarely _why_ they’re doing it. Perhaps the assumption is that the viewer already knows why, but from my experiences, that’s usually not the case.

> Don’t simply speak the code you’re writing. Tell me why it’s being written. You’re a teacher, not a code reciter.

All Recommended Products
------------------------

That’s all, folks! 

{info}
Here’s all the products I recommended in this article. **Please note that some of these may include affiliate links, because, hey, [why not?](https://tenor.com/view/easy-money-terminator-2-jhon-connor-gif-12024074497600583036)**

*   **[Rode NT USB Microphone](https://amzn.to/3Ayw7OF)**
*   **[Blue Yeti X USB Microphone](https://amzn.to/3TnIIg6)**
*   **[Shure SM7B XLR Microphone](https://amzn.to/3pZTITC)**
*   **[Triton Fethead Gain Booster](https://amzn.to/3AYDCjc)**
*   **[Cloudlifter Gain Booster](https://amzn.to/3R6zD9X)**
*   **[Focusrite Scarlett 2i2 Audio Interface](https://amzn.to/3R323lm)**
*   **[Heil Sound PL-2T Boom Stand](https://amzn.to/3wJ4u4c)**
*   **[ScreenFlow Software](https://www.telestream.net/screenflow/overview.htm?utm_source=bing&utm_term=screenflow&utm_campaign=Multi-Varient_brand_sf&utm_medium=cpc&utm_content=v1q88y5t_dc%7Cpcrid%7C82807117833237%7Cpkw%7Cscreenflow%7Cpmt%7Cbe)**
*   **[Camtasia Software](https://www.techsmith.com/video-editor.html)**
{/info}
