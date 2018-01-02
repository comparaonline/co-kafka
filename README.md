# Simple Confluent Kafka with docker-compose

This is an example app of how setup and use kafka.

The docker-compose provide access to the following services:

* Zookeeper: port 2181
* Kafka: port 9092

## Configuration

To access kafka broker from other docker containers it is required to create a common network:

```
docker network create confluent_kafka
```

Each consumer/produce container must join this network. It can be set in docker-compose.yml with:


```
services:
  <service_name>:
    ...
    networks:
      - default
      - confluent_kafka
    ...

networks:
  default: {}
  confluent_kafka:
    external: true
```

## Start Docker

```
docker-compose up -d
```

Access to the broker container to execute kafka cli commands
```
docker-compose exec kafka bash
```

## Kafka CLI Examples

### Topics
Create a new topic
```
kafka-topics --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic collections
```

List all topics
```
kafka-topics --list --zookeeper zookeeper:2181
```

Alter partition number in topic
```
kafka-topics --alter --partitions 2 --topic collections --zookeeper zookeeper:2181
```

### Consumer
Subscribe to a topic trough the terminal, all events will be printed
```
kafka-console-consumer --bootstrap-server kafka:9092 --topic collections
```

### Producer
Produce to a topic trough the terminal
```
kafka-console-producer --broker-list kafka:9092 --topic collections
```
then in the terminal paste the JSON event:
```
> {"code": "StartChargeFeesEvent"}
```
