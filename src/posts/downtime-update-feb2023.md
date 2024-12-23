---
title: 'Downtime Update: March 2023'
prettyUrl: false
url: /posts/downtime-update-feb-2023/
date: 2023-03-03
author: Admin Tabby
fediverse: "@admintabby@blorbo.social"
tags:
  - News
  - Technical Stuff
toc: false
---

*ETA by Tabby, November 2024: For whatever reason, past me set the URL to say "Feb 2023" on the old blog but our whole downtime adventure seemed to start March 1? I've left the URL as is for archival purposes, but added a more accurate title to this post.*

Hello everyone! This is Tabby, one of the mods for Blorbo, and the primary person for technical support on the team! I'll hold back on my fandoms for today, though if you look at [my profile](https://blorbo.social/@Tabby) it should be pretty clear what kind of person I am LOL.

_For a little background: I work in software development currently, though the bulk of my career so far has been in web development. Outside of work, I like messing around with websites anyway so I like to think I have a pretty decent understanding of how these things work.. But even when I don't, I like to learn, which is how I volunteered for Blorbo in the first place!_

So: Onto the adventures of what knocked Blorbo offline in the first place, and why it took TWO DAYS to get it back up.

<!--more-->

This was, frankly, a weird one to figure out. I've dealt with a lot of bizarre site issues over the years, but this was a surprise.

In the earliest days of Blorbo, we were set up on DigitalOcean. After we moved to Toot.io, we no longer used DigitalOcean, and so it sat quietly for the past few months. Reasonably, [the Admin](https://blorbo.social/@admin) was like "we aren't using this, so I'll delete it."

And down Blorbo went.

As it turns out, our nameservers were still pointing to DigitalOcean despite having moved to Toot MONTHS ago. I believe DigitalOcean was acting as a weird middle point between the domain registrar and Toot.io.

So this: `Domain Registrar → DigitalOcean Nameservers → Toot.io`

Became this: `Domain Registrar → ???????????????????????????????????????`

And our domain was basically not connected to anything anymore.

In theory, we just needed to set the nameservers back to the default ones on the domain registrar and everything should have been fine, but it wasn't!

We were advised to remove all extraneous DNS records that had been added by default by the host, leaving a single "A" Record that pointed to our host. I thought this seemed reasonable... If it's not something we set up on purpose then it's probably not in use or it's part of the problem!

This did not work.

The Admin contacted support at the Registrar since this wasn't something that Jan at Toot.io could fix, and the Admin's attempts at fixing it also didn't work. Support advised waiting up to 48 hours. This is pretty standard advice for domain issues, but I've never had anything domain related take THIS long to update... but maybe this would be the exception that proves the rule. We opted to see if the issue would be resolved with a bit of time.

The issue was ALSO not resolved by time.

So tonight I strapped myself in to REALLY dig into this. At which point, I found that editing records on the registrar's site was... not behaving how I anticipated at all. I couldn't delete and remake records without getting strange errors, like our "A" record was coming up not found even though I was looking at it. So I reached out to support.

The short of that conversation is that the support agent restored all the default records for our domain, except for pointing the "A" Record to Toot.io. I was suspicious, but decided to give this a shot, and waited, attempting to visit Blorbo every few minutes until finally, it worked!! Blorbo was alive!!

To be brutally honest, I'm not sure why this all broke the way that it did. I can see more or less why deleting the DigitalOcean stuff caused problems, but the fact that this turned into SUCH a mess after? I'm baffled! It's either a little above my head, or it was one of those situations with no answer, where the fix is literally "turn it off and on again" or pressing the reset button in our case.

Either way, this was odd enough that I thought it might be useful to document. If you ever find yourself with a very broken website after you delete something that SHOULDN'T have been an issue, I suppose the steps are:

1. Check your nameservers and DNS records. If your Nameservers are pointing to something no longer used, reset them to the default ones.

2. Restore all your default DNS records, or ask support to do so, and then change only the ones that need to be changed. This might be something specific to our registrar—Our host said we only needed the one "A" record to point to Toot.io, and when I asked support at the registrar, they said no, we did in fact need all the default records too... though this wasn't addressed previously.

All's well that ends well, but man. This has been quite the 48 hours.

Here's hoping you don't hear from me again any time soon!

Tabby

PS: It was during this whole adventure that I learned the term "DNS propagation" is a bit of a misnomer! It sounds like you're waiting for servers to get a message from you in order to update your domain records, right? Turns out, any time you're told to wait 24-48 hours for your changes to "propagate" you're actually waiting for a bunch of caches to clear. A difference that makes no difference to most people, but was quite interesting to me! ([Source](https://www.tiger-computing.co.uk/myth-dns-propagation/))