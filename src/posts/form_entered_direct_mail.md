---
title: Form Entered Direct Mail
slug: form_entered_direct_mail
date: 2026-4-08
tags:
- web
- email
description: An email like application that lets users to send messages directly on the web 
---

I love emails, I really do. However, now I feel email is a lost cause. [SPAM black listing is out of control](/posts/spam_blacklists_are_out_of_control/), [Sending one's email is increasing hard](https://blog.roastidio.us/posts/send_my_own_email/). I need to have a contingency plan, the online community need a contingency plan. Of coursre there are secure chat applications, the fediverse, but they all come with their own problems, high complexity if nothing else. Here I describe something much simpler, something can be implement by any compentent web application developer on any stack. 

## What do I need (or don't need) from emails

I need emails to communicate with my fellow human beings, not bots. I am willing to give up a few features provided by email:

* pretty format (I am not sending promotional materials)
* attachments (I can send a link)
* multicast (for which I can use a web forum)

My realization is that one does not need any intermediatry to achieve an one-on-one, text-based communication. If anyone wishing to receive mails can host a web application with a simple form for sender to write them the message, then there is nothing else needed. No one can intercept your messages (you have SSL), and it put you the receiver in control for who can or can't send you messages. 

The following describe such a system: Form Entered Direct Mail, or FedMail. 

## Not all users are created equal

There are 2 classes of users in this system:

* The ones who host their own FedMail instances (Class 1)
* The ones who do not host their own FedMail instances (Class 2)

Communcation can only happen between 2 class 1 users or between one class 1 user and one class 2 user. 2 class 2 users cannot message each other. 

Each Class 1 user can publish an URL as thier FedMail address. This URL shall be a https host, with no path or query string. Below I will use the example addresses https://user1.example.com and https://user2.example.com

## A Class 2 user writing to a Class 1 user

The sender finds out the recipient's address is https://user1.example.com and visit there. If the user has an esteblished cookie session then they can skip the user registration. If not, they are directed to a registration page which can be as simple as none at all. All personal information can be optional; including the name or the FedMail address.

If the user has a cookie session, the url https://user1.example.com shall present the user with past conversations between the user and the owner of the domain. Any messages retention policy can be implemented; some retention is expected.

On the same page, there should be a "compose" button/link that lead the user to a page with a form to compose a message to the owner of this instance. There should be a title field and the textarea for the body of the message. The application should allow a subset of Markdown syntax so the writer can format their message nicely. 

Once the writer clicks the "send" button, the message is submitted and to be presented to the owner of the domain. The owner can read and decide wheteher to reply. If the sender did not provide a return FedMail address in their configuration, the conversaion is captured no where else: The only way to check and read replies is by visiting back here. For the owner, they should see *all* converatiaons, and for the visitor, they should only see their own conversations, with identity as defined by the cookie session.

## A class 1 user writing to another Class 1 user

Things are more interesting for communications between 2 class 1 users. Let's say the owner of https://user2.example.com wish to write to the owner of https://user1.example.com 

The preparation step is the same, user2 should esteblish cookie session with https://user1.example.com . Cookie session should last some time so this step is infrequently needed. If the cookie session was not present or expired, a later step will fail so they can register and try again. Since user2 has an instance of their own, user2 should configure the FedMail address in the user registration/configuration page on https://user1.example.com to include the return FedMail address. 

To write to user1, the user2 visits *their own* FedMail instance: https://user2.example.com and click the "compose" button. Writing to an external party should be a priviledge reserved to the owner the instance. After done with composing and upon clicking the "send" button, the following should happen:

1. A copy is saved locally on https://user2.example.com, to esteblish the conversation context
1. a background post with the message to https://user1.example.com
1. Upon receiving a post, https://user1.example.com shall save the incoming message in a a special qurantine space, and generate an opaque, unguessable handle (could be just a random string) with a short life-span, embedded in an url as part of a HTTP 302 reply
1. Uppon receiving the reply with the url, https://user2.example.com will redirect the user to the given url
1. Now that the user is redirected to a page on https://user1.example.com with cookie session, https://user1.example.com can deliver the message for real, filling in the "from" address for future replies.

The above scheme is based on the [Post, Redirect, and Post again](/posts/pass_data_between_websites/) described earlier in this blog.

## Message-ID, In-Reply-To, and threading

Threading should be preserved in [the same way as in Email](https://cr.yp.to/immhf/thread.html). Because the communcation is always one-on-one and the full conversation is preservered on both FedMail instances, only the Message-ID and the In-Reply-To field should be generated and sent; the References field is optional.

Hard seperation of email threads by the other party's session cookie should be maintained. This is to mitigate the risk of Message-ID forgry and spoofed messages. The associated downside is that one cannoy reply to the same email thread from 2 different broswers/devices.

## Security analysis

The FedMail system does not even pretend to verify identity. Messages can be forged, just like email. However, human are pretty good at telling forgry from authentic messages by reading the conversation context, which is binned by cookie session. If a bad actor pretends to be someone else and send you a message, you will see no or little context on your server. Everything can be forged on a bad actor's server, however, a user should know what they have written, so a forged converation cannot hold up to scrunity.

SPAM cannot be fully eliminated but they can be limited by mitigations. The owner of any FedMail instance can employ all kinds of standard web methods:

* rating limiting incoming post
* rate liming per cookie session for message submission
* require cool down period for newly created cookie session to send a message
* or even more obnoxous measures, like CAPTCHAs

The spec is intentionally vague to be human (implementors, operators and users) friendly and bot unfriendly. The extensive use of cookie session also works this way. The author acknowledged that any system can be abused; however, the hope is that the cost-to-benefit ratio will make abuse less economically viable on the FebMail than on Email, due to the bot unfriendliness.
 

