---
layout: post
title: Actor Model and Data Grids
category: articles
---

The <a href="http://en.wikipedia.org/wiki/Actor_model">Actor Model</a> is getting a log of (much deserved) hype for the past year. With languages like Scala and Erlang pretty much leading the way in reviving it. 

First, here is what wikipedia has to say about the Actor Model:

> In computer science, the Actor model is a mathematical model of concurrent computation that treats "actors" as the universal primitives of concurrent digital computation: in response to a message that it receives, an actor can make local decisions, create more actors, send more messages, and determine how to respond to the next message received.

With Data Grids, a lot has been already said about collocation. When using a Data Grid, the next logical step (the first is to stop hitting the database so much) is to collocate business logic execution with data. 

One way to collocate business logic with Data can be done by sending the work to the node where the data resides. Data Grid vendors realize that this is a much needed feature and provide means to do so (Coherence with the InvocableService or <a href="http://www.gigaspaces.com/wiki/display/XAP66/Executors+Component">GigaSpaces Executors</a>).

Another way to collocate data with business logic is to actually start the Data Grid node in a collocated manner with a set of business services that interact with it in a collocated and event driven manner (this is called a processing unit in GigaSpaces).

A business service in such a case is a simple service that usually react to events happening in the Data Grid, process them, and produces results that cause other events to happen in the Data Grid.

If we take a step back for just a moment, you can see that the last paragraph sounds very similar to the Actor Model definition (not completely, but we are getting there...).

So, first, what constitutes as an event in a Data Grid? Well, surprise surprise, Data. Data being written / changed / removed from the Data Grid forms events within the Data Grid.

So, lets take a simple service, the service is running on each data grid node (the Data Grid is partitioned) in a collocated manner with each node. The service reacts to events occurring in the Data Grid (an Order has been submitted/written to the Data Grid). It them process the event, and produces more events. More events can be, for example, changing the status of the Order to "Requires Validation" (which will trigger the validation service), as well as write an "Email Message" with a link to the Order into the Data Grid (which will cause another service to email the fact that an Order has been received).

As you can see, the service above is an Actor, but not completely. We need to be able to control the "transactionaility" of the Actor by making the whole process transactional (a good Data Grid is also a transactional Data Grid, especially for collocated executions). 

We also need to make the Actor highly available. In this case, again, it should be simple. Data Grids usually provide the option to run backups for each partitions. We should be able to just create the service on backup nodes as well, and once the backup node becomes primary, the service should kick in and start processing Data. While the service is running collocated with a node that is in backup state, it should be in an "inactive" mode, waiting for a failure of the primary node.

Last, we should be able to make the Actor highly concurrent and thread safe. If the Actor operates solely using the Data Grid API, then is thread safe as Data Grid APIs are thread safe and highly concurrent.  There also should be a way to instantiate several instances of the Actor in order to fully utilize multi core systems.

Up until now, I tried to explain how Data Grids fits very nicely with the Actor Model. Let me finish with a simple example based on GigaSpaces APIs to show how all of the above can be achieved.

<div align="center"><img src="/images/polling_container_basicjpg.jpeg" alt="GigaSpaces Polling Container" title="GigaSpaces Polling Container" width="192" height="232" class="alignncenter" /></div>

The diagram is a simple polling container that wraps a service (Actor, the Order service in our example). The polling container takes (removes) events (Data), process them, and writes back Data to the collocated Space (Data Grid) it is running with. Here is an example of how a service like that can be coded:

{% highlight java %}
@EventDriven @Polling(concurrentConsumers = 3) @TransactionalEvent
public class NewOrderActor {

    @Autowired
    GigaSpace gigaSpace;

    @EventTemplate
    Order newOrder() {
        Order template = new Order();
        template.setState(OrderState.NEW);
        return template;
    }
    
    @SpaceDataEvent
    public Order newOrderArrived(Order newOrder) {
        EmailMessage email = new EmailMessage(newOrder);
        gigaSpace.write(email); // cause event to process emails
        newOrder.setState(Order.REQUIRES_VALIDATION);
        return newOrder;    
    }
}
{% endhighlight %}

The above example will use a template of "new Orders" to receive events. Once a new order is added to a certain primary partition, the polling container will take it and call the newOrderArrived event handler. Within the event handler, a new EmailMessage will be written to the Data Grid which will start a chain of events that will (hopefully, more Actors are needed) send an email. It will also return an updated Order with its state changed that will be written back to the collocated Data Grid. The update Order will cause the OrderValidator actor to kick in and do its magic. Of course, thanks to the @TransactionalEvent annotation, everything will happen under a single transaction.

Thats it. I hope that the "collocation" of Data Grid make sense to you as it make so much sense to me.