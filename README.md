# Kafkaschemaregistry
Kafka Schema Registry is an mechanism to store and retrieve Avro schemas. Its is an OSS project by Confluent under the confluent community license and has been around for many years. The Schema Registry serves the below purposes
 
 - Store and retrieve schemas for producers and consumers
 - Enforce backward/forward /full compatibility on Topics
 - Decrease the size of the payload sent to Kafka  

In an HDInsight Kafka cluster the schema Registry is typically deployed on an Edge node. 

Lab depicting how one can set up the Confluent Schema Registry on HDInsight Kafka 

[e ![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json
)


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyMzI5ODA3MywtNDQwNTgzOTY3LC0xMj
Y2NzcwNTI1LDE0OTE1MzY2MSw2NTU4MzE5NDksODUyMzAxNDU1
LDI3MDUzOTY2OV19
-->