# Weather Producer (QuixStreams → Kafka / Redpanda)

This project runs a **Python streaming producer** that fetches real-time weather data from the **Open-Meteo API** and publishes it to a **Kafka-compatible broker (Redpanda)** every 5 minutes.

---

## What This Project Does

- Fetches current weather data from **Open-Meteo**
- Sends the data to a Kafka topic using **QuixStreams**
- Runs continuously (every 5 minutes)
- Works with **Redpanda (Kafka-compatible)**

### Data Sent
- **Location:** London  
- **Latitude:** `51.5`  
- **Longitude:** `-0.11`  
- **Metric:** `temperature_2m`  
- **Topic:** `weather_data_demo`  
- **Key:** `London`  
- **Value:** Full JSON response  

---

## Tech Stack

- Python 3.12+
- QuixStreams
- Requests
- Redpanda (Kafka-compatible broker)
- Docker

---

## Step 1: Start Redpanda (Kafka Broker)

Run the following command:

```bash
docker run -d \
  --name redpanda \
  -p 9092:9092 \
  -p 9644:9644 \
  redpandadata/redpanda:latest \
  redpanda start \
  --overprovisioned \
  --smp 1 \
  --memory 1G \
  --reserve-memory 0M \
  --node-id 0 \
  --check=false
```

##  Step 2: Confirm Redpanda Is Running
```bash
docker ps
```

You should see:

redpanda

##  Step 3: Create the Kafka Topic (Optional but Recommended)
```bash
docker exec -it redpanda rpk topic create weather_data_demo
```

Verify:
```bash
docker exec -it redpanda rpk topic list
```

## Step 4: Create the Producer File
```bash
nano main.py
```

Paste in your Python producer code.

## Step 5: Set Up Python Environment
```bash
python -m venv VenV
source VenV/bin/activate
pip install quixstreams requests
```

## Step 6: Run the Producer
```bash
python main.py
```

Expected output:

Weather API request succeeds

JSON data logged

Message produced to Kafka

Script sleeps for 5 minutes

## Step 7: Consume Messages
Using Redpanda (Recommended)

Read from the beginning:
```bash
docker exec -it redpanda rpk topic consume weather_data_demo -o start
```
## Notes

-o start → reads all existing messages

