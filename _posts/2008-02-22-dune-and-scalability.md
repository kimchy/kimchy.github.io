---
layout: post
title: Dune and Scalability
category: articles
---

I am reading the wonderful Dune book again. Another quote from Dune reminded me of problems we have when trying to build a scalable application:

>Kynes looked at Jessica, said: "The newcomer to Arrakis frequently underestimate the importance of water here. You are dealing, you see, with the Law of the Minimum."

>She heard the testing quality in his voice, said, "Growth is limited by the necessity which is present in the least amount. And, naturally, the least favorable condition controls the growth rate."

>"It's rare to find members of a Great House aware of planetological problems," Kynes  said. "Water is the least favorable condition for life on Arrakis. And remember that __growth__ itself can produce unfavorable conditions unless treated with extreme care".

>So true. When we build an application and notice some performance problems, we first need to try and nail down the "least favorable condition". For example, if we have a messaging  system talking to a database, and we notice a bottleneck in the messaging system, we first need to tackle it.

>But, "growth itself can produce unfavorable conditions". We might find that we increased the growth in our messaging system two fold, only to find that our database became the next bottleneck which allowed our system to grow in a much smaller factor.

>Even so, "unfavorable conditions" can be much worse, where we find that our whole architecture can simply not scale anymore, and we have to re-architect it to meet our needs. Extreme care, Kynes said, and he is right... .

