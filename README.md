# kafka_docker_setup
Refer https://www.confluent.io/blog/kafka-listeners-explained/ for more detailed inforamtion

Basic setup and testing of kafka setup using Docker


1) Start docker
	docker-compose up -d

	Create dirs in case of errors
	./kafka_data
	./zookeeper_data
	./zookeeper_data/zk-data
	./zookeeper_data/zk-txn-logs


2) kafkacat to test basic kafka connections

	Listener BOB (port 29092) for internal traffic on the Docker network
  		docker-compose exec kafkacat kafkacat -b kafka0:29092  -L

	Listener FRED (port 9092) for traffic from the Docker-host machine (localhost)
		docker-compose exec kafkacat  kafkacat -b kafka0:9092 -L

	Listener ALICE (port 29094) for traffic from outside, reaching the Docker host on the DNS name never-gonna-give-you-up
		docker run -t --network kafka-listeners_default confluentinc/cp-kafka kafkacat -b kafka0:29094 -L



3) Connect to kafka container
	docker exec -it kafka_docker_setup_kafka0_1 bash

4) Create topic
	kafka-topics --create --topic testvg --bootstrap-server kafka0:29092 --partitions 1 --replication-factor 1

5) List topics
	kafka-topics --list --bootstrap-server kafka0:29092

6) Watch live log on consumer
	kafka-console-consumer  --bootstrap-server kafka0:29092 --topic testvg --from-beginning

7) Post data using produce data usning console-producer
	kafka-console-producer --broker-list  kafka0:29092 --topic testvg


