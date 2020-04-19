# Kafka Connect with HDInsight Managed Kafka 

In a normal Kafka cluster a produc

Kafka Schema Registry provides serializers that plug into Kafka clients that handle  message schema storage and retrieval for Kafka messages that are sent in the Avro format. Its used to be a  OSS project by Confluent , but is now under the [Confluent community license](https://www.confluent.io/blog/license-changes-confluent-platform/) . The Schema Registry can additionally serves the below purposes
 
 - Store and retrieve schemas for producers and consumers
 - Enforce backward/forward /full compatibility on Topics
 - Decrease the size of the payload sent to Kafka  

In an HDInsight Managed Kafka cluster the Schema Registry is typically deployed on an Edge node to allow compute separation from Head Nodes. 

Below is a representative architecture of how the Schema Registry is deployed on an HDInsight cluster. Note that Schema Registry natively exposes a REST API for operations on it.  Producers and consumers can interact with the Schema Registry from within the VNet or using the [Kafka REST Proxy](https://docs.microsoft.com/en-us/azure/hdinsight/kafka/rest-proxy). 

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/Pic1.png)

Click [**Next**](https://github.com/arnabganguly/Kafkaconnect/blob/master/HDInsightManagedKafka.md) to start the Lab 


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcyOTI0ODM0OCwxOTA1MDMwNzcsMTI2Mj
kwNzU2MywtMTg1NTU4MTQ2MywxNjM1NzEzNzU1LC05NzA2MDkx
OTUsMjAyMzI5ODA3MywtNDQwNTgzOTY3LC0xMjY2NzcwNTI1LD
E0OTE1MzY2MSw2NTU4MzE5NDksODUyMzAxNDU1LDI3MDUzOTY2
OV19
-->