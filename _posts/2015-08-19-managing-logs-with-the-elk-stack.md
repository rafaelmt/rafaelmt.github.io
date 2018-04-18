---
id: 85
title: 'Tutorial: Managing Logs with the ELK Stack'
date: 2015-08-19T23:53:32+00:00
author: Rafael M Teixeira
layout: post
guid: http://rafaelmt.github.io/?p=85
permalink: /en/2015/08/19/managing-logs-with-the-elk-stack/
dsq_thread_id:
  - "5225007025"
categories:
  - SysAdmin
  - Tutorial
tags:
  - elasticsearch
  - elk
  - kibana
  - logstash
  - tutorial
---
Consolidating, indexing and analyzing your environment's logs is fundamental. Gone are the days when truckloads of logs were stored only to be used when something went wrong. It's possible to extract a lot of information from your infrastructure and applications' logs, allowing you to rapidly detect abnormalities, foresee problems and even support business decisions.

There are plenty of cloud log management services around: <a href="http://www.loggly.com" target="_blank">Loggly</a>, <a href="http://papertrailapp.com" target="_blank">Papertrail</a> e <a href="https://logentries.com/" target="_blank">Logentries</a> are some of the most used. Their goal is to be as simple as it gets: create an account, redirect your logs to the provided endpoint and that's it: the rest is on their side. I've used these three and I might write about them some day, but we have another objective in this post: I'll show how you can create a flexible and powerful log manager using the Elasticsearch/Logstash/Kibana stack.

<!--more-->
<h2>ELK?</h2>
<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/logos1.png"><img class="wp-image-119 aligncenter" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/logos1.png" alt="logos" width="578" height="263" /></a>

All three components of the ELK stack are open source (<a href="https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)">Apache 2 license</a>) and kept by Elastic, allowing modification, distribution and commercial use. The components are:

<strong>Elasticsearch: </strong>distributed search engine based on Apache Lucene. It has a RESTful API as interface, with JSON objects.

<strong>Logstash: </strong>data pipeline tool. Centralizes, normalizes and processes data from different sources.

<strong>Kibana:</strong> data visualization web interface. Allows creating graphs and filters for data indexed by Elasticsearch.

This is the data flow throughout the stack: data is generated and sent to Logstash. Logstash reads, parses and filters the data, then sends it to Elasticsearch. Kibana uses Elasticsearch's  indexes to search data and displays it in a user-friendly way.

In this example, we'll use an Apache log as input. The flow is as follows:

&nbsp;

<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/logstash-dataflow-1.png"><img class="size-full wp-image-74 aligncenter" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/logstash-dataflow-1.png" alt="logstash-dataflow-1" width="523" height="309" /></a>
<h2>Installation</h2>
I've used Centos 7 for this tutorial. The commands should work in any RHEL-based distro.
<h3>Java</h3>
Logstash runs both on Oracle JDK and OpenJDK. We'll use OpenJDK:

<code>sudo yum install java-1.8.0-openjdk.x86_64</code>

The stack installation process is pretty straightforward: simply download the components and decompress the files. I'll put them all in the /opt/ folder. I'm using the most recent versions (at the time of writing).
<h3>Elasticsearch</h3>
<code>curl -O https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.7.1.tar.gz</code>
<code>sudo tar -xzvf elasticsearch-1.7.1.tar.gz -C /opt/</code>
<h3>Logstash</h3>
<code>curl -O https://download.elastic.co/logstash/logstash/logstash-1.5.3.tar.gz</code>
<code>sudo tar -xzvf logstash-1.5.3.tar.gz -C /opt/</code>
<h3>Kibana</h3>
<code>curl -O https://download.elastic.co/kibana/kibana/kibana-4.1.1-linux-x64.tar.gz</code>
<code>sudo tar -xzvf kibana-4.1.1-linux-x64.tar.gz -C /opt/</code>
<h2>Creating the Logstash pipeline</h2>
To create a Logstash data pipeline, we need to specify the inputs, filters and outputs.  The inputs and outputs read and write data, while the filters adapt the data format to what the output expects. Logstash's architecture allows installing inputs, filters and outputs as plugins, as well as creating your own.

Let's create a pipeline with a an Apache access log as input and Elasticsearch as output. The scenario is as follows:

<strong>input:</strong> reads a file containing Apache access log

<strong>filter: </strong>parses each log line in a JSON object to be used as input in Elasticsearch

<strong>output: </strong>sends the JSON objects to an Elasticsearch instance

We'll use the <a href="https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html" target="_blank">Grok filter</a> to transform text from the log file into structured data. Grok allows parsing input text using patterns. It ships with a number of <a href="https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns" target="_blank">different patterns</a>, Apache being one of them.

The <em>file</em> input transforms each line of the log file in an event. The Grok filter reads the events, finds the fields matching the pattern (ip, request type, response etc.) and adds them to the event. The Elasticsearch output gets the events, transforms them into JSON objects and sends it to an Elasticsearch instance.

This is the configuration file for the pipeline:
[gist 4e0c4561471f8a00f428]

Change /path/to/apache_log to (guess what ...) the path where apache log is in your machine and save it. If you don't have an Apache log to test, use <a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/access_log.tgz">this one</a>. Decompress it with <code>tar -xzvf access_log.tgz</code>

You might have noticed that, contrary to what one could expect, the elasticsearch output does not specify the Elasticsearch instance's address. Since Elasticsearch is running in the same machine as Logstash and is configured with Multicast enabled by default, it will be automatically found. This configuration is <strong>not recommended for production environments.</strong>
<h2>Running</h2>
Let's start by initializing Elasticsearch:

<code>&lt;elasticsearch_root_folder&gt;/bin/elasticsearch -d</code>

Now Logstash:

<code>&lt;logstash_root_folder&gt;/bin/logstash -f pipeline.conf &amp;</code>

And now Kibana. Notice that we didn't edit any configuration files for Kibana: by default, it will look for an Elasticsearch instance in localhost:9200 (which is exactly where our elasticsearch is).

<code>&lt;kibana_root_folder&gt;/bin/kibana &amp;</code>

Now browse to the port 5601 of your machine. You should see something like this:

<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/Screen-Shot-2015-08-18-at-10.27.34-PM.png"><img class="alignnone size-full wp-image-71" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/Screen-Shot-2015-08-18-at-10.27.34-PM.png" alt="kibana-dashboard" width="1280" height="627" /></a>

There you go! You have an open source, flexible and scalable log management system. You can read a little more about Kibana usage <a href="http://rafaelmt.github.io/en/2015/09/01/kibana-tutorial/">in this post</a>.

&nbsp;

[yasr_visitor_votes size="small"]
