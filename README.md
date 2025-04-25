# 📡 EmoStream: Concurrent Emoji Broadcast over Event-Driven Architecture

**EmoStream** is a real-time emoji broadcasting system designed to enhance viewer engagement during live events like cricket matches. It captures user-generated emoji reactions, processes them at scale using Apache Kafka and Spark Streaming, and delivers aggregated emoji sentiment in real-time to millions of users.

## 🚀 Project Objective

To build a **scalable, low-latency, event-driven architecture** that:
- Captures billions of user emoji reactions in real-time
- Aggregates them using micro-batch streaming (every 2 seconds)
- Broadcasts the collective sentiment back to all connected users using a Pub-Sub model

---

## 🏗️ System Architecture

The system has **3 main stages**:

### 1. API Ingestion + Kafka Producer
- A **Flask API** receives `POST` requests containing:
  - `user_id`
  - `emoji_type`
  - `timestamp`
- Emoji data is asynchronously written to a **Kafka producer**
- Kafka flush interval: **500 milliseconds**

### 2. Real-Time Processing (Spark Streaming)
- A **Spark Structured Streaming** job consumes emoji data from Kafka
- Data is micro-batched every **2 seconds**
- Aggregation Logic:
  - For every **1000 emojis or fewer** of the same type, one emoji count is retained
  - Example: 3785 ❤️ → Aggregated as 3 ❤️

### 3. Emoji Broadcast (Pub-Sub Architecture)
- A **Main Publisher** receives aggregated data from Spark
- Data is forwarded to **3+ Clusters**, each with:
  - A **Cluster Publisher**
  - **3+ Subscribers**
- All subscribers within a cluster receive the same message simultaneously
- Real-time delivery back to **registered clients**

---

## 🔄 Client Interaction

- Clients can:
  - **Send emoji data** via API
  - **Register** to receive live emoji sentiment from subscribers
- Clients can **join or leave dynamically**, with no interruption to the ongoing stream

---

## 🧪 Testing Strategy

### ✅ Unit Testing
- Tested the Flask API for high throughput handling of thousands of emoji requests/sec

### 🧪 Load Testing
- Simulated **200+ clients**, each sending **1000+ emojis/sec**
- Ensured stable delivery and processing with low latency

---


## 📦 Technologies Used

- **Python + Flask** – REST API backend
- **Apache Kafka** – Event streaming platform
- **Apache Spark Structured Streaming** – Real-time data processing
- **Threading / Async IO** – Handling concurrent clients
- **Pub-Sub Architecture** – For scalable real-time delivery

---

## 📸 Demo Highlights

- Live demo of 200+ clients sending emoji data
- Aggregated results reflected back to CLI clients every 2 seconds
- Auto-scalable cluster setup with seamless client join/leave capability

---

## 📚 References

- [ByteByteGo: Billion Scale Emoji Reactions](https://www.youtube.com/watch?v=UN1kW5AHid4)
- [Hotstar Pub-Sub System Design](https://blog.hotstar.com/building-pubsub-for-50m-concurrent-socket-connections-5506e3c3dabf)
- [High Scalability: Capturing a Billion Emojis](https://highscalability.com/capturing-a-billion-emo-j-i-ons/)
- [HasGeekTV – Real-time Data Systems](https://www.youtube.com/watch?v=sF7HN9L54SE)
- https://hackmd.io/@pesu-bigdata/H1tiaLseye

---

## 🏁 Final Deliverables

- ✅ Asynchronous emoji ingestion API
- ✅ Kafka producer with flush intervals
- ✅ Spark Streaming engine with 2-sec micro-batching
- ✅ Aggregation algorithm for emoji data
- ✅ Real-time Pub-Sub message delivery to CLI clients
- ✅ Client registration & management interface
- ✅ Demo setup with 200+ concurrent clients

---
