# Kafka Connect with HDInsight Managed Kafka 

In a normal Kafka cluster a producer application produces a message and publishes it to Kafka and a consumer application consumes the message from Kafka. 

In these circumstances it is the application developer's responsibility to ensure that the producer and consumers are reliable and fault tolerant. 

**Kafka Connect is a framework for connecting Kafka with external systems**  such as databases, storage systems, Applications , search indexes, and file systems, using so-called  _Connectors_.

**Kafka Connectors are ready-to-use components, which can help import data from external systems into Kafka topics and export data from Kafka topics into external systems**. Existing connector implementations are normally available for common data sources and sinks with the option of creating ones own connector.

A  _source connector_ collects data from a system. Source systems can be entire databases, applications or message brokers. A source connector could also collect metrics from application servers into Kafka topics, making the data available for stream processing with low latency.

A  _sink connector_  delivers data from Kafka topics into other systems, which might be indexes such as Elasticsearch, storage systems such as Azure Blob storage, or databases.

**Most connectors are maintained by the community, while others are supported by Confluent or its partners at [Confluent Connector Hub](https://www.confluent.io/hub/). One can normally find connectors for most popular systems like Azure Blob ,Azure Data Lake Store, Elastic Search etc. 


![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic1.png)




Below is a representative architecture of how **Kafka Connect** is  is deployed on an HDInsight Managed Kafka Cluster in **distributed mode**. Note that the Kafka Connect cluster may be deployed on a set of edge nodes. Edges nodes can be added or deleted from the cluster 

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/Pic1.png)

Click [**Next**](https://github.com/arnabganguly/Kafkaconnect/blob/master/HDInsightManagedKafka.md) to start the Lab 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMTg3NjE5MDQsNzI1MzI2OTI0LDE0Nj
A5NzQ4MDQsODAxNTgyMjIyLDE5MDUwMzA3NywxMjYyOTA3NTYz
LC0xODU1NTgxNDYzLDE2MzU3MTM3NTUsLTk3MDYwOTE5NSwyMD
IzMjk4MDczLC00NDA1ODM5NjcsLTEyNjY3NzA1MjUsMTQ5MTUz
NjYxLDY1NTgzMTk0OSw4NTIzMDE0NTUsMjcwNTM5NjY5XX0=
-->