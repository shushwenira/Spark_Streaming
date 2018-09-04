Steps to run the Application (Windows platform):
------------------------------------------------------------------------------
- Go to the src/main/resources folder. Add your Twitter credentials to
   the application.properties file.

- Import the project into IntelliJ IDEA as a Maven project.

- Click on View -> Tool Windows -> Maven Projects. 

- On the new window on right side, click on Lifecycle -> package. 
	Jar file will be created in the target folder.

- Run following command to start capturing tweet sentiment scores for "monday"
	spark-submit --class streaming.TwitterProducer /twitter-1.0-jar-with-dependencies.jar twitter "monday"

- Start Zookeeper server
	zookeeper-server-start.bat ../../etc/kafka/zookeeper.properties &

- Start Kafka server
	kafka-server-start.bat ../../etc/kafka/server.properties &

- Create Kafka topic
	kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic twitter

- Create Kafka producer 
	kafka-console-producer.bat --broker-list localhost:9092 --topic twitter

- Create Kafka consumer
	kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic twitter --from-beginning

- Start Elastic search using "elasticsearch.exe" file inside the bin folder

- Start Kibana using following command at kibana/bin folder
	kibana

- Create a file logstash-simple.conf inside logstash directory with following content:
	input {
	 kafka {
	   bootstrap_servers => "localhost:9092"
	   topics => ["twitter"]
	 }
	}
	
	output {
	 elasticsearch {
	   hosts => ["localhost:9200"]
	   index => "twitter-index"
	  }
	}

- Run the following command to start Logstash
	bin/logstash -f logstash-simple.conf

- Open following link in browser to access Kibana
	http://localhost:5601

- Create following index pattern
	twitter-index

- Visualize the tweets' sentiment scores using Kibana dashboards.

	
