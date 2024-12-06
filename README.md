# kafka-partition-count-analyzer
This document helps you determine the number of partitions (replicas) per broker in your Confluent Platform cluster.

## Usage

1. Describe all topics in your Kafka cluster:

   ```bash
   /bin/kafka-topics --bootstrap-server <your-bootstrap-server>:9092 --describe > prod_topics.txt
   ```

Replace <your-bootstrap-server> with your Kafka bootstrap server address.

2. Analyze the partition distribution:
   ```bash
   cat prod_topics.txt | grep "Leader:" | awk '{print $8}' | tr "," " " | tr "\n" " " | tr " " "\n" | sort | uniq -c
   ```

This command outputs two columns:
* First column: Number of replicas
* Second column: Broker number

The sum total of the number of replicas across each broker in (2) should match the number of Replicas (highlighted region) in the screen shot of the Confluent Control Center below

![Screenshot 2024-12-06 at 3 31 56 PM](https://github.com/user-attachments/assets/70e2bd78-8ebe-46d9-8577-94bd7962bacb)
