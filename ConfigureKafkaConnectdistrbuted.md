## Configure the Confluent Schema Registry

- To run the HDInsight worker in distributed mode one needs to look at two important files 


  - `connect-distributed.properties` : Located at /usr/hdp/current/kafka-broker/bin/

  - `connect-distributed.sh` : Located at /usr/hdp/current/kafka-broker/conf/


-   In the  `connect-distributed.properties`  file, define the topics that will store the connector state, task configuration state, and connector offset state.

   In distributed mode, the workers need to be able to discover each other and have shared storage for connector configuration and offset data. In addition to the usual worker settings, ensure you have configured the following for the cluster:
    
    -   group.id: ID that uniquely identifies the cluster these workers belong to. Ensure this is unique for all groups that work with a cluster.
    -   config.storage.topic: Topic to store the connector and task configuration state in.
    -   offset.storage.topic: Topic to store the connector offset state in. 
    -   rest.port: Port where the REST interface listens for HTTP requests. 

    
4.  Run the distributed worker command, `connect-distributed.properties`
    
      ```
    sudo  /usr/hdp/current/kafka-broker/bin/connect-distributed.sh  /usr/hdp/current/kafka-broker/conf/connect-distributed.properties
    ```
    

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzUwMTU0ODUxLDE4MjMxODA3MTYsLTEwNz
QzNTIzNTcsLTE1NzEwOTE3MTldfQ==
-->