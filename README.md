# HDInsight Managed Kafka with Confluent Kafka Schema Registry
Kafka Schema Registry provides serializers that plug into Kafka clients that handle  message schema storage and retrieval for Kafka messages that are sent in the Avro format. Its is an OSS project by Confluent under the [Confluent community license](https://www.confluent.io/blog/license-changes-confluent-platform/) and has been around for many years. The Schema Registry can additionally serves the below purposes
 
 - Store and retrieve schemas for producers and consumers
 - Enforce backward/forward /full compatibility on Topics
 - Decrease the size of the payload sent to Kafka  

In an HDInsight Kafka cluster the schema Registry is typically deployed on an Edge node. 

Below is a representative architecture of how the Schema Registry is deployed on an HDInsight cluster. Note that Schema Registry natively exposes a REST API for operations on it.  Producers and consumers can interact with the Schema Registry from within the VNet or when using the Kafka REST Proxy. 

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json
)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI1Njg3NDM0OCwxNjM1NzEzNzU1LC05Nz
A2MDkxOTUsMjAyMzI5ODA3MywtNDQwNTgzOTY3LC0xMjY2Nzcw
NTI1LDE0OTE1MzY2MSw2NTU4MzE5NDksODUyMzAxNDU1LDI3MD
UzOTY2OV19
-->