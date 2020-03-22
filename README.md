# HDInsight Managed Kafka with Confluent Kafka Schema Registry
Kafka Schema Registry provides serializers that plug into Kafka clients that handle  message schema storage and retrieval for Kafka messages that are sent in the Avro format. Its used to be a  OSS project by Confluent , but is now under the [Confluent community license](https://www.confluent.io/blog/license-changes-confluent-platform/) . The Schema Registry can additionally serves the below purposes
 
 - Store and retrieve schemas for producers and consumers
 - Enforce backward/forward /full compatibility on Topics
 - Decrease the size of the payload sent to Kafka  

In an HDInsight Managed Kafka cluster the Schema Registry is typically deployed on an Edge node to allow compute separation from Head Nodes. 

Below is a representative architecture of how the Schema Registry is deployed on an HDInsight cluster. Note that Schema Registry natively exposes a REST API for operations on it.  Producers and consumers can interact with the Schema Registry from within the VNet or when using the Kafka REST Proxy. 

![Create Azure Resource Group](https://github.com/arnabganguly/Kafkaschemaregistry/blob/master/images/Pic1.png)

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json
)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNDYyOTA1NTIsLTE4NTU1ODE0NjMsMT
YzNTcxMzc1NSwtOTcwNjA5MTk1LDIwMjMyOTgwNzMsLTQ0MDU4
Mzk2NywtMTI2Njc3MDUyNSwxNDkxNTM2NjEsNjU1ODMxOTQ5LD
g1MjMwMTQ1NSwyNzA1Mzk2NjldfQ==
-->