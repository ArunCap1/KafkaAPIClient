
Compile the code using 

mvn compile assembly:single

Process What i have followed : 
Initially i have started zookeeper and kafka servers which are part of docker compose files with below images  :


  # Kafka : 
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"

  kafka:
    image: wurstmeister/kafka:2.11-1.1.1
    ports:
      - "9092:9092"
    environment:
      - KAFKA_ADVERTISED_HOST_NAME=localhost
      - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_CREATE_TOPICS=etl_load:1:1,dm:1:1,Sample_TestTopic:2:1
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      
      
If you could observe, Sample_TestTopic:2:1 is the one i have created the topic in application which will be using for  transmitting the data from produser to consumer 
Sample_TestTopic:2:1: here we have 2 partitions and one replica (that means there are no duplicate data for it)
if 2:2 means there will be two partitions and 2 duplicate records for it.
https://www.youtube.com/watch?v=r4c0whAOCGI will tell you everything .. 

sometimes what happens : you will miss partition names in the console and we are not sure what is happening in the backend system .. 
--> for that what i have done is,  initially i have checked removing partitions and then executed entire application. it was working with out any issues(make sure topic name in producer and consumer  .. thats it .. you dnt have to worry about other things)
--> And i kept partitions back in the code .. i have faced some issues which are related to broker and replica issues.
if you would like to maintain more than one replica, you should have more than one broker in the back end side .. so I followed 2:1 since i dnt have more than one broker .. please make sure to go through online and find how to start more than one broker in it.. 

I have cretaed the topic name is : Sample_TestTopic
And partitions are created by kafka machine : Sample_TestTopic-0,Sample_TestTopic-1













