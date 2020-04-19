# Kafka Connect with HDInsight Managed Kafka 

In a normal Kafka cluster a producer application produces a message and publishes it to Kafka and a consumer application consumes the message from Kafka. 

In these circumstances it is the application developer's responsibility to ensure that the producer and consumers are reliable and fault tolerant. 

**Kafka Connect is a framework for connecting Kafka with external systems**  such as databases, storage systems, applications , search indexes, and file systems, using so-called  _Connectors_, in a reliable and fault tolerant way.

**Kafka Connectors are ready-to-use components, which can help import data from external systems into Kafka topics and export data from Kafka topics into external systems**. Existing connector implementations are normally available for common data sources and sinks with the option of creating ones own connector.

A  _source connector_ collects data from a system. Source systems can be entire databases, applications or message brokers. A source connector could also collect metrics from application servers into Kafka topics, making the data available for stream processing with low latency.

A  _sink connector_  delivers data from Kafka topics into other systems, which might be indexes such as Elasticsearch, storage systems such as Azure Blob storage, or databases.

**Most connectors are maintained by the community, while others are supported by Confluent or its partners at [Confluent Connector Hub](https://www.confluent.io/hub/). One can normally find connectors for most popular systems like Azure Blob ,Azure Data Lake Store, Elastic Search etc. 


![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic1.png)



### Lab Objective 
- This lab explores ways to use **Kafka Connect** on an HDInsight Managed Kafka Cluster in both **Standalone Mode** and **Distributed Mode**. - The connect cluster in both the setups would ingest messages from twitter and  write them to an Azure Storage Blob. 

#### Standalone Mode 
- Single edge on the HDInsight cluster will used to demonstrate a standalone mode setup. 

#### Distributed Mode 
-  Two edge nodes on an HDInsight cluster will be used to demonstrate a distributed mode setup. 

 - Scalabilty is achieved in Kafka Connect with the addition of more edges nodes an HDInsight cluster [either at the time of creation](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apps-use-edge-node#add-an-edge-node-when-creating-a-cluster) or [post creation](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apps-use-edge-node#add-an-edge-node-to-an-existing-cluster). 

- Since the number of Edge nodes can be scaled up or down on an existing cluster , this functionality can be used to scale the size of the Kafka Connect cluster as well.



![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic2.png)

Click [**Next**](https://github.com/arnabganguly/Kafkaconnect/blob/master/HDInsightManagedKafka.md) to start the Lab 


<!--stackedit_data:
eyJoaXN0b3J5IjpbNTYzNzk4NTgzLDcyNTMyNjkyNCwxNDYwOT
c0ODA0LDgwMTU4MjIyMiwxOTA1MDMwNzcsMTI2MjkwNzU2Mywt
MTg1NTU4MTQ2MywxNjM1NzEzNzU1LC05NzA2MDkxOTUsMjAyMz
I5ODA3MywtNDQwNTgzOTY3LC0xMjY2NzcwNTI1LDE0OTE1MzY2
MSw2NTU4MzE5NDksODUyMzAxNDU1LDI3MDUzOTY2OV19
-->