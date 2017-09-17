# Rabbit or Kafka

Mainly depends on the problem in hand. 

### Rabbit MQ Features
1. Communication in RabbitMQ can be either synchronous or asynchronous as needed. 
2. Decouples producers from queues via exchanges ensures that producers aren't burdened with hardcoded routing decisions. 
3. RabbitMQ also offers a number of distributed deployment scenarios (and does require all nodes be able to resolve hostnames). It can be setup for multi-node clusters to cluster federation and does not have dependencies on external services 

### Kafka Features
1. Kafka employs a dumb broker and uses smart consumers to read its buffer. 
    - Kafka does not attempt to track which messages were read by each consumer and only retain unread messages; rather, Kafka retains all messages for a set amount of time, and consumers are responsible to track their location in each log (consumer state). 
2. Kafka can support a large number of consumers and retain large amounts of data with very little overhead. 
3. Kafka does require Zookeeper and therefore depends on external services.
4. Kafka presumes that producers generate a massive stream of events on their own timetable - there's no room for throttling producers because consumers are slow, since the data is too massive.
5. Finally, an important difference between these two systems is that Rabbit offers per-message acknowledgments, while with Kafka you can only acknowledge all messages up to an offset. This has important consequences if processing a message fails: with Rabbit, processing will be re-tried. With Kafka, you can either drop the message or fail processing of a whole batch.
