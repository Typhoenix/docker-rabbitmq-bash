## Simple Publish and Subscribe Solution with Docker, RabbitMQ, and Bash

I recently developed a simple publish and subscribe solution using Docker, RabbitMQ, and Bash scripting. In this article, I'll explain the solution and walk you through the steps to set it up.

### Prerequisites

Before getting started, make sure you have Docker installed on your machine. Docker allows us to run applications in isolated containers, making it convenient to deploy and manage services like RabbitMQ.

### Solution Overview

The solution involves using RabbitMQ as the message broker and Bash scripting to interact with RabbitMQ and perform the publish and subscribe operations. Here's an overview of the process:

1. Define the RabbitMQ broker details such as the host, port, username, and password.
2. Specify the topic to which you want to subscribe and publish messages.
3. Prepare the message you want to publish.
4. Subscribe to the specified topic by creating a queue and declaring a binding between the topic exchange (`amq.topic`) and the queue.
5. Publish the message by using the RabbitMQ management plugin and the `rabbitmqadmin` command-line tool.
6. Retrieve the received messages from the queue.

### Implementation

To implement the solution, follow these steps:

1. Create a new bash script file, for example, `subscribe_publish.sh`, and open it in a text editor.
2. Copy the following script into the file:

```bash
#!/bin/bash

# RabbitMQ broker details
HOST="localhost"
PORT="15672"
USER="guest"
PASS="guest"

# Topic to subscribe and publish
TOPIC="mytopic"

# Message to publish
MESSAGE="Hello, Phoenix!"

# Subscribe to the topic
echo "Subscribing to topic $TOPIC..."
docker exec -it rabbitmq rabbitmqadmin --username=$USER --password=$PASS --host=$HOST --port=$PORT declare queue name=$TOPIC
docker exec -it rabbitmq rabbitmqadmin --username=$USER --password=$PASS --host=$HOST --port=$PORT declare binding source=amq.topic destination_type=queue destination=$TOPIC routing_key=$TOPIC

# Publish a message
echo "Publishing message: $MESSAGE"
docker exec -it rabbitmq rabbitmqadmin --username=$USER --password=$PASS --host=$HOST --port=$PORT publish exchange=amq.topic routing_key=$TOPIC payload="$MESSAGE"

# Display received messages
echo "Received messages:"
docker exec -it rabbitmq rabbitmqadmin --username=$USER --password=$PASS --host=$HOST --port=$PORT get queue=$TOPIC
```

3. Save the file and close the text editor.

### Running the Solution

To run the solution, follow these steps:

1. Open a terminal or command prompt.
2. Navigate to the directory where you saved the `subscribe_publish.sh` script.
3. Make the script executable by running the command: `chmod +x subscribe_publish.sh`.
4. Execute the script by running the command: `./subscribe_publish.sh`.

The script will perform the following actions:

1. Subscribe to the specified topic by creating a queue and declaring a binding between the topic exchange (`amq.topic`) and the queue.
2. Publish the specified message to the topic using the RabbitMQ management plugin and the `rabbitmqadmin` command-line tool.
3. Retrieve and display the received messages from the queue.

### Conclusion

In this article, we explored a simple publish and subscribe solution using Docker, RabbitMQ, and Bash scripting. By following the steps outlined here, you can quickly set up and run this solution, enabling you to publish and subscribe to messages using RabbitMQ.

This solution can serve as a starting point for more advanced message exchange scenarios and can be extended to meet specific application
