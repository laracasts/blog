---
title: Automated, Interactive Transcripts
description: As you can imagine, Laracasts frequently receives requests for new courses and site features. One feature request in particular that often pops up is companion written tutorials for every video. It’s not a bad idea! In a perfect world, this would be a natural and perfect addition to the site. But, as always, there’s one big roadblock... time.
author: Jeffrey Way
---

<p>As you can imagine, Laracasts frequently receives requests for new courses and site features. One feature request in particular that often pops up is companion written tutorials for every video. It’s not a bad idea! In a perfect world, this would be a natural and perfect addition to the site. But, as always, there’s one big roadblock: time.</p>

<p>To do it properly would require a significant amount of work. And this is especially true for the types of videos we create at Laracasts. They don’t always map so easily to a written format. Nonetheless, the appeal is still there. I’ve even toyed around with implementing it, on and off, for a number of years now. But every time I get close to saying yes, I’m forced to return to the reality that we’re a very small business with limited resources. What you may refer to as a helpful new feature, I might consider “yet another thing we have to organize and be responsible for.” It’s a difficult tight-rope to walk sometimes. <strong>For every “wouldn’t it be cool if…”, somebody has to take ownership.</strong></p>

<h2 id="Middle Ground">Middle Ground</h2>
<p>I think I’ve landed on a reasonable middle-ground, however. One that’s imperfect, but cheap and automated. My two favorite things! </p>

<blockquote>
<p>We’re programmers. If it can be automated, then it should be automated.</p>
</blockquote>

<p>You’ll notice that all recent and new videos at Laracasts include a “<i>Toggle Transcript</i>” button at the bottom of the episode list in the sidebar.</p>

<figure><img src="https://laracasts.nyc3.digitaloceanspaces.com/blog/images/toggle-transcript.png" alt="image" title=""><figcaption>Video page with "Toggle Transcript" button.</figure>

<p>Clicking this will reveal an interactive transcript for the video. Press play, and the transcript will neatly follow along with the video, highlighting each section as you come to it. </p>

<figure><img src="https://laracasts.nyc3.digitaloceanspaces.com/blog/images/transcript-view.png" alt="image" title=""><figcaption>Laracasts Video With Transcript.</figcaption></figure>

<p>And if you’d rather advance to the portion of the video where we, say, discuss Laravel Sail, then you can search for “Laravel Sail” in the transcript and instantly play the video at that exact timestamp. Useful! </p>

<p><figure><img src="https://laracasts.nyc3.digitaloceanspaces.com/blog/images/transcript-search.jpeg" alt="image" title=""><figcaption><a href="https://laracasts.com/series/learn-vue-3-step-by-step/episodes/5">Click here to see an example of a video that includes a companion transcript.</a></figcaption></figure></p>

<p>But, it’s not perfect. Perhaps one day, automated transcriptions will be indistinguishable from those that are manually prepared by a human. But right now - and especially for programming videos - that’s not possible. So, we have two ways to deal with this:</p>

{info}
<ol start="1"><li>Allow it. These are automatically generated; people will understand that it’s a close approximation, but not perfect.
</li><li>Require a human to review and edit every new transcript.
</li></ol>
{/info}

<p>For a period of time a number of months ago, I chose the second option. And I was that human editor. The problem, again, was that it required a significant amount of time; time that would be better spent elsewhere. While I might eventually hire a third-party service or contractor to take ownership of this, for now, I’m going to keeps things automated and imperfect.</p>

<h2 id="Planning the Feature">Planning the Feature</h2>
<p>From the start, we knew that we wanted more than just a basic transcript below the video player.  No, instead, we wanted it to be interactive. Consider these requirements that we came up with:</p>

{info}
<ul><li>The transcript should be split into timestamps.
</li><li>It should synchronize with the video player.  Press play, and the transcript instantly transitions to the corresponding paragraph.
</li><li>The user should be able to search the transcript for keywords, and tab between every occurrence.
</li><li>Every segment of the transcript should be clickable. When clicked, the video immediately transitions to that exact timestamp.
</li></ul>
{/info}

<p><strong>I had no idea how to do any of these things.</strong> And, to be truthful, I couldn’t even tell you the definitions and difference between transcript, caption, and subtitle. 😬</p>

<p>But that’s my favorite aspect of programming. We’re constantly introduced to things we don’t know how to do, and told to “<i>figure it out.</i>” You better. Your job depends on it.</p>

