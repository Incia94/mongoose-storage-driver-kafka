# Introduction

Kafka storage driver for Mongoose

# Features
+ Item types:
  * `data item` - Record (ProducerRecord, ConsumerRecord)
  * `path` - Topic
+ Data item operation types:
  * `create`
  * `read`
+ Path item operation types:
    * `create`
    * `read`
    * `delete`
    * `list`
`
# Design

| Kafka | Mongoose |
|---------|----------|
| Record | *Data Item* |
| Topic | *Path Item* |
| Partition | N/A |
## Record Operations

Mongoose should perform the load operations on the *records* when the configuration option `item-type` is set to `data`.

### Create
`ProducerApi` has a `KafkaProducer` class with function  [send()](http://kafka.apache.org/21/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html#send-org.apache.kafka.clients.producer.ProducerRecord-org.apache.kafka.clients.producer.Callback-), which can send a record to topic.
* Steps:

### Read
`ConsumerApi` has a `KafkaConsumer` class, provided with function [poll()](https://kafka.apache.org/10/javadoc/org/apache/kafka/clients/consumer/Consumer.html#poll-long-). According to Kafka documentation, on each poll Consumer  begins to consume records from last offset.
* Steps:

### Update
Not supported. 

### Delete
[deleteRecords()](http://kafka.apache.org/21/javadoc/org/apache/kafka/clients/admin/AdminClient.html#deleteRecords-java.util.Map-org.apache.kafka.clients.admin.DeleteRecordsOptions-) function from AdminClient(AdminClient API) class, deletes all records before the one with giving offset.

### List
Not supported.

## Topic Operations

Mongoose should perform the load operations on the *topic* when the configuration option `item-type` is set to `path`.
Apache Kafka has `AdminClient Api`, which provides function for managing and inspecting topics. 
### Create
[createTopics()](http://kafka.apache.org/21/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html) creates a batch of new topics.
* Steps:

### Read
Read all records at once.
### Update
Not supported

### Delete
[deleteTopics()](http://kafka.apache.org/21/javadoc/org/apache/kafka/clients/admin/AdminClient.html#deleteRecords-java.util.Map-) deletes a batch of topics.
* Steps:

### List
[listTopics()](http://kafka.apache.org/21/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html) returns list of topics
* Steps:

## Specific Configuration Options

| Name | Type | Default Value | Description |
|---------|----------|----------|----------|
| storage-driver-read-timeoutMillis | integer | 300000 | The event read and create timeout in milliseconds |
| storage-driver-create-key-enabled | boolean | false | Creates a record with or without a key |
| storage-driver-send-bufferBytes | integer | 131072 | The size of the TCP send buffer to use when sending data. If the value is -1, the OS default will be used. |
| storage-driver-receive-bufferBytes | integer | 32768 | The size of the TCP receive buffer to use when reading data. If the value is -1, the OS default will be used. |
| storage-driver-request-size | integer | 1048576 | The maximum size of a request in bytes. This setting will limit the number of record batches the producer will send in a single request to avoid sending huge requests. |
| storage-driver-batch-size | integer | 16384 | Allows you to change the batch size |
| storage-driver-linger-ms | integer | 0 | The delay before sending the records. This setting gives the upper bound on the delay for batching: once we get *batch.size* worth of records for a partition it will be sent immediately regardless of this setting, however if we have fewer than this many bytes accumulated for this partition we will 'linger' for the specified time waiting for more records to show up. |
| storage-driver-buffer-memory | long | 33554432 | The total bytes of memory the producer can use to buffer records waiting to be sent to the server. |
| storage-net-node-connection | list | "" | A list of host/port pairs to use for establishing the initial connection to the Kafka cluster.  This list should be in the form *host1:port1*,*host2:port2* |