# [RabbitMq](https://www.rabbitmq.com/)

### Introduction
RabbitMQ is a message broker: it accepts and forwards messages.

You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that Mr. or Ms. Mailperson will eventually deliver the mail to your recipient.
In this analogy, RabbitMQ is a post box, a post office and a postman.
The major difference between RabbitMQ and the post office is that it doesn't deal with paper, instead it accepts, stores and forwards binary blobs of data ‒ messages.

RabbitMQ, and messaging in general, uses some jargon.
Producing means nothing more than sending. A program that sends messages is a **producer**.
A **queue** is the name for a post box which lives inside RabbitMQ. Although messages flow through RabbitMQ and your applications, they can only be stored inside a queue.
A queue is only bound by the host's memory & disk limits, it's essentially a large message buffer.
Many producers can send messages that go to one queue, and many consumers can try to receive data from one queue.
Consuming has a similar meaning to receiving. A **consumer** is a program that mostly waits to receive messages.

### Work Queues
The main idea behind Work Queues (aka: Task Queues) is to avoid doing a resource-intensive task immediately and having to wait for it to complete.
Instead we schedule the task to be done later.
We encapsulate a task as a message and send it to the queue.
A worker process running in the background will pop the tasks and eventually execute the job.
When you run many workers the tasks will be shared between them.

This concept is especially useful in web applications where it's impossible to handle a complex task during a short HTTP request window.