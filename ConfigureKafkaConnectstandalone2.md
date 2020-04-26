## Start source tasks and sink tasks 

- From the edge node  run each of the below in a **separate session** to create new connectors and start tasks.  

**Start Source connector** 
```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```
- If the connector is created and tasks are started you will see the below notifications

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic21.png)


- Message ingestion from Twitter will start immediately thereafter 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic22.png)

- One other to way to test if Twitter Messages with the keywords are indeed being ingested is to start a console consumer in a fresh session and start consuming messages from topic *twitterstatus* .In a new session , launch a console consumer. (Make sure *$KAFKAZKHOSTS* still holds values of Kafka brokers)

```
/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKAZKHOSTS --topic twitterstatus 
```
- If everything is working , you should see a stream of relevant Twitter Messages on the console with specified keywords. 


**Start Sink Connector**

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```

- If the connector is created and tasks are started you will see the below notifications

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic23.png)

- Messages from Twitter will written to the Azure Blob Store 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic24.png)

- This ends the lab for Kafka Connect on standalone mode. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzNzIyMzcxOCwtMzYzNDc5MjY3LC02Nj
U0NzU5MDMsMTk0OTIzMzcyNCwtMTg4MDQ4MDk1OSw3NjQ0MTc1
MDYsLTExMzgwMzE0MDYsNjM0MzAxODM2LDE4OTc3MzAyMDYsMT
A3MjUwOTk1MV19
-->