Copied from
https://gist.github.com/WisdomSky/fd348eb012b8f37f6b9b7dbb69eed6e1

### **# Requirements**



*   Java 11
*   Maven
*   Redis
*   PostgreSQL


### **# Cloning the repository**

Run the following commands:


    $ git clone [https://github.com/signalapp/Signal-Server.git](https://github.com/signalapp/Signal-Server.git)


    $ cd Signal-Server


    $ git checkout 15cf010e449e61d06fed1387abd01d64c5e6ea82

**Note:** The commit hash specified in the checkout command is the **v1.88** version of the source.


### **# Installing Java 11 on Ubuntu**

The original required version of Java for Signal is Java 8, however the PPA for this version has been discontinued. We can install and use Java 11 however there are issues when this version of Java is used for Signal.

You can install Java 11 on Ubuntu by following the steps specified in this page:

[https://tecadmin.net/install-oracle-java-11-on-ubuntu-16-04-xenial/](https://tecadmin.net/install-oracle-java-11-on-ubuntu-16-04-xenial/)


### **# Compiling Signal from Source**

In order to compile the source, you need to install Maven by following the steps specified in this page:

[https://www.vultr.com/docs/how-to-install-apache-maven-on-ubuntu-16-04](https://www.vultr.com/docs/how-to-install-apache-maven-on-ubuntu-16-04)

After successfully installing Maven, you can start compiling the source by running the following command:


    $ mvn install -DskipTests		

When the compilation is successful, a folder named **target **should exist and contains the jar files inside especially the file **TextSecureServer-1.88.jar**. If this file doesn’t exist, it means that the compilation was unsuccessful.


### **# Installing Redis**

To install Redis, just follow the steps specified in this page:

[https://tecadmin.net/install-redis-ubuntu/](https://tecadmin.net/install-redis-ubuntu/)


### **# Installing PostgreSQL**

To install PostgreSQL, just follow the steps specified in this page:

[https://tecadmin.net/install-postgresql-server-on-ubuntu/](https://tecadmin.net/install-postgresql-server-on-ubuntu/)

After installing PostgreSQL, you need to create **two** separate databases, you can just name them **accountdb** and **messagedb** for convention, but you can name them anything as long as you know which is which.


### **# Configuration**

You can find a sample configuration file at **config/sample.yml**. You can create a copy of this file with a different filename.

Here is a sample configuration file with the correct format of values.


    **twilio**: _# Twilio gateway configuration_


    _ **accountId**_: test


     **accountToken**: test


     **numbers**: _# Numbers allocated in Twilio_


    _   _- +181591234567


     **messagingServicesId**:


     **localDomain**: exampleapp.com _# Domain Twilio can connect back to for calls. Should be domain of your service._


    **push**:


     **queueSize**: 10 _# Size of push pending queue_


    _#redphone:_


    _#  authKey: # Deprecated_


    **turn**: _# TURN server configuration_


    _ **secret**_: test _# TURN server secret_


    _ **uris**_:


       - turn:127.0.0.1:3478


    **cache**: _# Redis server configuration for cache cluster_


    _ **url**_: redis://127.0.0.1:6379/1


     **replicaUrls**:


       - redis://127.0.0.1:6379/2


    **directory**: _# Redis server configuration for directory cluster_


    _ **url**_: redis://127.0.0.1:6379/3


     **replicaUrls**:


       - redis://127.0.0.1:6379/4


    **messageCache**: _# Redis server configuration for message store cache_


    _ **redis**_: {**url**: "redis://localhost:6379/5", **replicaUrls**: ["redis://localhost:6379/6"]}


     **cacheRate**: 1


     **persistDelayMinutes**: 10


    **messageStore**: _# Postgresql database configuration for message store_


    _ **driverClass**_: org.postgresql.Driver


     **user**: example


     **password**: 12345678


     **url**: jdbc:postgresql://127.0.0.1:5432/example


    **attachments**: _# AWS S3 configuration_


    _ **accessKey**_: test


     **accessSecret**: test


     **bucket**: exampleapp.com


    **profiles**: _# AWS S3 configuration_


    _ **accessKey**_: test


     **accessSecret**: test


     **bucket**: exampleapp.com


     **region**: us-east-2


    **database**: _# Postgresql database configuration_


    _ **driverClass**_: org.postgresql.Driver


     **user**: example


     **password**: 12345678


     **url**: jdbc:postgresql://127.0.0.1:5432/example2


    **apn**:


     **bundleId**: com.exampleapp.example3


     **pushCertificate**: "-----BEGIN CERTIFICATE-----\r\nMIIFjzCCBHegAwIBAgIIfkSIVVtC9UIwDQYJKoZIhvcNAQEFBQAwgZYxCzAJBgNV\r\nBAYTAlVTMRMwEQYDVQQKDApBcHBsZSBJbmMuMSwwKgYDVQQLDCNBcHBsZSBXb3Js\r\nZHdpZGUgRGV2ZWxvcGVyIFJlbGF0aW9uczFEMEIGA1UEAww7QXBwbGUgV29ybGR3\r\naWRlIERldmVsb3BlciBSZWxhdGlvbnMgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkw\r\nHhcNMTkwNTA5MTc0NzUyWhcNMjAwNTA4MTc0NzUyWjCBjjElMCMGCgmSJomT8ixk\r\nAQEMFWNvbS5xYW5kb3JhcHAucWFuZG9yMzFDMEEGA1UEAww6QXBwbGUgRGV2ZWxv\r\ncG1lbnQgSU9TIFB1c2ggU2VydmljZXM6IGNvbS5xYW5kb3JhcHAucWFuZG9yMzET\r\nMBEGA1UECwwKTlA2UE5GUUdBODELMAkGA1UEBhMCVVMwggEiMA0GCSqGSIb3DQEB\r\nAQUAA4IBDwAwggEKAoIBAQDIo+8GD6chbxCFDI7bTK6iTxQHnD/FiXJvEYBbO3ey\r\nQV8/XZm66VbO+RbA4UArA2bjdW5KZkSQ7hSYJcomQCmcmpqmrXjcaL0KfeWinSz4\r\nmbXxoXK9JsoUSjOfy7yXHGDLZSHGVvn5gO1FU9WPX3Iu7YYMT1hwnMgmZIuzn5K9\r\nAiyxvQzSlIlEsnpoqTDUz+1eXtPXAISPQT8+jM8JoZohTrODuUUj2rSPWi62kMBP\r\nsX4H4ncSjI56DG/E0lvUV7d0PEiUnoxGwzycmGJVgpZqv/E6p1eUNT7Utl4ktuiq\r\nEv8ft3+brJx+FokMi7AKjxLvlYS18LTG6JbQMcmXHsdlAgMBAAGjggHlMIIB4TAJ\r\nBgNVHRMEAjAAMB8GA1UdIwQYMBaAFIgnFwmpthhgi+zruvZHWcVSVKO3MIIBDwYD\r\nVR0gBIIBBjCCAQIwgf8GCSqGSIb3Y2QFATCB8TCBwwYIKwYBBQUHAgIwgbYMgbNS\r\nZWxpYW5jZSBvbiB0aGlzIGNlcnRpZmljYXRlIGJ5IGFueSBwYXJ0eSBhc3N1bWVz\r\nIGFjY2VwdGFuY2Ugb2YgdGhlIHRoZW4gYXBwbGljYWJsZSBzdGFuZGFyZCB0ZXJt\r\ncyBhbmQgY29uZGl0aW9ucyBvZiB1c2UsIGNlcnRpZmljYXRlIHBvbGljeSBhbmQg\r\nY2VydGlmaWNhdGlvbiBwcmFjdGljZSBzdGF0ZW1lbnRzLjApBggrBgEFBQcCARYd\r\naHR0cDovL3d3dy5hcHBsZS5jb20vYXBwbGVjYS8wEwYDVR0lBAwwCgYIKwYBBQUH\r\nAwIwTQYDVR0fBEYwRDBCoECgPoY8aHR0cDovL2RldmVsb3Blci5hcHBsZS5jb20v\r\nY2VydGlmaWNhdGlvbmF1dGhvcml0eS93d2RyY2EuY3JsMB0GA1UdDgQWBBR6BPca\r\ngaRwijNzbL4lcYrJkU2r7TALBgNVHQ8EBAMCB4AwEAYKKoZIhvdjZAYDAQQCBQAw\r\nDQYJKoZIhvcNAQEFBQADggEBAFcIWIc1T0PtgeaMgjwQcTmfJGy8MUdIO/hdElo/\r\nOZF4ts4c2xkddanZ9IzOCj/HzmRJEs6WVZhNxySc3Cxo6KejsLbGLJmMoEh72xyQ\r\nwgnMBiapMnRhCfd68NMnTUClNHvGsg+NipnAN63r+HZSgPsCMXHsEMyZ+qQendRc\r\nDZH6m5FN1TqdAVtChdDPItzYJuQpyeKJpiiQGeCd6YjCELkWVxHcTU67CWmkuVqx\r\n9BRoANbJXty3b9T5KHxJYcEMj3pvsgcTOR/nGKIT9+B2iqrt6i/YY2n4p5NXXFzZ\r\nHewPc93srGXfyrvW7SeQs+93vZ7WlntfihY7WCoUbEOnHso=\r\n-----END CERTIFICATE-----"


     **pushKey**: "-----BEGIN RSA PRIVATE KEY-----\r\nMIIEpQIBAAKCAQEAyKPvBg+nIW8QhQyO20yuok8UB5w/xYlybxGAWzt3skFfP12Z\r\nuulWzvkWwOFAKwNm43VuSmZEkO4UmCXKJkApnJqapq143Gi9Cn3lop0s+Jm18aFy\r\nvSbKFEozn8u8lxxgy2Uhxlb5+YDtRVPVj19yLu2GDE9YcJzIJmSLs5+SvQIssb0M\r\n0pSJRLJ6aKkw1M/tXl7T1wCEj0E/PozPCaGaIU6zg7lFI9q0j1outpDAT7F+B+J3\r\nEoyOegxvxNJb1Fe3dDxIlJ6MRsM8nJhiVYKWar/xOqdXlDU+1LZeJLboqhL/H7d/\r\nm6ycfhaJDIuwCo8S75WEtfC0xuiW0DHJlx7HZQIDAQABAoIBAFNo+1xMs5FNp9N4\r\nBgebGFp3f38ucMCBRGZyIydKUJd1X9Bq7BbtHF6M5O2odtGq52IWFpStcUHDCCK8\r\nSw6dy+7DwxkZss4GaNhswENbDjAHTsE1+goyjv3iXxXGUA+OB5tm3qSi0ebstzcE\r\nBBtHdaOWsQx7C+w88WQslntFEm6qNSqeM1s5eQ20wSnlAA43Pm+NdVNM+JYX4iqt\r\n/MFdaPINE5XAXyBRAYir0l1dkofeGsb4rCuZmXSmjRRcC9vdhzmjrDUxLspOI7du\r\nUT7vYy3/hWFdng5oHu7JoDVrxF8/5e11j04jTq8SiHYfxUdR/Pmzt4/Nnu8SVjDJ\r\nesZJhwECgYEA7ElQyw2fzF3CQ2x526SBvLfFSxuX8zDkPjFtsrpZf78MHz9AR3ak\r\nlgEjR9aOfeWC1nRRZ6kGG8AgbpIZEN5KoCwZM98D6oub0VIg8iuR1UeArjebI8QV\r\nq2q5BeR3v2nmHsDXeG57D7O7DRko1tazp+d3/19hmMBOa4os19SsEaUCgYEA2WFH\r\nPtVwRtsQNcuKBCKHGzEVDG6Gm86kye2AdfJB7USr0fvD3HRah/chGi+foz+CDwwA\r\nzuBcgL45rSZZCTA9AFzihVpAFJ+a3GDgT/hSjJmMX3vHmySaZABmKNsiXRlPW+Fr\r\n7jkXwk7JH6emgcBH9d+Gyyp6ybVZvv/tNrFCcsECgYEA1DhtNlLASZ+UUVZmhF3W\r\noJc1vmXELgqllS5z5mj05YXD73Sx2P24iXnwJB+Sz4SJ5O+IBeCLufTvrB/QH5Rn\r\n1kCFSk9thwVpJ7HqIVf8nWChNNiAoLkG9XTfRWmUG/mTU9/EJ0ijgtDcmcEVKxCf\r\nP5jn8BfM4pMmW/Q4nolHGnkCgYEAnnLT1a8KSft/k1arYVwxktZx+z/NCmDTqQRf\r\nMJnHCEWX4FVdbKG7I4Q1Mrsn53xxNrqPFDxh8M23iMh8+b+Zl1wdGQqxztaPsLdE\r\nicX9ldKOiULWOfWyO9Y2oO0p3SaHu/dSDrC66r02yMYRDl6zlTq7K/fozIJNynUN\r\n2WHXh4ECgYEAneclaDj/KMtedyaJcblyEjtpzW3V/Gotws2cAaHqVaWDJHRrp0bl\r\nZNg8Jjf/NoixxENZ5bdduAR3JEjTwScRpOluO45Huq2gIMS7jsBKoDAvF94DNUI5\r\nNDtK8x/8+SOdk5HAGNHDoMqVbgZ2NpeOIGxzoZUCCK4f1j1CZbeEIio=\r\n-----END RSA PRIVATE KEY-----"


    **gcm**:


     **senderId**: 90077701463


     **apiKey**: AIzaSyAHNIwGE0yKG9QnDZQMcziNAF-0zliXOtH


    **pushScheduler**:


     **url**: redis://localhost:6379/7


     **replicaUrls**:


     - redis://localhost:6379/8


### 


### **# Extracting Push Certificate and Push Private Key**

The configuration file requires you to include the string contents of the certificate and private key  which can be extracted from the **.p12** file.

To extract the certificate, just run the comment below:

$ openssl pkcs12 -in example.p12 -out example.crt.pem -clcerts -nokeys -passin 'pass:XXXXXXXX'

The **example.p12** is the path to the .p12 file.

The **example.crt.pem** is the outfile filename.

Replace **XXXXXXXX** with the password of the file.

To extract the certificate, just run the comment below:

$ openssl pkcs12 -in example.p12 -nocerts -nodes -passin 'pass:XXXXXXXX'' | openssl rsa -out example.rsa.key.pem

The **example.p12** is the path to the .p12 file.

The **example.rsa.key.pem** is the outfile filename.

Replace **XXXXXXXX** with the password of the file.

On each file, the contents are separated by a new line character, since the configuration file only accepts a single line string for the pushCertificate and pushKey, you need to replace the new line character with **\r\n** just like in the sample configuration above. This is important to avoid errors.


### **# Running Migrations**

Before you start running the server, you need to run the following commands to create the necessary tables used by Signal.


    $ java -jar service/target/TextSecureServer-1.88.jar  messagedb migrate  config/Signal.yml


    $ java -jar servive/target/TextSecureServer-1.88.jar  accountdb migrate  config/Signal.yml

Note: Replace the **Signal.yml** with the correct filename of your configuration file.


### **# Starting the Server**

You can start the server by running the command below:


    $ java -jar target/TextSecureServer-1.88.jar  server config/Signal.yml

Note: Replace the **Signal.yml** with the correct filename of your configuration file.