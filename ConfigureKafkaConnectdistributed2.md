## Create connectors and start tasks

**Create a Twitter App and get the credentials** 
 - Go to
   [https://dev.twitter.com/apps/new](https://dev.twitter.com/apps/new
   "https://dev.twitter.com/apps/new") and log in, if necessary
 - Enter your Application Name, Description and your website address. You can leave the callback URL empty.
 - Accept the TOS, and solve the CAPTCHA.
 - Submit the form by clicking the **Create your Twitter Application**
 - Copy the below information from the screen for later use in your properties file.
```
twitter.oauth.consumerKey
twitter.oauth.consumerSecret
twitter.oauth.accessToken
twitter.oauth.accessTokenSecret
```
![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic25.png)




### Source Task

- From any edge node run the below to create a new connector and start tasks. Note that the number of tasks can be increased as per the size of your cluster. 
```
curl -X POST http://<edge-node-FQDN>:8083/connectors -H "Content-Type: application/json" -d @- <<BODY
  {
      "name": "Twitter-to-Kafka",
      "config": {
          "name": "Twitter-to-Kafka",
          "connector.class": "com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector",
          "tasks.max": 3,
          "kafka.status.topic":"twitterstatus",
          "kafka.delete.topic":"twitterdelete",        
          "twitter.oauth.consumerKey":"<twitter.oauth.consumerKey>",
          "twitter.oauth.consumerSecret":"<twitter.oauth.consumerSecret>",
          "twitter.oauth.accessToken":"<twitter.oauth.accessToken>",
          "twitter.oauth.accessTokenSecret":"<twitter.oauth.accessTokenSecret>",
          "filter.keywords":"<keyword>",
          "process.deletes":false
      }
  }
BODY 
```
- If the connector is created , you will see a notification like below.

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic15.png)

- Use the Kafka REST API to check if the connector `Twitter-to-Kafka`was created 

 ```
curl -X GET http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors
["local-file-source","Twitter-to-Kafka"]
```

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic16.png)


- One way to test if Twitter Messages with the keywords are being ingested is to start a console consumer in a different session and start consuming messages from the topic *twitterstatus* . 

```
/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKAZKHOSTS --topic twitterstatus 
```
- If everything is working , you should see a stream of relevant Twitter Messages on the console with specified keywords. 

- Try pausing the tasks in the connector , this should also **pause** the Twitter Stream on the console producer.

```
curl -X PUT http://<edge-node-FQDN>:8083/connectors/Twitter-to-Kafka/pause 
```
- Try resuming the tasks in the connector , this should also **resume** the Twitter Stream on the console producer.

```
curl -X PUT http://<edge-node-FQDN>:8083/connectors/Twitter-to-Kafka/resume 
```

### Sink Task 

- Create a regular [Azure Blob storage account and a container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) on Azure and note the storage access keys 

- From any edge node run the below to create a new connector and start tasks. Note that the number of tasks can be increased as per the size of your cluster. 

```
curl -X POST http://<edge-node-FQDN>:8083/connectors -H "Content-Type: application/json" -d @- <<BODY
  {
      "name": "Kafka-to-Blob",
      "config": {
          "connector.class": "io.confluent.connect.azure.blob.AzureBlobStorageSinkConnector",
          "tasks.max": 1,
          "topics":"twitterstatus",
          "flush.size":3,
          "azblob.account.name":"<Storage-account-name>",
          "azblob.account.key":"<Storage-accesss-key>",
          "azblob.container.name":"<Container-name>",
          "format.class":"io.confluent.connect.azure.blob.format.avro.AvroFormat",
          "confluent.topic.bootstrap.servers":"Enter the full contents of $KAFKAZKHOSTS",   
          "confluent.topic.replication.factor":3

      }
  }
BODY
```
- If the connector is created  , you will see a notification like below.

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic17.png)


- Use the Kafka REST API to check if the connector `Kafka-to-Blob`was created. You should see both the source and sink connectors.  

```
curl -X GET http://<edge-node-FQDN>:8083/connectors
["local-file-source","Twitter-to-Kafka","Kafka-to-Blob"]
```

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic18.png)

- Authenticate into your Azure portal and navigate to the storage account to validate if Twitter Messages are being sent to the specific container. 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic19.png)

 ![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic20.png)


- In this section we saw how the source and sink connectors were created . In the next section , we will explore some Kafka REST API's to control Kafka Connect.  

Click  [Next ->](https://github.com/arnabganguly/Kafkaconnect/blob/master/KafkaRESTAPI.md)  
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzOTE0MDQ4LC0yMTI5MDAxMTM3LC00MD
E2Nzk4MTAsNDk4ODQxNjk5XX0=
-->