# Kafkaschemaregistry
Kafka Schema Registry provides serializers that plug into Kafka clients that handle  message schema storage and retrieval for Kafka messages that are sent in the Avro format. Its is an OSS project by Confluent under the confluent community license and has been around for many years. The Schema Registry can additionally serves the below purposes
 
 - Store and retrieve schemas for producers and consumers
 - Enforce backward/forward /full compatibility on Topics
 - Decrease the size of the payload sent to Kafka  

In an HDInsight Kafka cluster the schema Registry is typically deployed on an Edge node. 



[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json
)


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIxMzg1OTczOCwyMDIzMjk4MDczLC00ND
A1ODM5NjcsLTEyNjY3NzA1MjUsMTQ5MTUzNjYxLDY1NTgzMTk0
OSw4NTIzMDE0NTUsMjcwNTM5NjY5XX0=
-->