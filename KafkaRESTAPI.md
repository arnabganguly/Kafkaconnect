## Kafka REST APIs


### Commonly used REST APIs for Kafka Connect 

- Below are some commonly used REST APIs for controlling KAFKA Connect 

- Status of distributed connect 
```
curl -s http://<edge-node-FQDN>:8083/ |jq
```

- Get list of Connect 
```
curl -X GET http://<edge-node-FQDN>:8083/connector-plugins | jq
```
- Get list of connectors in the cluster 
```
curl -X GET http://<edge-node-FQDN>:8083/connectors
```

- Get Status of connector 
```
curl -X GET http://<edge-node-FQDN>:8083/connectors/<connector-name>
```
- Get connector Tasks
``` 
curl -X GET http://<edge-node-FQDN>:8083/connectors/<connector-name>/tasks
```
- Restart a connector
```
curl -X POST http://<edge-node-FQDN>:8083/connectors/<connector-name>/restart
```

- Delete a connector
``` 
curl -X DELETE http://<edge-node-FQDN>:8083/connectors/<connector-name>/
```
- Pause a connector
```
curl -X PUT http://<edge-node-FQDN>:8083/connectors/<connector-name>/pause
```
- Resume a connector
```
curl -X PUT http://<edge-node-FQDN>:8083/connectors/<connector-name>/resume
```

-  Refer [Apache Kafka documentation](https://kafka.apache.org/documentation/#connect_rest) to get exhaustive list of REST API functions
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTA1MDgyMDA2LC0xNjM3MzkzMTAxXX0=
-->