---
title: SPA, History API, and hash links 
slug: spa_history_hash_links
date: 2024-10-05
tags:
- web
description: How I relunctantly adopted a SPA style for roastidio.us web site and my struggle with backward and forward navigations
---

[roastidio.us](https://roastidio.us) was a fairly old-school website. There is very little javascript; all same-site navigations were done by plain old links. Because the site is so light, pages load instantly; and all amenities provided by the browser work out of the box: you can bookmark any page; the back and forward buttons work as one expected. However, good things are not meant to last ... 

## Bots, what else?

I know, I know, having bots is a fact of life for any webmaster. I heard it is a [dead internet](https://theconversation.com/the-dead-internet-theory-makes-eerie-claims-about-an-ai-run-web-the-truth-is-more-sinister-229609) out there now; real human visitors are few and far in between, bots dominate your traffic. Search engines, AI data ingestors, everyone want your data. I am usually fine with it until my server start to struggle. Recently, I start to notice a pattern in bot traffic: Obviously they are crawling my site and following _every_ links. For a blog with lower hundereds of pages served statically it should not matter; the crawling will be done soon enough. But I have millons of pages, all dynamically generated, all inter-linking to each other. The traffic multiplies and it will never end.

To let human in but keep bots out, some people put up pay walls, some people use CAPTCHAs. I loathe both approaches. My solution is invisible to the users: I replaced all internal links with javascript generated form POST with CSRF protection, and patched the web page to the new content with fetch'ed data. Bots see no links, human happily clicking. Effectively, I turned the web-site into a quasi-SPA.

Bookmarks still have to work. So, I used the [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) to patch the location bar disaplay to the direct link. As expected, after a few days, bot traffic went down significantly. But then, new problems emerged.

## Back and Forward buttons?

Back and Forward buttons stopped working once I converted the web site to quasi-SPA style. To make them working again, I have to take on jobs previously I happily outsourced to the browser: Now, I have to save and restore page content myself in javascript using the History API. The API works this way: each time I change the URL, I can stow an opaque javascript object in the browser together with the URL, and the browser will give back the same object when history is revisited so I can resotre the page content when needed.

With javascript in my hand, I could do any DOM manipulation at the click; However, going back in time after arbitrary DOM manipulations would have been a hard problem. My usage here is fairly simple; so I got it all working in an hour or 2. Then, more problems emerged.

## Hash links and history don't mix

A hash link is a link to a `#...` URL, it points to a location within the same page. It is immensely useful: The browser will scroll to the right location for you and you can use the CSS `:target` selector to do all kind of styling. Having hash links does not contribute to the bots problem I have in the first place, so I don't want to give up on them. However, they don't not play well with the History API.

If a user clicks on a hash link, the browser will generate a `popstate` event. This is counter-intuitive: Nothing was _popped_ in this case. Clicking the hash link bypass my javascript, so, the state associated with this event is a helpful _null_. There is nothing I can do with it, and I don't need to, for the moment.

The trouble hits me when the user clicked a hash link, then a SPA link, then the back button. Because the transition before the last step was not a real navigation, the backward navigation depends on the javascript to restore the content. However, as I mentioned before, the state object from the browser for this history node is a helpful _null_. What can I do? Nothing. Therefore, the page content does not change but location bar changed. Not good.

My first attempt to fix is let the SPA transition to overwrite the _null_ state left behind by the hash link navigation. So, the hash link disapears from the history stack and clicking the back button take me back to the previous restorable state. Then I realized that the user could click a hash link, then another hash link, then a SPA link. My SPA link can decide not to create a new history, but it cannot erase existing history. You will still face a null state sometimes.

I tried a second approach, then a third one, each funkier than the last one. Eventually, I settled down to a compromise. It is still ugly, but at least not much javascript is needed.

* When I receive a `popstate` event, and the state is _null_, set a timer of 10ms.
* If the previous location and the new location point to the same page sans the hash part, the browser will send a [hashchange event](https://developer.mozilla.org/en-US/docs/Web/API/Window/hashchange_event). If this event is fired, I cancel the timer previously set: It is intra-page navigation, nothing need to be done. 
* If the timer expired, which means the page content is not matching the URL and nothing was restored, reload the page.

## My Rants

I took on a journey I did not expect, I learned many things along the way, I achieved (largely) what I set out to do. Why the taste in my mouth at the end is still sour?

I don't believe the web is really dead now. However, it is indeed a crueler place than 10 or 20 years ago. To survive in the cruel web, good, honest people like me have to dabble in some dark art to protect ourselves. I hope my dose of dark art is the smallest possible, so anyone reading this blog post can avoid the bigger ones.
