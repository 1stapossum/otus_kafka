Create topics:
docker exec -it kafka-kraft kafka-topics --create   --topic my-secure-topic --bootstrap-server localhost:9092   --command-config config/client.properties   --partitions 1 --replication-factor 1
List topics:
docker exec -it kafka_kraft \
  kafka-topics --bootstrap-server kafka:9092 \
  --list \
  --command-config /etc/kafka/client.properties
test-topic