---
layout: post
title: The Birth of Compass
description: "How Compass came to be..."
category: articles
---

It all started when I moved to London. My wife just started to learn at the Cordon Blue to be a chef (yea, I know, I'm a lucky SOB), and she really needed something to help her manage all her recipes. As we all know, most successful open source projects starts with an "itch to scratch", but only slightly less known is, that for most married man, the itch starts with the Misses ;).

Well, me being a geek software developer, I went out and checked all the current things out there, but there was nothing as close to what I envisioned as a good recipe management software. So, I started iCook (yea, I am a Mac geek as well).

While I started writing the software, I was looking for a Job, and I really wanted to start using all the cool/resume enhancing Java projects. It meant that I decided to use <a href="http://www.springframework.org">Spring</a>, <a href="http://www.hibernate.org">Hibernate</a>, and Eclipse RCP (an overkill? Definitely! But hey, I told my wife that I'm going to use it as a learning experience as well, and she, as the usual users, told me that she does not give a crap, as long as it works and works well).

I started to do the usual bits, define a proper domain model, using your usual Spring Dao, Service abstraction, and integrating all that with Eclipse RCP. Being an avid Mac user, I wanted something similar to most Mac software, a great user experience and I consider search to be a vital part of it. Basically it meant having a general search box that would search on all my domain model (Recipes, Articles, Books, Steps, Ingredients, ...). Naturally, I turned to the best Java search engine out there, namely <a href="http://lucene.apache.org">Lucene</a>.

While trying to integrate Lucene into my app (hey, after a couple of days, I got a proper app running, listing and editing Recipes in Eclipse based RCP - the early crappy version) I stumbled into several problems with Lucene.

The first problem with Lucene was transaction support. Before you start, I know, my wife and iCook could not care less about transaction support in her single user recipe manager software, but it persisted in my mind as a drawback for integrating Lucene with a transactional system.

The second problem was the fact that I expected my application to have constant updates to the Lucene index. As most Lucene users know, this is not simple. The deletion of a Document is done using an IndexReader (funny name), and adding documents is done using the IndexWriter. If you add the same Document in Lucene (i.e. have the same business ids), it will be duplicated in the Lucene index unless you delete it first. And in order to get proper performance out of Lucene, you should batch your deletes, and than perform your updates.

The third problem was the hurdle of mapping Lucene Documents into my domain mode. I had to do the old Jdbc to Domain model mapping that you usually do with Databases, and I thought that once I have Hibernate, I don't care about this (as much) anymore. I wanted something similar for Lucene.

Don't get me wrong, Lucene does very well in what it aims to provide, a low level Java search engine. But for me, it did not follow the rule of make simple things simple, and make difficult things possible. When you look at the current successful projects in the Java world, they are projects that help you deliver and develop in a much faster pace, usually breaking current misconceptions or habits.

So I dumped my iCook project for now, and started to work on this project, which was later called Compass. My main requirements for the project at that time was for transaction support, identifiable "Documents", and an Object mapping framework.

Naturally, once I got started, I found out that there are a lot of things missing when you just use Lucene. Caching of IndexReaders and Searchers and invalidating them for better performance, a lower level abstraction than OSEM (my name for Object Search Engine Mapping), which I called RSEM (Resource Search Engine Mapping) - think Spring Jdbc or iBatis for Lucene. Once you have OSEM, it only makes sense to integrate it with ORM tool for seamless integration, and Spring support. And many others.

So the project grew and grew, I decided to release it in Source Forge under an open source license (initially LGPL, later moved to Apache 2, another story for another time), and the end result is that my wife still waits for iCook (it still means that I eat well!), and I spend my days and nights on Compass. I do have a wonderful wife :-).

