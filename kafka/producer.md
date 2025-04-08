# Kafka Producer

A Kafka producer is responsible for publishing records (messages) to Kafka topics. It abstracts the complexities of interacting with the Kafka cluster, allowing applications to focus on generating and delivering data.

## Message Sending Process

1.  Record Creation: The application creates a record containing the message's key, value, and optional headers.
2.  Serialisation: The key and value are serialised into byte arrays using a configured serialiser.
3.  Partitioning: The producer determines the target partition for the record.
4.  Batching: Records are accumulated in a batch in the Records Accumulator.
5.  Sending: The Sender thread sends the batches to the Kafka brokers.
6.  Acknowledgement: The broker acknowledges the receipt of the records.

### Message Structure of a Kafka Producer Record

A record encapsulates the message to be sent. It comprises:

- Topic: The Kafka topic to which the record is published.
- Partition (optional): The specific partition to which the record should be sent.
- Key (optional): A key used for partitioning and compaction.
- Value: The message payload.
- Timestamp (optional): The timestamp of the record.
- Headers (optional): Key-value pairs for metadata.

### Serialisation and Deserialisation

- Serialisation is the process of converting the message key and value from objects to byte arrays. This is crucial for transmitting data over the network. Common serialisers include `StringSerializer`, `ByteArraySerializer`, `JsonSerializer`, `AvroSerializer`, `ProtobufSerializer` and custom serialisers for complex objects.
- Deserialisation is the reverse process, converting byte arrays back to objects, performed by the Kafka consumer.

### Partition Selection Algorithm

- Round Robin: Records are distributed evenly across partitions.
- Key-Based: Records with the same key are sent to the same partition, ensuring ordering for that key. This is the default if a key is provided.
- Partition Designation: The application explicitly specifies the partition in the record.
- Uniform Sticky: This algorithm aims to provide better batching and improve throughput. It selects a partition and then sends records to it until the batch is full or a certain time limit is reached, then selects a new partition.

### Compression

Kafka supports message compression to reduce network bandwidth and storage. Available compression types include:

- GZIP: Good compression ratio but slower.
- Snappy: Faster compression and decompression with a moderate compression ratio.
- LZ4: Very fast compression and decompression with a good compression ratio.

Compression can be configured at the producer level.

### Batching

The Kafka Producer employs a sophisticated batching mechanism to maximise the efficiency of message transmission. This process, facilitated by the Records Accumulator (RA), significantly reduces network overhead and improves throughput. 

The RA buffer stores messages before they are sent to the Kafka brokers to reduce the number of network requests required to send data. It organises messages into batch queues, with each queue corresponding to a specific topic partition, which ensures efficient handling of messages.

The producer further optimises transmission by "piggybacking" new messages onto existing batches that are already in the process of being sent. This technique maximises the utilisation of each network transmission, ensuring that as much data as possible is sent with each network request.

## Idempotent Producer

- Idempotence ensures that each record is written exactly once, even in the event of retries.
- Kafka assigns a unique producer ID (PID) and sequence number to each record. 
- The broker uses the PID and sequence number to detect and discard duplicate records. This prevents data duplication during retries.