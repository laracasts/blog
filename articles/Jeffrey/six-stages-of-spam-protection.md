---
title: The 6 Stages of Spam Protection
description: Launching and maintaining an increasingly popular forum requires nonstop attention on the developer’s part.
author: Jeffrey Way
---

Launching and maintaining an increasingly popular forum requires nonstop attention on the developer’s part.

Sure, the initial forum launch went smoothly enough. And why wouldn’t it? Nobody outside of your immediate circle knows it even exists. At this stage, assuming the “happy path” is sufficient enough. Some basic form validation will do the trick. “_You must provide a title and description to create a new thread._”

People are generally good. Of course they won’t take advantage of your new platform. Right?

Stage 1
-------

Over the first few weeks, your forum steadily grows. Conversations begin to form in each of the respective threads. How exciting! Remember that scene in Kindergarten Cop when Arnold Schwarzenegger realizes that his unconventional teaching technique is beginning to work with the kindergarteners? “_Yes - it’s working. It’s working!_” he says. …Of course you don’t remember that scene. But I do - and watching your fledgling forum grow feels exactly like that.

One day, however, you wake up, check in on the forum, and notice your first piece of spam. The thread reads:

> Greetings. I very much enjoy the teachings on this site. Click here for HD megavideo streaming.

Damn it. _They found us. I don’t know how, but they found us._

### Keyword Censoring

You shrug it off. No big deal. Perhaps some basic keyword validation will do the trick. In Laravel, we can easily whip up a custom validation rule:

    php artisan make:rule NoInvalidKeywords

A few lines of code later, and we now have a list of invalid keywords - beginning with “MegaVideo.”

    request()->validate([
        'body' => ['required', new NoInvalidKeywords]
    ]);

Problem solved! …Right?

Stage 2
-------

Spammers are like dinosaurs: life finds a way. Your basic keyword string checker didn’t account for clever formatting, like “_MEGA%%VID%EO Download H%D Link._” So once again, you wake up to a fresh pot of spam. Back to the drawing board.

### Email Confirmation

Perhaps the next step isn’t validation, but instead email confirmation. You can’t participate in my forum until you’ve confirmed your email address. This should get around all the [adsfasfaasfadsf@domain.com](mailto:adsfasfaasfadsf@domain.com) signups you’re suddenly seeing.

Laravel - again to the rescue - provides email confirmation via the `MustVerifyEmail` contract. Add it to your `User` model and the person will automatically receive a confirmation email after they sign up. You can then protect your routes by applying the `verified` middleware.

    Route::post('threads', function () {
        //
    })->middleware('verified');

With this change, we’re now effectively saying, “_Unless you’ve verified your email address, you may not create a forum thread._”

Problem solved! …Right?

Stage 3
-------

Email confirmation absolutely helps, but maybe not to the extent you initially hoped for. Like the bad guy at the end of the movie who just won’t die, the spam continues.

You’ll soon begin to encounter spam written in certain foreign languages. What’s the motivation for doing so? Who knows, but it comes in waves.

Drop whatever projects you had lined up for the morning. We need to fix this.

### Language Detection

It’s time to spend the next hour researching how to validate against particular languages. Funnily enough, the search results return a link to your very own forum. You find a thread that recommends the following regular expression.

    <?php
    
    namespace App\Rules;
    
    use Illuminate\Contracts\Validation\Rule;
    
    class EnglishOnly implements Rule
    {
        public function passes($attribute, $value)
        {
            return ! preg_match("/\p{Han}+/u", $value);
        }
    
        public function message()
        {
            return 'Please use English for the :attribute.';
        }
    }

You take a moment to smile. Wow, the tool I built to help others just helped me improve the tool, itself. That’s very cool.

Anyhow, focus Daniel-san. Back to work.

This validation rule isn’t quite right, but it’s a decent-enough first step to quickly push out a preventative fix.

    request()->validate([
        'body' => [
            'required',
            new NoInvalidKeywords,
            new EnglishOnly
        ]
    ]);

Problem solved! …Right?

Stage 4
-------

Don’t rest on your laurels too long. The next stage - and problem - stems, not from spammers or bots, but from the very people who actively participate in your forum! It seems that someone has realized that you aren’t throttling new threads or replies. An evil Grinch-like grin begins to shape their face.

> What if I wrote a script that replies to this thread with gibberish every five seconds?

Now you’re surely thinking to yourself, “_But why? What’s the point?_” Like so many things in life, the answer is a simple one: “_To see if they can._” Rule number one: assume the worst. People are not inherently good.

