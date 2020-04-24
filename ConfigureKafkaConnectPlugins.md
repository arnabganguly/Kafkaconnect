## Configure Kafka Connect in Distributed Mode

In this section we would configure the Kafka Connect Plugins that we dowloaded earlier .

### Kafka Connect plugin for streaming data from Twitter to Kafka 

**Create a Twitter App and get the credentials** 
 - Go to
   [https://dev.twitter.com/apps/new](https://dev.twitter.com/apps/new
   "https://dev.twitter.com/apps/new") and log in, if necessary
 - Enter your Application Name, Description and your website address. You can leave the callback URL empty.
 - Accept the TOS, and solve the CAPTCHA.
 - Submit the form by clicking the **Create your Twitter Application**
 - Copy the below information from the screen for later use in your properties file.
```
"twitter.oauth.consumerSecret"
"twitter.oauth.accessToken"
"twitter.oauth.consumerKey"
"twitter.oauth.accessTokenSecret":"Efb8bmhX5JAJpPwAdFBN38N9xumur0MdECE6Te8KEdODr", 
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzU1NTc1NzMsNjU5ODQ3ODI5XX0=
-->