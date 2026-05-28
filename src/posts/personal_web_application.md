---
title: Personal Web Applications
slug: personal_web_applications
date: 2026-05-28
tags:
- web
description: A personal web application is one that is written by me, hosted by me, and have a grand total of one user, me. 
---

This is a manifesto of personal web applications. I came from an older generation when computers are used to write programs, mostly to scrach our own itches. Then the programs we wrote were open sourced so we can share with others, then they become sharewares so we can earn some money, eventually they becaome SaaS and we became entrepreneur wannabes. This is a call to action to go back to our origin using the latest technology.

## The computers in fromt of you are not yours

You are probably reading this using a phone, a tablet or a laptop. They are powerful computers, but they are not yours. They are owned and subsidized by Apple, Google, Microsoft for the purpose of harvesting your attentions. You are granded a license to use them in a sanctioned way, for a time. Let's call them for what they are: they are terminals, terminals to access entertainment contents, terminal to access the SaaS that have lodged in your life for better or for worse, and terminal to access your own computers, if you have one.

Your computers are elsewhere; they are in your basement, for the ones who have a home lab, or in a datacenter for the ones who rent VPSs. Although the VPSs are also not owned by you, they are at least real computers, one that you can install software without approval, one that you can program using a plethora of programing stacks. The application that you can make with them is most likely a web application nowdays.

## The Web is a lonely place

The modern web is built for SaaS, or software as a service. Companies large and small publish their applications on the web, for a subscription that sometimes is free, and we the users can enjoy the convieniences at our finger tips to hail rides, order food, or binge watch TV shows. Each SaaS applications have tons of users, or at least they claim to have; some SaaS applications are specifically made for communication and have billion of users! Why are we still feeling lonely?

This is a question I cannot answer, nor can I offer a cure. What I can offer here, is to strip off the glamour of collaboration on the web and tell you what we can do and should do without it. Collaboration does not exist as much as you like because everyone has different agenda. Collaboration is unnecessary because you alone can achieve amazing things already, and collaboration is often harmful because it dilutes your responsibility.

So you made this amazing web application and no users come. You thought all you need is a little polishing here and there, a little marketing, but the reality is bleak: The web is a lonely place, there is no collaborators, neither does it provide users. All you have is yourself to enjoy your creations. It is personal, like it or not. 

## The taxnomy of personal web applcations

Once you have the revelation that your web applications are personal one way or the other, the decisions come easy. You write one for yourself to scrach your itch. You tweak it for yourself for your ever changing needs. And you make it online or offline on your own schedule.

It can be completly private, no other visitors are welcome. For example a diary.

Or, it can allow anonymous visiting to public accessable content that you choose to share. For example a blog.

Or, it can allow user registration and offer them limited utilities. You alone remain the super user for life.

Or, it can be as democratic as possible and does not have a concept of a super user. Still, you control the source code so it remains highly opnionated and obscure. Obscure is good! It adds a layer of security that you dearly need.

## The simplicity of a personal web application

Although there are many different kinds of personal web applications, they share some common traits.

> complexity sells better.  ― Edsger Wybe Dijkstra 

But we don't need to sell, right? We have an army of exactly one soldier, anything that is not strictly needed by ourselves is a burden we do want to carry.

A personal web application will most likely have a very light client-side, most (or all) application logic should reside on the server-side. This is because the data must reside on the server-side so server-side logic is a must. And we don't want to deal with the complexity of a distributed system that will be caused by a heavy client side.

A personal web application will be hosted on my home lab or on a VPS near me. We are not masochists so server latency should be small. Also, it does not need to scale, or have much eye candies. Therefore, server-side rendering is not only possible but also preferred. 

One will have multiple personal web applications for different tasks, a super app that does everything adds unecessary complexity. However, personal web applications from one author will share much of the infrasture and common framework/libraries. Sharing is caring, even in a 100% selfish perspective. Then it is only a small extrapolation to share the common libraries/framework as free sofware to the wider world. Why not? Maybe collaboration will come after all.