### Throttling

Less chipper than your former self, you return to your codebase in search of a solution. Perhaps I should limit the frequency at which you may create new threads or replies. The same user replying multiple times in a single minute is surely a sign of malicious intent. …That, or Red Bull.

Yay for Laravel’s custom validation rules. The gift that keeps on giving.

    <?php
    
    namespace App\Rules;
    
    use Illuminate\Contracts\Validation\Rule;
    use App\User;
    
    class PostThrottling implements Rule
    {
        public function __construct(protected User $user)
        {
        }
    
        public function passes($attribute, $value)
        {
            return $this->user->latestThread?->created_at->lt(
                now()->subMinutes(2)
            );
        }
    
        public function message()
        {
            return 'You are posting too frequently.';
        }
    }

This quick rule determines if the given user created a thread within the last two minutes. If so, that’s gonna be a no from us, dog. Let’s add this new rule to our primary validation logic.

    request()->validate([
        'body' => [
            'required',
       	new PostThrottling($user),
            new NoInvalidKeywords,
            new EnglishOnly
        ]
    ]);

At this point, you’re somewhat bitter that you’ve spent the morning on a commit to protect your forum from the very users it was meant to serve. But at least it’s done. Let’s push it to production and get back to work.

Problem solved! …Right?

Stage 5
-------

A week goes by without a single piece of spam. Cue the celebration music from the end of Independence Day. You won.

> Get on the wire and inform the squadron around the world. Tell them how to bring those sons of bitches down!

…If only.

Like so many Hollywood movies these days, there’s always a sequel. The spammers are not gone. They’re merely regrouping; waiting for the right time to attack. It has been decided that your upcoming family vacation is the right time. “_We strike at dawn_” I imagine they snicker amongst themselves.

“_Now boarding Zone 3_” the flight attendant says. You rise, gather your bags, and make your way to the boarding entrance. You feel a slight buzz in your left pocket. A new email. “_I really need to disable these damn notifications_”, you think. You remind yourself that you’re on vacation and have no interest in work emails. And, yet, it’s the not knowing that traps us all. What if it’s important? What if the site is down? Come on, it’s only one pull of the slot machine. And then you’ll turn it off. Of course you will.

You set your bags down, reach into your pocket, and secretly check (so as to avoid the judgmental glance of your spouse) your phone. The front page of your forum is covered in spam. We’re under attack! How did they get through our defenses?

“_Sorry, honey. I have to deal with this._” you shamefully remark to your spouse. With pep in your step, you make your way to your assigned seat, pull out your laptop, and quickly boot up your code editor. The flight doesn’t take off for 25 minutes. **There’s still time! I can fix this.**

### Honeypots

Perhaps we should be more thoughtful. Is there a way to trick these bots, so as to separate them from the submissions of our active users?

The answer, as it turns out, is yes! Honeypots to the rescue. Luckily, it’s a simple-enough technique that we should be able to implement before the plane lifts off…if we remain focused.

{definition=honeypot}
A **honeypot** is a pattern or technique that involves adding invisible inputs to your form. A bot will scan your form, encounter these fields, and fill them in to the best of its ability.
{/definition}

Now, all we must do is validate if these particular request fields are present. If so, ding ding ding: we have a spammer on our hands. No doubt about it. Abort, abort!

It’s such a simple solution that’s surprisingly effective. If only we had implemented this at the launch of the forum. Sadly, the obvious solution is rarely the first one we consider.

Problem solved! …Right?

Stage 6
-------

Congratulations. You kicked ass with time to spare. With five minutes to take-off, you won your vacation back. Don’t forget to disable those notifications… _like you promised_.

Fast-forward a few months, and spam is a fraction of what it once was. And yet, still, **a fraction is not nothing**. Despite your best efforts, you haven’t truly won the battle. Like the near-defeated monster, it still crawls toward you, hoping for one last grab.

It looks like we need a reCAPTCHA service. Must we make a deal with the devil to solve this unrelenting problem? “_Help me solve my lingering spam issue, and my users will help train your AI how to detect a stop sign._”

Like every bloody thing on the web, implementing it is never quite so easy. But, several browser tabs later, you manage to cobble together a solution.

Surely, the combination of keyword censoring, email confirmation, language detection, post throttling, honeypots, and reCAPTCHA are enough to slay this beast. It certainly seems that way. It has been three days without a single piece of spam.

Problem solved! **…Right**?
