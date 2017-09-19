# Rabbit or Kafka

Mainly depends on the problem in hand. 
 - RabbitMQ is more suitable for system integration where features like acknowledgments, deeper management of messages are important
 - Use Kafka, when High ingestion platforms where speed, scale and efficiency is the prime concern and both the consumer and the producer can operate at relatively the same speed.

### Rabbit MQ Features
1. Communication in RabbitMQ can be either synchronous or asynchronous as needed. 
2. Decouples producers from queues via exchanges ensures that producers aren't burdened with hardcoded routing decisions. 
3. RabbitMQ also offers a number of distributed deployment scenarios (and does require all nodes be able to resolve hostnames). It can be setup for multi-node clusters to cluster federation and does not have dependencies on external services
4. RabbitMQ  has build in clustering, heartbeating and things that Kafka is just starting to get

### Kafka Features
1. Kafka employs a dumb broker and uses smart consumers to read its buffer. 
    - Kafka does not attempt to track which messages were read by each consumer and only retain unread messages; rather, Kafka retains all messages for a set amount of time, and consumers are responsible to track their location in each log (consumer state). 
2. Kafka can support a large number of consumers and retain large amounts of data with very little overhead. 
3. Kafka does require Zookeeper and therefore depends on external services.
4. Kafka presumes that producers generate a massive stream of events on their own timetable - there's no room for throttling producers because consumers are slow, since the data is too massive.
5. Finally, an important difference between these two systems is that Rabbit offers per-message acknowledgments, while with Kafka you can only acknowledge all messages up to an offset. This has important consequences if processing a message fails: with Rabbit, processing will be re-tried. With Kafka, you can either drop the message or fail processing of a whole batch.
6. In general Kafka is optimized for work with “fast” consumers. It can work with “slow” consumers pretty well, however partition-centric design has some consiquences that make dealing with some critical situations difficult.
    - The major limitation is that each partition can have only one logical consumer (in the consumer group). So it means that if we are working with “slow” messages single issue (slow processing) blocks all other messages submitted to this partition after that message. Different strategies can be used to resolve this issues, one of them is to run another consumer group and sychronize it with existing consumer somehow to avoid duplicated processing of messages.

### Conclusion
> Kafka is good for “fast” and reliable consumers while RabbitMQ is good for “slow” and unreliable consumers. When messaging is being used for system integration (which can constitute a wide variety of publishers and consumers that can operate at their on speed, RabbitMQ provides a much richer set of functionality to deal with it.

### References
1. [Replay capability tool](http://qdb.io/)
2. [CloudKarafka](https://www.cloudkarafka.com/)
3. [yurisubach blog](https://yurisubach.com/2016/05/19/kafka-or-rabbitmq/)
4. [Kafka opinion](https://gist.github.com/markrendle/26e423b6597685757732)
5. [Quora - article](https://www.quora.com/What-are-the-differences-between-Apache-Kafka-and-RabbitMQ)
