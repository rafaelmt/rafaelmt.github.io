---
id: 140
title: Automatically Shutting Down Idle AWS Linux Instances
date: 2015-09-01T00:49:09+00:00
author: Rafael M Teixeira
layout: post
guid: http://rafaelmt.github.io/?p=140
permalink: /en/2015/09/01/automatically-shutting-down-aws-linux-instances/
dsq_thread_id:
  - "5312430388"
categories:
  - AWS
tags:
  - aws
  - linux
  - script
---
Let me guess: you forgot to turn off an EC2 instance that you've created to run a quick test only. Depending on how big the instance is and how long it took you to realize, it can be a costly mistake. And if you memory is as bad as mine, you have been there a couple of times ...

<!--more-->

To solve this problem, I created a script that turns off the machine after a given time without any SSH sessions:

[gist 20a89197200f95d30b38]

The time is passed as a parameter (in minutes). I haven't tested in all distros, but it should be fine with RHEL-based ones (Centos, Amazon Linux etc). Just put it in the root's crontab with a higher frequency than the desired timeout.. To check every minute and shut down after 30 minutes, for example, add the following line to root's crontab (<code>sudo crontab -e</code>):

<code>*/1 * * * * /home/centos/autoshutdown.sh 30</code>

Change the path to where the script is saved. Also make sure you have the permission to execute it

And please don't forget to remove it before going to production!

&nbsp;

[yasr_visitor_votes size="small"]