<blockquote>
<p>As a quick aside, I’ve found this to be excellent training for your life in general. Whatever it is you need to accomplish, don’t always throw your hands up. Instead… figure it out! If your toilet is broken and needs to be replaced, don’t hire a plumber. Figure it out yourself. It’s not that hard. If you bought a home that has a small pool in the back, don’t hire a pool company to maintain the chemicals. Figure it out. </p>
</blockquote>

<p>Anyhow, let’s get back to the transcription project. I of course knew that there are a variety of services that will automatically generate transcripts for an uploaded video. In fact, platforms like YouTube and Vimeo do this automatically. If you inspect one of these generated files, you may find that it has a .VTT extension. VTT stands for “<i>Video Text Track</i>.” Let’s have a look at one of these files.</p>

<pre><code class='code-multiline'>WEBVTT - This file was automatically generated by VIMEO

0
00:00:04.400 --&gt; 00:00:07.000
All right, welcome back. So in just a

1
00:00:07.200 --&gt; 00:00:10.400
 little bit, maybe the next video, I will introduce you to a tool

2
00:00:10.400 --&gt; 00:00:13.100
 called Pinia, but you know, what? Why don't

3
00:00:13.100 --&gt; 00:00:16.300
 we take five minutes or so and just talk a little bit more

4
00:00:16.300 --&gt; 00:00:19.400
 about how you would wire this up yourself? Okay.</code></pre>

<p>Interesting! Notice how each section includes the corresponding timestamps. And you get that for free! Amazing.</p>

<p>The next hurdle is to figure out how to synchronize the transcript with the video, as it plays. That part is a little trickier, but you can probably imagine what needs to happen behind the scenes. Perhaps every second or so, the video should dispatch a JavaScript event with the current timestamp of the video. Our transcript “service” could then listen for this event, and highlight or “activate” the portion of the transcript that matches this segment. </p>

<p>To allow for this,  <a href="https://github.com/laracasts/transcriptions">I created a relatively simple Composer package</a> that reads a VTT file, and converts it into a series of <code class='code-inline'>Line</code> objects that encapsulate the line’s corresponding text and beginning / ending timestamps. </p>

<p>I could have reached for an existing package that supports a variety of text track file formats, but, again, I’m a big fan of figuring these things out yourself. Developers often say “<i>Don’t reinvent the wheel.</i>” I say reinvent to your heart’s content. (My personal workflow is to reach for existing packages for big or complex requirements. For smaller things, I almost always create an internal package myself.)</p>

<p>With this package, I can now load and parse a VTT file, like so:</p>

<pre><code class='code-multiline'>use Laracasts\Transcriptions\Transcription;

$transcription = Transcription::load('path/to/file.vtt');

foreach ($transcription-&gt;lines() as $line) {
    // $line-&gt;body;
    // $line-&gt;toHtml();
    
    // $line-&gt;timestamp-&gt;begin();
    // $line-&gt;timestamp-&gt;end();    
}

// Group lines into full sentences.
// $transcription-&gt;lines()-&gt;groupBySentence();</code></pre>
<p><br></p>
<p>Now that we can easily split this VTT file into lines, the front-end only requires a basic foreach statement to loop over the lines and generate a nicely formatted transcript. Perhaps something like:</p>

<pre><code class='code-multiline'>&lt;div 
   v-for="line in transcript" 
   class="transcript-section" 
   :data-time-start="line.timestamp.begin"
&gt;
   &lt;span&gt;{{ line.timestamp.begin }}:&lt;/span&gt;

   {{ line.body }}
&lt;/div&gt;</code></pre>

<p>Notice that <code class='code-inline'>data-time</code> attribute on the <code class='code-inline'>div</code> tag? We could pretty easily listen for a timestamp update event from the video player, and then write a query selector to find the closest matching transcript section.</p>

<p>And, really, that’s 90% of the work. As I worked on this new feature, I was repeatedly surprised by how simple it ended up being. </p>

<h2 id="The Next Steps">The Next Steps</h2>
<p>We’re going to live with what we currently have on the site for a few months. Near the end of the year, perhaps we’ll consider taking another step. While there are dedicated “premium” transcription services, we might instead hire a dedicated monthly contractor who would be responsible for doing a “second pass” over each generated VTT file (among other things). But we’ll see!</p>
