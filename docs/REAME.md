# Start project

```shell

kubectl apply -f deployment/db-configmap.yaml
kubectl apply -f deployment/db-secret.yaml
kubectl apply -f deployment/kafka-configmap.yaml
kubectl apply -f deployment/postgres.yaml
kubectl apply -f deployment/kafka.yaml
kubectl apply -f deployment/person-rpc.yaml
kubectl apply -f deployment/udaconnect-api.yaml
kubectl apply -f deployment/udaconnect-connection.yaml
kubectl apply -f deployment/udaconnect-location.yaml
kubectl apply -f deployment/udaconnect-location-consumer.yaml
kubectl apply -f deployment/udaconnect-person.yaml
kubectl apply -f deployment/udaconnect-person-consumer.yaml

```

# get pods
![Alt text](pods_screenshot.png)

# get svc
![Alt text](services_screenshot.png)

# test
```shell

kubectl port-forward svc/location-api 5000:5000

```

```shell
curl -X POST \
  http://localhost:5000/api/locations \
  -H 'Content-Type: application/json' \
  -d '{
  "latitude": "-122.290524",
  "longitude": "37.553441",
  "creation_time": "2020-08-18T10:37:06",
  "person_id": 29
}'

```

# logs
```shell

# producer logs
kubectl logs location-api-7cd979d9d5-5sdt9
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
INFO:werkzeug: * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
INFO:root:kafka-service:9092
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:Probing node bootstrap-0 broker version
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.conn:Broker version identified as 2.5.0
INFO:kafka.conn:Set configuration api_version=(2, 5, 0) to skip auto check_version requests on startup
INFO:kafka.conn:<BrokerConnection node_id=1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:<BrokerConnection node_id=1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connected> [IPv4 ('10.43.3.214', 9092)]>: Closing connection. 
INFO:kafka.conn:<BrokerConnection node_id=1 host=kafka-service:9092 <connected> [IPv4 ('10.43.3.214', 9092)]>: Closing connection. 
INFO:werkzeug:127.0.0.1 - - [21/Dec/2023 01:37:12] "POST /api/locations HTTP/1.1" 200 -

# consumer logs
kubectl logs location-consumer-57d6d6f7b9-wxsgc
INFO:root:postgresql://ct_admin:wowimsosecure@postgres:5432/geoconnections
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:Probing node bootstrap-0 broker version
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.conn:Broker version identified as 2.5.0
INFO:kafka.conn:Set configuration api_version=(2, 5, 0) to skip auto check_version requests on startup
INFO:kafka.consumer.subscription_state:Updating subscribed topics to: ('location-topic',)
INFO:kafka.cluster:Group coordinator for location-consumer-group is BrokerMetadata(nodeId='coordinator-1', host='kafka-service', port=9092, rack=None)
INFO:kafka.coordinator:Discovered coordinator coordinator-1 for group location-consumer-group
INFO:kafka.coordinator:Starting new heartbeat thread
INFO:kafka.coordinator.consumer:Revoking previously assigned partitions set() for group location-consumer-group
INFO:kafka.conn:<BrokerConnection node_id=coordinator-1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:<BrokerConnection node_id=coordinator-1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connected> [IPv4 ('10.43.3.214', 9092)]>: Closing connection. 
INFO:kafka.coordinator:(Re-)joining group location-consumer-group
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.coordinator:Elected group leader -- performing partition assignments using range
INFO:kafka.coordinator:Successfully joined group location-consumer-group with generation 1
INFO:kafka.consumer.subscription_state:Updated partition assignment: [TopicPartition(topic='location-topic', partition=0)]
INFO:kafka.coordinator.consumer:Setting newly assigned partitions {TopicPartition(topic='location-topic', partition=0)} for group location-consumer-group
INFO:kafka.conn:<BrokerConnection node_id=1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: connecting to kafka-service:9092 [('10.43.3.214', 9092) IPv4]
INFO:kafka.conn:<BrokerConnection node_id=1 host=kafka-service:9092 <connecting> [IPv4 ('10.43.3.214', 9092)]>: Connection complete.
INFO:kafka.conn:<BrokerConnection node_id=bootstrap-0 host=kafka-service:9092 <connected> [IPv4 ('10.43.3.214', 9092)]>: Closing connection. 
INFO:location-consumer:Location created: {'person_id': 29, 'latitude': '-122.290524', 'longitude': '37.553441', 'creation_time': '2020-08-18T10:37:06'}


```
