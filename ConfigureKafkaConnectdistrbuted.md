## Configure the Confluent Schema Registry

> Note :  The below steps need to be repeatred for both t


- To run the HDInsight worker in distributed mode one needs to look at two important files 


  - `connect-distributed.properties` : Located at /usr/hdp/current/kafka-broker/bin/

  - `connect-distributed.sh` : Located at /usr/hdp/current/kafka-broker/conf/


- Create the config storage topic 


- Create the offset storage topic

    
- In the  `connect-distributed.properties`  file, define the topics that will store the connector state, task configuration state, and connector offset state.

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
eyJoaXN0b3J5IjpbMTM3MzIzMjAzMSwtMTUyMTUyNzU4MiwxMz
g5MzMzMDU5LDE5NjE3MzQ5NDYsMTgyMzE4MDcxNiwtMTA3NDM1
MjM1NywtMTU3MTA5MTcxOV19
-->