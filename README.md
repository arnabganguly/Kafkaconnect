# Kafka Connect with HDInsight Managed Kafka 

In a normal Kafka cluster a producer application produces a message and publishes it to Kafka and a consumer application consumes the message from Kafka. 

In these circumstances it is the application developer's responsibility to ensure that the producer and consumers are reliable and fault tolerant. 

**Kafka Connect is a framework for connecting Kafka with external systems**  such as databases, storage systems, Applications , search indexes, and file systems, using so-called  _Connectors_.

**Kafka Connectors are ready-to-use components, which can help import data from external systems into Kafka topics and export data from Kafka topics into external systems**. Existing connector implementations are normally available for common data sources and sinks with the option of creating ones own connector.

A  _source connector_ collects data from a system. Source systems can be entire databases, applications or message brokers. A source connector could also collect metrics from application servers into Kafka topics, making the data available for stream processing with low latency.

A  _sink connector_  delivers data from Kafka topics into other systems, which might be indexes such as Elasticsearch, storage systems such as Azure Blob storage, or databases.

**Most connectors are maintained by the community, while others are supported by Confluent or its partners.  One can normally find connectors for most popular systems, like Azure Blob , Azure Data Lake Store, Elastic Search etc. 



Below is a representative architecture of how the Schema Registry is deployed on an HDInsight cluster. Note that Schema Registry natively exposes a REST API for operations on it.  Producers and consumers can interact with the Schema Registry from within the VNet or using the [Kafka REST Proxy](https://docs.microsoft.com/en-us/azure/hdinsight/kafka/rest-proxy). 

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/Pic1.png)

Click [**Next**](https://github.com/arnabganguly/Kafkaconnect/blob/master/HDInsightManagedKafka.md) to start the Lab 


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc1MDQ0ODA4OSwxNDYwOTc0ODA0LDgwMT
U4MjIyMiwxOTA1MDMwNzcsMTI2MjkwNzU2MywtMTg1NTU4MTQ2
MywxNjM1NzEzNzU1LC05NzA2MDkxOTUsMjAyMzI5ODA3MywtND
QwNTgzOTY3LC0xMjY2NzcwNTI1LDE0OTE1MzY2MSw2NTU4MzE5
NDksODUyMzAxNDU1LDI3MDUzOTY2OV19
-->