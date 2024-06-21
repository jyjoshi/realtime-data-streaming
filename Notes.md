#

What is DAG?

What is AirFlow?

What is Zookeeper?

What is Confluence Systems?

Docker compose helps with spinning up and spinning down at anytime

# Setup AirFlow with PostGreSQL

# What does docker compose up do?

'''docker compose up''' command is used to start up and run a multi-container Docker application defined in a docker-compose.yaml file.

Here's the breakdown of what it does:

1. Reads the docker-compose.yaml file: The command reads the configuration defined in the 'docker-compose.yaml' file. This file specifies the services (containers) that make up the application, their respective configurations, networks, volumes, and dependencies.

2. Creates and starts containers: It builds the necessary Docker images (if they haevn't been built yet) and starts the container defined in the 'docker-compose.yaml' file. If the images are already available, it will use them directly.

3. Attaches to container output: By default, 'docker compose up' runs in the foreground and attaches to the logs of the containers, displaying their output in your terminal. This helps in monitoring the startup process and viewing logs in real-time.

4. Creates networks and volumes: If specified in the 'docker-compose.yaml' file, it will create the required networks and volumes. These are essential for inter-container communication and data persistence respectively.

5. Handles dependencies: It ensures that containers are started in the correct order, respecting the dependency relationships specified in the 'depends_on' section of the 'docker-compose.yaml' file.

# Architecture

We are going to have a DAG in Apache Airflow which is fetching data from the random user api intermittently.
The data fetch is going to be streamed into Kafka q
Apache Airflow with out config is going to be running on Postgresql.

The data that we get from this api is going to be streamed into Kafka which is sitting on Apache Zookeper.
Zookeper is the manager, that manages the multiple brokers that we have on Kafka.

Data inside the Kafka brokers will be visualised in the control center. It servers as the ui where we can see what is going on in the kafka brokers.

Schema Registry is restful interface for storing and retrieving schema which is particularly useful when the kafka stream is being visualised so we can understand the schema of records of the data that is coming into kafka.

So the data that is outputted from kafka will be streamed with Apache Spark.
We have a master-worker architecture setup on Apache Spark.
When a job is submitted to the master. The master decides which of the worker takes up the work and runs the task.
Once task is run, the task in this case will be to stream data into cassandra.
So we have a listener that is going to be getting data from kafka into spark then streamed directly into cassandra. Everything run on Docker container with a single docker compose file that helps us to spin up the entire architecture.
