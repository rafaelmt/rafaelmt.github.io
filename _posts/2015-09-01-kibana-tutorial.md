---
id: 124
title: Kibana Tutorial
date: 2015-09-01T00:30:20+00:00
author: Rafael M Teixeira
layout: post
guid: http://rafaelmt.github.io/?p=124
permalink: /en/2015/09/01/kibana-tutorial/
dsq_thread_id:
  - "5225005824"
categories:
  - Sem categoria
---
In the <a href="http://rafaelmt.github.io/en/2015/08/19/managing-logs-with-the-elk-stack/">Managing Logs with the ELK Stack</a> post, we have installed and configured an ELK stack to consolidate and analyze logs from an Apache Server. In this post I'll talk a little more about Kibana and how to use it to create data charts and filter data.

<!--more-->
<h2>Getting Started</h2>
<a id="refresh-fields"></a>First step is reloading the fields. This step is necessary so Kibana knows that the Apache log fields are indexed and can be used for searches. To do so, go to Settings -&gt; Indexes. In the left-hand bar you'll see all Elasticsearch indexes Kibana will load. This allows us to restrict Kibana's access to the analyzed indexes only. Kibana comes with the Logstash index's name pattern by default. If it does not happen, click "<em>Add new"</em> and create an index with <em>logstash-* </em>as pattern.

<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/1-reindex.png"><img class="wp-image-105 aligncenter" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/1-reindex.png" alt="1-reindex" width="475" height="201" /></a>

To reload the fields, choose Logstash index and press the orange <em>refresh</em> button on the top right corner. The Apache log fields (path, request, agent etc.) should be displayed as <em>indexed</em> in the field list.

Let's switch to the "Discover" tab. You will see a chart containing the amount of entries per day and the apache log entries in unparsed text. If you don't see those, increase the analysis period on the top right corner. If logs still don't appear, check if they are correctly loaded in Elasticsearch with the following commands:

<code>curl 'localhost:9200/_cat/indices?v'</code> - List all Elasticsearch indexes

<code>curl -XGET 'localhost:9200/&lt;nome_do_indice&gt;/_search?'</code> -  List all entries in an index.

The index' fields are listed in the left-hand tab. Clicking them will show a count of occurrences. You can display the selected fields only instead of log text by clicking "<em>add</em>".

If you see "This field is not indexed thus unavailable for visualization and search" ou "unindexed fields cannot be searched" when trying to add the Apache fields you need to <a href="#refresh-fields">reload the fields</a>
<h2>Creating Visualizations</h2>
To create a vertical bar graph containing the count of every value in a field, select it and click "Visualize":

<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/agent-chart.png"><img class=" wp-image-110 alignnone" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/agent-chart.png" alt="agent-chart" width="695" height="341" /></a>

Save the visualization by clicking the top right corner <em>save</em> button.

There are other forms of data visualization. In the <em>Visualize</em> you will see a list of possible visualizations and its use cases. Let's create another visualization to count the errors 500 returned by the server. Select the "Metric" visualization and in the next step choose "From a new search".

Kibana's search syntax is the same as Elasticsearch's (with is the same as Apache Lucene's). The <code>response: 500</code> query returns the responses with code 500. Replace * for this query. Save it in another visualization.
<h2>Creating a Dashboard</h2>
Change to the Dashboard tab. By clicking + in the top right corner, you can add visualizations to your Dashboard. They can be re-dimensioned and re-organized.

<a href="http://rafaelmt.github.io/wp-content/uploads/2015/08/Screen-Shot-2015-08-25-at-10.56.34-AM.png"><img class="wp-image-112 alignnone" src="http://rafaelmt.github.io/wp-content/uploads/2015/08/Screen-Shot-2015-08-25-at-10.56.34-AM.png" alt="Screen Shot 2015-08-25 at 10.56.34 AM" width="727" height="356" /></a>

&nbsp;

[yasr_visitor_votes size="small"]