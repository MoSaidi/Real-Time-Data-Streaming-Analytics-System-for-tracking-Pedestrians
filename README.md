# Real Time Data Streaming Analytics System for tracking pedestrians

## The Project GOAL 

![Screenshot (488)](https://user-images.githubusercontent.com/74468388/146798688-c5035625-2ee2-4e0b-bece-72cf5d27cdd7.png)

## The Main Project Functional Architecture 

![Screenshot (487)](https://user-images.githubusercontent.com/74468388/146799004-0b42f1e7-60c3-4b65-a16c-37e7d765b2b0.png)

## The Technical Project Architecture


![I-track - Technical architecture](https://user-images.githubusercontent.com/74468388/149933820-f1df134b-f7d7-473b-98a1-917e1c0d247c.png)

#### Project technical architecture Description 
<p>
First of all, we chosed traccar server as data source generator. We installed on our users' phones the traccar client application to establish a connection with traccar server, the data generated from the application will be sent to traccar server and mongoDB will act as a database for traccar server. the connection between these two components was established with the help of a websocket API using a python program for this purpose. 
</p>
  
<p>
After generating the data, two main phases are coming in the picture, batch processing and stream processing. For the batch phase we used S3 as a cloud simple storage datalake service for advanced analytics and ML algorithms purpose using a python program that stores the data generated from traccar Server as excel file every 24 hours and store it in S3, then we connected S3 with the sageMaker service to manipulate this for and build an adequate models.
</p>  
  
 <p>
Let's move now the stream processing phase, Kafka was used as a data ingestion tool between mongoDB and spark streaming, so here mongoDB were acting as a producer and spark as consumer that subscribes to our kafka topic. spark streaming holds the data and handle it so to be convenient for our needs.
</p>   

 <p>
Another important phase was important, which is the monitoring phase. so to take advantage about the process of assessing the performance of our cluster entities either as individual nodes or as a collection. for our case, the prometheus solution has come to the picture to handle our cluster status. Prometheus collects and stores its metrics as time series data in one place, this advantage will make us easily supervise our cluster, and based on these metrics we can perform a real-time monitoring of how our cluster is responding over time with the help of grafana that integrate perfectly with prometheus and provide a user friendly and human readable dashboard.   
</p> 

 <p>
The last phase was the real time visualisation of our data flow. we chose to work with ELK stack in this phase, ELK stands for Elasticsearch, logstash and kibana. Logstash played the middle man role between the Kafka handled data topic and elasticsearch, the connection between these two components is easy, a pipeline.conf script should be written, the script will build the pipeline between kafka and elasticsearch and once elasticsearch starts consuming the last preprocessed data kibana will take the data and present it the best way possible using its visualisation capabilities.  
</p> 

 <p>
The last phase was the real time visualisation of our data flow. we chose to work with ELK stack in this phase, ELK stands for Elasticsearch, logstash and kibana. Logstash played the middle man role between the Kafka handled data topic and elasticsearch, the connection between these two components is easy, a pipeline.conf script should be written, the script will build the pipeline between kafka and elasticsearch and once elasticsearch starts consuming the last preprocessed data kibana will take the data and present it the best way possible using its visualisation capabilities.  
</p> 

<p>
In the end, and to establish the connection between all these components, we used docker compose to build a containerized architecture by defining and running multi-container docker applications. With Compose, we used a YAML file to configure our platform’s services. Then, with a single command, we can create and start all the services from the configuration file. 
  </p>

### Detail Summary about the architecture

| Container | Image | Tag | Accessible | 
|-|-|-|-|
| zookeeper | zookeeper | latest | 172.25.0.10:2181 |
| kafka1 | wurstmeister/kafka | 2.12-2.2.0 | 172.25.0.11:9092  |
| kafka_manager | hlebalbau/kafka_manager | stable | 172.25.0.12:9000 |
| prometheus | prom/prometheus | v2.8.1 | 172.25.0.13:9090 |
| grafana | grafana/grafana | 6.1.1 | 172.25.0.14:3000 |
| spark-master | bde2020/spark-worker | 3.2.0-hadoop3.2 | 172.25.0.15:8085 |
| spark-worker-1| bde2020/spark-worker | 3.2.0-hadoop3.2 | 172.25.0.16:8081 |
| elasticsearch| docker.elastic.co/elasticsearch/elasticsearch:7.4.1  | 7.4.1 | 172.25.0.17:9200 |
| kibana| docker.elastic.co/kibana/kibana:7.4.0 | 7.4.0 | 172.25.0.18:5601 |


## Data source

- **The application name :** traccar client
- **The application url      :** https://play.google.com/store/apps/details?id=org.traccar.client

Description :  
![Capture](https://user-images.githubusercontent.com/74468388/142759290-c207da1e-ef2b-44a4-99b5-978956642716.PNG)

Traccar Client is an app that allows you to use your mobile device as a GPS tracker. It reports location to your own or hosted server with selected time intervals.

By default application is configured to use free Traccar service (address - demo.traccar.org, port - 5055). To see your device on map register on http://demo.traccar.org/ and add your device with identifier.

Traccar (Server) is a free open source server that supports more than 100 different protocols and GPS tracking devices. You can use this application with your own hosted instance of Traccar. For more information visit https://www.traccar.org/.

## Traccar Server 

We installed traccar server on a digitalocean VPC, it will cost us 5$ for one month from 100$ promo credit ( i used github student offer ) 

So the traccar is accessible publicly for now you and anyone can access it using this link : 

http://167.99.215.40:8082/

The traccar server is using h2 in memory database by default and we have to configure our custom mongoDB database as a next step. 

![Screenshot (481)](https://user-images.githubusercontent.com/74468388/146070938-1a731c94-5de5-4a75-bd9c-5a4c4745b9b4.png)

## Traccar and mongoDB nosql database connection

We used python sdk to connect the traccar server and the mongodb using the websocket.

## The Technical Project Architecture
