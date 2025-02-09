Create topics:
docker exec -it kafka_kraft kafka-topics --create   --topic my-secure-topic --bootstrap-server kafka:9092   --command-config config/client.properties   --partitions 1 --replication-factor 1

List topics:
docker exec -it kafka_kraft   kafka-topics --bootstrap-server kafka:9092   --list   --command-config /etc/kafka/client.properties

ACL for sriter:
docker exec -it kafka_kraft kafka-acls --bootstrap-server kafka:9092 --add --allow-principal User:writer   --operation Write   --topic my-sec
ure-topic --command-config /etc/kafka/writer-client.properties