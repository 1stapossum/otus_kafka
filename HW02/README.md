1. Был создан docker compose файл. Который запускает контейнерезированый инстанс Kafka (1 нода) в режиме kraft с SASL.
Скриншот№1.

2. Создан топик my-secure-topic. 
Скриншот№2.

3. Добавлены ACL для writer , reader и norights.
Скриншот№3 , Скриншот№4 и Скриншот№6
итоговый список ACL - скриншот№7

4. Отправлены сообщения в топик my-secure-topic. От пользователя writer, reader и norights.
Сообщени от пользователя writer было успешно отправлено в топик my-secure-topic Скриншот№8 . Сообщени от пользователя reader не было отправлено в топик my-secure-topic. Скриншот№9 . Сообщени от пользователя norights не было отправлено в топик my-secure. Cкриншот№10.

5. Получены сообщения из топика my-secure-topic.
Для пользователя writer сообщения получены НЕ БЫЛИ. Скриншот№11.
Для пользователя reader сообщения получены БЫЛИ. Скриншот№12.
Для пользователя norights сообщения получены НЕ БЫЛИ. Скриншот№13

6. Получен список топиков для пользователей writer, reader и norights.
Для пользователя writer список топиков получен. Скриншот№14.
Для пользователя reader список топиков получен. Скриншот№15.
Для пользователя norights список топиков получен. Скриншот№16. нашего топика в списке нет.







 



Используемые команды:
Create topics:
docker exec -it kafka_kraft kafka-topics --create   --topic my-secure-topic --bootstrap-server kafka:9092   --command-config config/client.properties   --partitions 1 --replication-factor 1

List topics:
docker exec -it kafka_kraft   kafka-topics --bootstrap-server kafka:9092   --list   --command-config /etc/kafka/client.properties

ACL for writer:
docker exec -it kafka_kraft kafka-acls --bootstrap-server kafka:9092 --add --allow-principal User:writer   --operation Write   --topic my-secure-topic --command-config /etc/kafka/writer-client.properties

ACL for reader:
docker exec -it kafka_kraft kafka-acls --bootstrap-server kafka:9092   --add   --allow-principal User:reader   --operation Read --topic my-secure-topic   --command-config /etc/kafka/reader-client.properties
ACL for norights:
docker exec -it kafka_kraft kafka-acls --add --deny-principal User:norights --operation All --topic my-secure-topic --bootstrap-server kafka:9092 --command-config /etc/kafka/admin-client.properties


Remove all ACLs:
docker exec -it kafka_kraft kafka-acls --bootstrap-server kafka:9092   --remove   --topic my-secure-topic   --command-config /etc/kafka/admin-client.properties


Produce from writers client:
docker exec -it kafka_kraft kafka-console-producer --bootstrap-server kafka:9092 --topic my-secure-topic --producer.config /etc/kafka/writer-client.properties

Produce from readers client:
docker exec -it kafka_kraft kafka-console-producer --bootstrap-server kafka:9092 --topic my-secure-topic --producer.config /etc/kafka/reader-client.properties

Produce from norights client:
docker exec -it kafka_kraft kafka-console-producer   --bootstrap-server kafka:9092   --topic my-secure-topic   --producer.config /etc/kafka/norights-client.properties

Consume from writers client:
docker exec -it kafka_kraft kafka-console-consumer   --bootstrap-server kafka:9092   --topic my-secure-topic   --consumer.config /etc/kafka/writer-client.properties   --from-beginning

Consume from readers client:
docker exec -it kafka_kraft kafka-console-consumer   --bootstrap-server kafka:9092   --topic my-secure-topic   --consumer.config /etc/kafka/reader-client.properties   --from-beginning

Consume from norights client:
docker exec -it kafka_kraft kafka-console-consumer   --bootstrap-server kafka:9092   --topic my-secure-topic   --consumer.config /etc/kafka/norights-client.properties   --from-beginning

Get topics by writer client:
docker exec -it kafka_kraft   kafka-topics --bootstrap-server kafka:9092   --list   --command-config /etc/kafka/writer-client.properties

Get topics by readers client:
docker exec -it kafka_kraft   kafka-topics --bootstrap-server kafka:9092   --list   --command-config /etc/kafka/reader-client.properties

Get topics by norights client:
docker exec -it kafka_kraft   kafka-topics --bootstrap-server kafka:9092   --list   --command-config /etc/kafka/norights-client.properties