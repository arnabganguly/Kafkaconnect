
## Configure Kafka Connect in Standalone Mode


### Acquire the Zookeeper and Kafka broker data 

  - Set up password variable. Replace `PASSWORD` with the cluster login password, then enter the command
 ```
 export password='PASSWORD' 
```
   - Extract the correctly cased cluster name

```
export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
```

- Extract the Kafka Zookeeper hosts

```
export KAFKAZKHOSTS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2);
```
- Validate the content of the `KAFKAZKHOSTS` variable
```
echo  $KAFKAZKHOSTS
```
- Zookeeper values appear in the below format . Make a note of these values as they will be used later
```
zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
```

- To extract Kafka Broker information into the variable KAFKABROKERS use the below command

```
export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
```

- Check to see if the Kafka Broker information is available
```
echo $KAFKABROKERS
```
- Kafka Broker host information appears in the below format
```
wn1-kafka.eahjefyeyyeyeyygqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eaeyhdseyy1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092
```


### Configure the nodes for Kafka Connect in Distributed mode


#### Create the topics you will need 

- Create the **Offset Storage** topic with a name of your choice . Here we use *agconnect-offsets*
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-offsets --zookeeper $KAFKAZKHOSTS
```

- Create the **Config Storage** topic with a name of your choice. Here we use *agconnect-configs* 
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-configs --zookeeper $KAFKAZKHOSTS
```
- Create the **Status** topic with a name of your choice. Here we use *agconnect-status* 
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-status --zookeeper $KAFKAZKHOSTS
```

- Create the Topic for storing Twitter Messages. Here we use *twitterstatus*
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic twitterstatus --zookeeper $KAFKAZKHOSTS
```


### Deploy the Kafka Connect Plugins

- Download the relevant Kafka Plugins from the [Confluent Hub](https://www.confluent.io/hub/) to your local desktop 
     - [Kafka Connect plugin for streaming data from Twitter to Kafka](https://www.confluent.io/hub/jcustenborder/kafka-connect-twitter).
     - [Azure Blob Storage Sink Connector](https://www.confluent.io/hub/confluentinc/kafka-connect-azure-blob-storage)
- Unzip the files to create the folder structures

     ![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic9.png)

  -  Create a new folder path on the edge node
  ```
     sudo mkdir /usr/hdp/current/kafka-broker/connectors
     sudo chmod 777 /usr/hdp/current/kafka-broker/connectors 
     ``` 
- Using WINSCP or any other SCP tool of your choice upload the Kafka Connect Plugins into folder path created in the last step

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic11.png)


![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic12.png)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzNzk1MzYxNl19
-->