## Start source tasks and sink tasks 


### Source Task

- From any edge node run the below to create a new connector and start tasks. Note that the number of tasks can be increased as per the size of your cluster. 
```
curl -X POST http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors -H "Content-Type: application/json" -d @- <<BODY
  {
      "name": "connector1",
      "config": {
          "name": "connector1",
          "connector.class": "com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector",
          "tasks.max": 3,
          "kafka.status.topic":"twitterstatus",
          "kafka.delete.topic":"twitterdelete",        
          "topic": "twitter1",   
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


- One way to test if Twitter Messages with the keywords are being ingested is to start a console consumer in a different session and start consuming messages from topic *twitterstatus* . 

```
/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKAZKHOSTS --topic twitterstatus 
```
- If everything is working , you should see a stream of relevant Twitter Messages on the console with specified keywords.  


### Sink Task 


- From any edge node run the below to create a new connector and start tasks. Note that the number of tasks can be increased as per the size of your cluster. 

```
curl -X POST http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors -H "Content-Type: application/json" -d @- <<BODY
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


- Use the Kafka REST API to check if the connector `Kafka-to-Blob`was created 
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTQ3MTcyMzMsMTAwMzUwMDc2MywtMT
YwNTk3MzMwNSwxMTc4NzY3NDY1XX0=
-->