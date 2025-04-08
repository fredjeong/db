# Kafka Consumer

The Kafka Consumer plays a pivotal role in the Apache Kafka ecosystem, responsible for retrieving and processing data streams published to Kafka topics. This article delves into the intricacies of the Kafka Consumer, elucidating its message receiving process, configuration parameters, and core functionalities.

## Fundamental Role of the Kafka Consumer

The Kafka Consumer is designed to subscribe to one or more Kafka topics and retrieve records from the partitions of those topics. It operates as a client that polls the Kafka brokers for new data, enabling applications to consume and process data streams in real-time or near real-time.

## Message Receiving Process

1. Subscription: The consumer initially subscribes to one or more Kafka topics. This subscription informs the Kafka cluster which topics and partitions the consumer intends to read from.
2. Partition Assignment: The Kafka broker, or consumer group coordinator, assigns partitions to the consumer. This assignment ensures that each partition is consumed by only one consumer within a consumer group, maintaining data integrity and order.
3. Polling: The consumer periodically polls the Kafka brokers for new records. This polling mechanism is fundamental to the consumer's operation, enabling it to retrieve data as it becomes available.
4. Deserialisation: Upon receiving records, the consumer deserialises the message's key and value from byte arrays back into their original object format. This process is the inverse of the serialisation performed by the producer.
5. Offset Management: The consumer maintains an offset for each partition it consumes. This offset tracks the position of the last read record. The consumer periodically commits these offsets to Kafka, ensuring that, in the event of a failure, it can resume consumption from the last committed offset.
6. Data Processing: The consumer then processes the deserialised data according to the application's requirements.

### Subscription

- The consumer application uses the Kafka client API to specify the topics it wishes to consume. This can involve subscribing to a single topic or multiple topics.
- Upon subscription, the consumer becomes aware of the partitions within the subscribed topics, although the actual assignment of partitions occurs later.
- Subscriptions are often associated with consumer groups, which enable parallel consumption and horizontal scaling. Consumers within the same group coordinate to ensure that each partition is consumed by only one consumer.

### Partition Assignment

- The Kafka broker designated as the consumer group coordinator manages partition assignments.
- When consumers join or leave a group, or when partitions are added or removed, a rebalance is triggered.
    - During a rebalance, the coordinator revokes and reassigns partitions to maintain a balanced distribution.
    - This process ensures that all partitions are actively consumed and prevents data duplication within a consumer group.
- Kafka guarantees that each partition is consumed by only one consumer within a group, preserving data order and integrity.

### Polling

- The consumer application periodically calls the `poll()` method to retrieve new records from assigned partitions.
- The `poll()` method returns a collection of consumer records, each containing the message's key, value, topic, partition, and offset.
- Polling frequency and timeout are configurable, allowing for optimisation based on latency and throughput requirements.

### Deserialisation

- The consumer must be configured with deserialisers that match the serialisers used by the producer.
- Incorrectly configured deserialisers can lead to errors and data processing failures.

### Offset Management

- The consumer maintains an offset for each partition, indicating the position of the last consumed record.
- Offsets can be automatically committed at regular intervals, simplifying offset management. Or, they can be commited manually for finer control and "at least once" processing semantics.
- Committed offsets ensure that the consumer can resume consumption from the last successfully processed record in case of failures.