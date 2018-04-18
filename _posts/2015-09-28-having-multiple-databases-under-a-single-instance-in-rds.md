---
id: 177
title: Having Multiple Databases under a Single Instance in RDS
date: 2015-09-28T18:07:42+00:00
author: Rafael M Teixeira
layout: post
guid: http://rafaelmt.github.io/?p=177
permalink: /en/2015/09/28/having-multiple-databases-under-a-single-instance-in-rds/
dsq_thread_id:
  - "5225007069"
categories:
  - AWS
  - PostgreSQL
---
A not-so-well documented feature in AWS RDS is the ability of creating multiple databases under a single database instance. This can be a low-cost solution for test environments, since RDS charges per instance. You can lump together multiple test databases into a single instance and save some bucks, for example.

Why not production? All instance resources will be shared among the databases, causing one database to affect the performance of the others. Another issue would be the database parameter group, which would have to be the same for all databases under the instance. All RDS backups are also instance-level, so you would be out of luck if you needed a backup for one of the databases only. The AWS dashboard does not seem to support this either, so you won't see a list of the databases under the instance.
<h3>How To</h3>
There's no way of creating other databases from the AWS dashboard or even from the API. You will have to rely on the DB engine's commands to do it. For PostgreSQL, for example, connect to the instance (psql -h &lt;RDS_endpoint&gt; -U &lt;master_user&gt;) and run the following command:

<code>CREATE DATABASE &lt;new_db_name&gt;;</code>

From now on, you can connect to this DB with the regular psql command. To list the databases under the current instance, run <code>\list</code> (or <code>\l</code>). To switch databases, use <code>\connect &lt;database_name&gt;</code>.