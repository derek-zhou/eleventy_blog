---
title: Pass data from site A to site B
slug: pass_data_between_websites
date: 2023-01-19
tags:
- web
description: How to pass data between 2 websites, with no trust or shared secret established between the 2 websites?
---

How can one user use their data from site A and in site B? The user could open 2 tabs in the browser, copy/paste the data from site A into site B. But it is tedious. On the other hand, huge amount of user data are passed around the web each day, without user intervention or even consent, thanks to API calls between websites. Those means are not controlled by the user; also, there have to be some kind of trust established between the sites for API calls to work; again, the shared secrets are not under user's control. Is there anopther way?

## Can I just post it? ##

Sending data on the web is usually as simple as submitting a form via HTTP POST. Site A could present a form to the user, but post the data to an enpoint on site B. The data will land in site B no problem; however, there are 2 issues with cross-site POSTs:

* Site B has no way to verify the identity of the user. In a cross-site POST, the browser will not attach cookies that belong to site B, so a user session is missing.
* site B has no way to verify the integrity of the incoming data. It could be modified, or even forged, by site A. Or, it may be part of a DDoS attack.

Can we overcome the issues through a bit of ingenuity?

## Post, Redirect, and Post again ##

Here I will describe a method that let site B to verify both the identity of the user and the integraty of the data. I call it PRPA, or post, redirect and post again. It only depends on basic HTTP operations; there is no shared secret, no javascript magic, the 2 site don't need to talk to each other directly. Site A can even be a static site.

The first step is the same: Site A presents a form to the user, with action pointed to site B. The user is in full control of whether to submit the form. Then, once site B see the incoming post, it does 2 things:

* stashes the data in a special qurantine space, and generate an opaque, unguessable handle (could be just a random string) with a short life-span.
* redirects the user to another endpoint on site B, with the handle as a query parameter

Back on the user's browser. It sees the redirect and will oblidge. Because this redirect is from site B back to site B, the session cookie will be attached in the redirected HTTP GET. From this incoming GET with session cookie, site B establishs the user's identity. Then, site B presents a form of its own, pre-populated with the data retrieved from the qurantine area with the given handle. Npw the user can visually verify the data to be the same piece of data they wanted to submit back in site A. If everything looks right, they can submit the form. Now site B can rest assure that the data is genuine.

## Drawbacks and future work ##

One downside is that the user have to click twice. It may be regarded as a feature: Often times form submission need a confirmation step; with this method, the confirmation step can be merged into the second post.

The other downside is that it only works with one-way data passing. The initiating website cannot pull data from another website; nor can it get an confirmation of whether the data is really posted. I can argue that the PRPA method favors users' privacy over convieniency though.

Finally, site A must cooperate and present the form. If it doesn't, then there is still hope. For the more enterprising user, they can make an [Greesemonkey script](https://www.greasespot.net/) to inject the bit of html in the website from which you desire to fetch data.

## A demo ##

A blog post without a demo is not worth reading. The commenting box at the end of this script is such cross-site form. This blog is a static website; however it can make use of [Roastidio.us](https://roastidio.us) for commenting from within the static html. Try it yourself, or better, look at the html source and use it on your own site. It is just a simple form.

The PRPA method is so simple, I don't think it is patentable. I am not aware of anyone using it, though I could be wrong. If you have seen a prior art, let me know by commenting here!

