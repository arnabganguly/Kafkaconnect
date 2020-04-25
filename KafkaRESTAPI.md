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
curl -X GET http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors

curl -X GET http://ed12-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/Twitter-to-Kafka

curl -X GET http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/connector1/tasks

curl -X POST http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/connector1/restart

curl -X DELETE http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/connector1/


curl -X PUT http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/connector1/pause

curl -X PUT http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/blob-sink/resume



curl -X PUT http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors/connector1/resume 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzODQzNzY3NF19
-->