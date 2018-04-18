---
id: 225
title: Don't overengineer and get stuff done
date: 2016-02-02T14:33:49+00:00
author: Rafael M Teixeira
layout: post
guid: http://rafaelmt.github.io/?p=225
permalink: /en/2016/02/02/dont-overengineer-and-get-stuff-done/
dsq_thread_id:
  - "5227604866"
categories:
  - Software Engineering
---
While reading Umberto Eco's Numero Zero I came across a funny conversation that many software engineers will probably relate. Colonna, the main character and narrator, is talking to Romano Braggadocio, a journalist he just met. Braggadocio starts telling him about some story he is writing about that's going to be so huge it might even turn into a book. There's one thing, however, keeping him from finishing the required investigations: he needs a car.

Braggadocio then proceeds to describe all the features he looks for in a car. It has to be fast, sturdy, not too wide so to not fit an average garage, but not too narrow so to not be tight for a big man like himself. It has to be low-maintenance and quickly available but not too quickly (this would mean nobody wants the car, he claims).  He enumerates a lot of candidate cars, but none fits the perfect model of car he envisions.

As a result, his investigations never happen and the "big story" never comes through. The funny thing is, none of the features of the car would directly affect his investigations or keep him from doing them. You don't need a car that goes from 0 to 100km/h in 7.5 seconds to run a few errands; one that does it in 8.5 seconds would do just fine.

I saw an immediate parallel between this conversation and a number of personal/side projects that me and lots of colleagues have started but never finished. Everyone has cool ideas of new apps, web sites, services etc, but when it comes to implement it, some people get lost in an internal debate on whether they should use database A or B, which framework would scale better for 10 million users, what's the best language for concurrency etc. While you do it, your idea is still just an idea.

Engineers tend to overthink when they are starting new projects. As inherently lazy people, we dread the idea of having to rewrite stuff because the original implementation doesn't scale well, or having to revamp the whole architecture because it does not support the load anymore. What we sometimes fail to grasp is that, in order to have those problems, the product has to exist. If you ever hit one of those performance walls, you'll already have a somewhat successful product, which is certainly better than having just an idea.

Don't get me wrong, you can't just randomly make decisions at the beginning of your project and hope for the best. Some rationale must be used to back your architectural/design choices. Just don't let these decisions take too much of your time. Sam Altman from Y Combinator in his "<a href="http://playbook.samaltman.com/">Startup Playbook</a>" (http://playbook.samaltman.com/) recommends building things that would work on a scale 10x bigger than your current one. I think this is a great rule of thumb.

&nbsp;

&nbsp;
