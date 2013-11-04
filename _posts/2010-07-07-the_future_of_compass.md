---
layout: post
title: The Future of Compass &amp; Elasticsearch
category: articles
---

Its been a long time since I blogged about Compass, and I guess its about time to discuss [Compass](http://www.compass-project.org), [ElasticSearch](http://www.elasticsearch.org), and how they relate to one another.

I started Compass six years ago with a real belief that search is something that any application should have (and search here is not just full text search), and the aim was to have search integrated into a Java application as simple as possible.

Compass has pioneered some really exciting features, including the ability to map your domain model to a search engine (OSEM), later also XML and JSON support, and integration with other ORM libraries (Hibernate, JPA, and so on) to try and make the integration of search as seamless as possible to your typical Java stack application (which has been copied quite nicely by others as well ;) ).

During the lifecycle of Compass, I have also tried to address the scalability aspects of a search solution. By integrating with solutions such as GigaSpaces, Coherence, and Terracotta, the aim was to try and make a search based solution more scalable and usable by applications.

About 8 months ago, I started to think about Compass 3.0. I knew that it required a major rewrite in how it uses Lucene (Lucene 2.9 came with several major changes internally, mainly in how it handles low level readers and search), and also in how to better create a scalable search solution, being able to scale from a single box to many easily and seamlessly. The changes did not end there, I also wanted to create a solution where adding more advance search features, such as facets and others, would be simple.

The more I thought about it, the more I understood that this basically entitles a complete rewrite of Compass if its going to be done correctly. Also, I wanted to bring to the table the experience I had with search over the past years and how search should be done, which is hard with an existing codebase.

This is an important point, especially when it comes to scalable search, which I would like to touch on. The way that I started with trying to solve scalable search using Compass is by creating a distributed Lucene Directory implementation. Of course, this does gets you a bit further down the scalability road, but its very evident for people knowing how Lucene works (or, for that matter, search engines) that this is not the preferred solution (I knew it as I was writing it). Even going up the stack and creating something similar to Lucandra won't cut it... . The proper way to solve the scalability problem is by running a "local" index (a shard) on each node, and do map reduce when you execute a search, and routing when you index (this is a very simplistic explanation).

So, I started out building elasticsearch. Its basically a solution built from the ground up to be distributed. I also wanted to create a search solution that can be used by any other programming language easily, which basically means JSON over HTTP, without sacrificing the ease of use within the Java programming language (or more specially, the JVM).

To be honest, I am amazed at what has happened in just 8 months. ElasticSearch is up and running, providing all the core features I wanted it to have at the beginning. Its a scalable search solution, with a JSON over HTTP interface as well as really nice "native" Java API (it gets nicer in the upcoming 0.9 release).

Sadly, I have been spending a lot of time on elasticsearch, and almost no time on Compass itself, especially around the forum. For that, I deeply apologize to the amazing Compass users that have been there over the years.

So, what about the future of Compass? I see ElasticSearch as Compass 3.0. Thats how I would have wanted the next major version of Compass to look like. This is not to say that the current ElasticSearch version implements all of Compass features, but the basis is there. The two main features that are missing are OSEM, and ORM integration.

As for OSEM, ElasticSearch can already index JSON (and JSON like structure, for example, in the form of a Map). What is left to be done is to create a mapping layer from the object model to this JSON like structure. ORM level integration should work in very similar to how Compass implements it today.

In terms of Java (JVM) level integration, ElasticSearch can easily either start embedded or remote to the Java process, both in distributed mode or in a single node mode.

So, what should someone do today? If you are going to start a new project, I would suggest you take ElasticSearch for a spin, I am sure you will like it. Existing Compass users should start to give serious thought as to how to migrate to ElasticSearch. Hopefully, once OSEM is implemented in ElasticSearch, the migration will be simpler.

Regarding the current Compass 2.x version, its basically in maintenance mode. I will try and help in the forum as much as I can. Will gladly accept patches and apply them to trunk and maybe even release a minor version for it. If someone would like to get more involved with it (administer the forum, help with the patches, releases, commit permission, and so on), I would be happy for it.

As far as I am concerned, the future is ElasticSearch. It is probably the most advanced open source distributed search solution you can find today, and its integration with Java (JVM) is a first class citizen. I hope that Compass user base will follow... .
