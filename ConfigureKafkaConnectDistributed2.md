## Start source tasks and sink tasks 

- From any edge node run the below to create a new connector. 
```
curl -X POST http://ed10-ag4kac.ohdqdgkr0bpe3kjx3dteggje4c.gx.internal.cloudapp.net:8083/connectors -H "Content-Type: application/json" -d @- <<BODY
  {
      "name": "connector1",
      "config": {
          "name": "connector1",
          "connector.class": "com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector",
          "tasks.max": 1,
          "kafka.status.topic":"twitterstatus",
          "kafka.delete.topic":"twitterdelete",        
          "topic": "twitter1",   
          "twitter.oauth.consumerKey":"LgKBFENGxiPepLt11Cql36T2r",
          "twitter.oauth.consumerSecret":"CZug3RPR01ns5DvBH8LNPvLzHWxgvXOQ97hSFByJ6x393vagFC",
          "twitter.oauth.accessToken":"1022650746-Ujs8mXTAfiQlqgnkspGYNLu2ImYwwCXAm99DwVX",
          "twitter.oauth.accessTokenSecret":"Efb8bmhX5JAJpPwAdFBN38N9xumur0MdECE6Te8KEdODr",
          "filter.keywords":"COVID19",
          "process.deletes":false
      }
  }
BODY 
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzMyMDEwMDgsMTE3ODc2NzQ2NV19
-->