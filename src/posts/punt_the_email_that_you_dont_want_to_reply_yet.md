---
title: Punt The Email That You Don't Want To Reply Yet 
slug: punt_the_email_that_you_dont_want_to_reply_yet
date: 2022-09-01
tags:
- email
description: If you got an email that you know that you need to reply, but for whatever reason you also don't want to reply too soon. What are you going to do?
---

I rigorously practice [inbox zero](https://messagedesk.com/blog/inbox-zero-method/). I check my emails every few hours during the day, read every unread emails, including SPAMs. While going through my unread emails, I reply every emails of substance, even those that I don't have anything to say; A "Got it" or "Thank you" will do. However, there are some emails that you need to reply, but somehow you don't want to reply _just yet_:

* Maybe you need to do some research before reply and you don't want to disrupt the current work flow.
* Maybe you cannot think of anything to say, but you are sure you will, after a few hours or days.
* Maybe you just don't want to appear to be pushy or overly eager.

So what do you do?

## A few things you can do in mainstream email clients

You can just reply, but delay send the email. However, this method only covers a small subset of the situations. Also, delay sending feels like faking; If you are not careful, you could be sending your boss an email about work at 3am, and give your boss ideas that you don't want him to have.

You can mark the email as important, and come back later. However, it feels like cheating the inbox zero principle: Your unread emails are cleared but you just stuff the shit in another place. Emails may pile up there, you may forget to review, and you don't have the same sense of accomplishment when you reduce your inbox to zero.

You can start a reply, but don't finish the reply, and let the draft sit in the draft folder. Well, this is just a variant of the previous method, and worse still: you could accidently send your incomplete reply. uh-oh. 

## When you write your own email client

[I wrote my own email client](https://github.com/derek-zhou/liv). My philosophy is: If it is something you use daily with a non-trivial amount of time, you deserve a tool tweaked to your liking. What is better than a tool that you made yourself, entirely fitting to your own style? Whih this luxury on hand, I can afford thinking outside the box.

Why not just [punt](https://en.wikipedia.org/wiki/Punt_(gridiron_football)) it, just like in American football? Whenever I read an email, it is marked as read and will not show up the next time I get all my unread emails. This is basically the idea behind inbox zero. So instead of juggling replies and delay sending, I can set a timer on the original email, so it will be automatically marked as unread again in the future? This way, it will re-appear in my inbox and haunt me again, like a football punted to the other side of the court.

I come up with the idea during my morning jog, and enjoy the functionality before noon time. You know, I am a slow coder. I use [maildir](https://cr.yp.to/proto/maildir.html) to store my emails. The read/unread status is determined by the `S` flag; to toggle the flag is as simple as renaming the file. The timer is < 100 lines of code with [Elixir GenServer](https://hexdocs.pm/elixir/GenServer.html), then there is bits of U/I code and I am done. [The code is published](https://github.com/derek-zhou/liv) for anyone want to checkout.

## Summary

People keep saying that [email is a dinasour](https://emailisbad.com/). So what? It is an open standard, with a simple network format that you can monitor and manipulate, a documented data store that you can dig into and modify with your scripts. Try to do what I did with Slack, I chanllenge you.


