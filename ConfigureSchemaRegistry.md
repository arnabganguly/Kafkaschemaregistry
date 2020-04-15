## Configure the Confluent Schema Registry

The confluent schema registry is located at  ``` /etc/schema-registry/schema-registry.properties ``` and the mechanisms to start and stop service executables are located at the  ```/usr/bin/``` folder. 

The Schema Register needs to know the Zookeeper service to be able to interact with HDInsight Kafka cluster. Follow the below steps to get the details of the Zookeeper Quorum.

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
- Validate the content of the ```KAFKAZKHOSTS``` variable
```
echo  $KAFKAZKHOSTS
```
- Zookeeper values appear in the below format . Make a note of these values as they will be used later
```
zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
```
- To extract  Kafka Broker information into the variable KAFKABROKERS use the below command.

```
export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
```

Check to see if the Kafka Broker  information is available
```
echo $KAFKABROKERS
```
- Kafka Broker host information appears in the below format
```
wn1-kafka.eahjefyeyyeyeyygqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eaeyhdseyy1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092
```



- Open the Schema Registry properties files in edit mode

``` 
sudo vi /etc/schema-registry/schema-registry.properties
```
- By default the file would contain the below parameters 
```
listeners=http://0.0.0.0:8081
kafkastore.connection.url=zk0-ohkl-h:2181,zk1-ohkl-h:2181,zk2-ohkl-h:2181
kafkastore.topic=_schemas
debug=false
```
- Replace the kafastore.connection.url variable with the Zookeeper string that you noted earlier.  Also replace the debug variable to true . If set to true true, API requests that fail will include extra debugging information, including stack traces. The properties files now looks like this.  

```
listeners=http://0.0.0.0:8081
kafkastore.connection.url=zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
kafkastore.topic=_schemas
debug=true
```

- Save and exit the properties file using ``` :wq``` command

- Use the below commands to **Start the Schema Registry** and point it to use the updated Schema Registry properties file
```
cd /bin
``` 

 ```
 $ sudo schema-registry-start /etc/schema-registry/schema-registry.properties
 ```


- Schema Registry Starts and starts listening for requests. 
```
...
...
[2020-03-22 13:24:49,089] INFO Adding listener: http://0.0.0.0:8081 (io.confluent.rest.Application:190)
[2020-03-22 13:24:49,154] INFO jetty-9.2.24.v20180105 (org.eclipse.jetty.server.Server:327)
[2020-03-22 13:24:49,753] INFO HV000001: Hibernate Validator 5.1.3.Final (org.hibernate.validator.internal.util.Version:27)
[2020-03-22 13:24:49,902] INFO Started o.e.j.s.ServletContextHandler@40844aab{/,null,AVAILABLE} (org.eclipse.jetty.server.handler.ContextHandler:744)
[2020-03-22 13:24:49,914] INFO Started NetworkTrafficServerConnector@33fe57a9{HTTP/1.1}{0.0.0.0:8081} (org.eclipse.jetty.server.NetworkTrafficServerConnector:266)
[2020-03-22 13:24:49,915] INFO Started @2780ms (org.eclipse.jetty.server.Server:379)
[2020-03-22 13:24:49,915] INFO Server started, listening for requests... (io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain:45)
```

- With the Schema Registry running in one SSH session , launch another SSH window and try out some basic commands to ensure that Schema Registry is working as expected.


 - Register a new version of a schema under the subject "Kafka-key" and note the output 
```
$ curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
    --data '{"schema": "{\"type\": \"string\"}"}'
```
```
   HTTP/1.1 200 OK
Date: Sun, 22 Mar 2020 16:33:04 GMT
Content-Type: application/vnd.schemaregistry.v1+json
Content-Length: 9
Server: Jetty(9.2.24.v20180105)
```
      
 - Register a new version of a schema under the subject "Kafka-value" and note the output

```
curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
    --data '{"schema": "{\"type\": \"string\"}"}' \
```
```
HTTP/1.1 200 OK
Date: Sun, 22 Mar 2020 16:34:18 GMT
Content-Type: application/vnd.schemaregistry.v1+json
Content-Length: 9
Server: Jetty(9.2.24.v20180105)
```
- List all subjects and check the output 
```
curl -X GET -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
    http://localhost:8081/subjects
```
```
HTTP/1.1 200 OK
Date: Sun, 22 Mar 2020 16:34:39 GMT
Content-Type: application/vnd.schemaregistry.v1+json
Content-Length: 27
Server: Jetty(9.2.24.v20180105)

["Kafka-value","Kafka-key"]
```
- You may want to try some other [advanced commands listed here](https://docs.confluent.io/1.0/schema-registry/docs/intro.html#quickstart).

- In the next section we will read data from standard input and write it to a Kafka Topic  topic in an  format. We will then read the data from the Kafka Topics using a consumer with an Avro formatter to transform that data into readable format. 

Click [Next](https://github.com/arnabganguly/Kafkaschemaregistry/blob/master/UseAvroSchema.md) 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NzEwOTE3MTksLTEyODU3NzI5NjgsLT
E4MTA5OTU1ODEsLTE5OTU3NzI2MzYsLTE1MDM2MDI0MSw0NjM1
NDIzNTgsMTQ0NDE0Nzc5NywxNzI5Nzc3Mzg3LC0xNzMwMTQ5OD
Q1LC0xNjk2MzExNjY3XX0=
-->