## Configure Kafka Connect in Distributed Mode


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

#### Configure Kafka Connect 

 - To run Kafka Connect in **distributed mode** one needs to look at two important files. 

  - `connect-distributed.properties` : Located at /usr/hdp/current/kafka-broker/bin/

  - `connect-distributed.sh` : Located at /usr/hdp/current/kafka-broker/conf/

    
- In the  `connect-distributed.properties`  file, define the topics that will store the connector state, task configuration state, and connector offset state. Modify the parameters in `connect-distributed.properties`  file as shown below. Note that we use some of the topics we created 

```
key.converter.schemas.enable=true
value.converter.schemas.enable=true

offset.storage.topic=agconnect-offsets
offset.storage.replication.factor=3
offset.storage.partitions=25

config.storage.topic=agconnect-configs
config.storage.replication.factor=3

status.storage.topic=agconnect-status
status.storage.replication.factor=3
status.storage.partitions=5

offset.flush.interval.ms=10000

rest.port=8083
```



- In distributed mode, the workers need to be able to discover each other and have shared storage for connector configuration and offset data. In addition to the usual worker settings, ensure you have configured the following for the cluster:
    
    - `group.id` : ID that uniquely identifies the cluster these workers belong to. Ensure this is unique for all groups that work with a cluster.
    -   `config.storage.topic`: Topic to store the connector and task configuration state in.
    -   `offset.storage.topic`: Topic to store the connector offset state in. 
    -   `rest.port`: Port where the REST interface listens for HTTP requests. 

    
4.  To run the distributed worker command, `connect-distributed.properties`
    
      ```
    sudo  /usr/hdp/current/kafka-broker/bin/connect-distributed.sh  /usr/hdp/current/kafka-broker/conf/connect-distributed.properties
    ```
    

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE3NzcyNjM1MywtOTI0NTYwOTY0LC0xNT
IxNTI3NTgyLDEzODkzMzMwNTksMTk2MTczNDk0NiwxODIzMTgw
NzE2LC0xMDc0MzUyMzU3LC0xNTcxMDkxNzE5XX0=
-->