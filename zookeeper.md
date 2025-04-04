# Zookeeper

Zookeeper is a distributed, open-source coordination service for Kafka. It provides services such as:

- Configuration Management: Storing and managing configuration data for distributed components.
- Naming Service: Providing a hierarchical namespace for organising and referencing components.
- Distributed Synchronisation: Enabling coordination and synchronisation between processes in a distributed system.
- Group Services: Managing group membership and leader election.

As a distributed system, Kafka runs on a cluster of brokers, each responsible for handling partitions of topics. To ensure smooth operation and data consistency, Kafka relies on Zookeeper for several critical tasks:

- Broker Registration and Discovery
    - When a Kafka broker starts, it registers itself with Zookeeper, which allows other brokers and clients to discover its existence and location.
    - Zookeeper maintains a dynamic list of active brokers, enabling the cluster to adapt to changes in membership.
- Controller Election
    - One broker in the Kafka cluster is designated as the controller, which is responsible for managing partition and replica states, handling leader elections, and performing other cluster-wide operations.
    - Zookeeper facilitates the election of the controller. If the current controller fails, Zookeeper ensures a new controller is elected quickly.
- Topic Configuration and Metadata Management
    - Zookeeper stores metadata about Kafka topics, including partition assignments, replica locations, and configuration settings.
    - This centralised storage allows all brokers and clients to access consistent information about the cluster's state.
- Ledaer Election for Partitions
    - Each partition in a Kafka topic has a leader broker responsible for handling read and write requests.
    - Zookeeper helps elect the leader for each partition. If a leader fails, Zookeeper triggers the election of a new leader form the available replicas.