# 🚀 Project Features & Roadmap

This document outlines the core functional and non-functional requirements for **FleetTracker**. It serves as a checklist for development and a guide to the system's capabilities.

## 📡 Phase 1: Telemetry Simulation (The Edge)

_Simulating a realistic fleet of IoT devices under varying network conditions._

- [ ] **Physics-Based Traffic Generator**
  - [ ] Implement `sim.py` to generate $N$ vehicles moving realistically along coordinates (not random jumping).
  - [ ] Simulate fuel decay and engine temperature fluctuations.
- [ ] **Network Quality Simulation (Chaos Engineering)**
  - [ ] **Jitter:** Randomize the interval between packet sends (0.8s - 1.2s).
  - [ ] **Packet Loss:** Randomly drop 5% of packets to test QoS handling.
  - [ ] **Thundering Herd:** Simulate a "tunnel exit" scenario where a car buffers 60 seconds of data and uploads it all at once upon reconnection.
- [ ] **MQTT Publishing**
  - [ ] Implement lightweight JSON payloads.
  - [ ] Use **QoS 1** (At Least Once) for critical alerts (Crashes).
  - [ ] Use **QoS 0** (At Most Once) for high-frequency speed updates.

## 🛡️ Phase 2: High-Performance Ingestion

_Handling scale, concurrency, and data integrity._

- [ ] **MQTT Broker Setup**
  - [ ] Deploy Eclipse Mosquitto via Docker.
  - [ ] Configure ACLs (Access Control Lists) to prevent unauthorized publishers.
- [ ] **Ingestion Microservice (Go)**
  - [ ] **Concurrency:** Implement Worker Pools to handle 10k+ concurrent connections.
  - [ ] **Validation:** Discard corrupt packets (e.g., negative speed, out-of-bounds Lat/Long).
  - [ ] **Serialization:** Convert incoming JSON to **Protocol Buffers** for efficient internal transport.
- [ ] **Event Streaming (Kafka)**
  - [ ] Buffer validated messages into `vehicle-events` topic.
  - [ ] Implement partitions to allow parallel processing by multiple consumers.

## ⚙️ Phase 3: Processing & Analytics

_Deriving value from raw data streams._

- [ ] **Real-Time Stream Processor (Go)**
  - [ ] **Crash Detection:** Detect if speed drops from >60km/h to 0km/h in <2 seconds.
  - [ ] **Geofencing:** Flag events if a vehicle leaves a designated operating zone.
- [ ] **Data Persistence Strategy**
  - [ ] **Batch Writing:** Implement a robust buffering mechanism to write to DB in batches of 500 records.
  - [ ] **TimescaleDB Setup:** Configure Hypertables for automatic time-partitioning.
  - [ ] **Retention Policy:** Auto-delete raw data > 7 days old; keep downsampled data for 1 year.

## 🖥️ Phase 4: The Dashboard

_Visualizing the fleet for operators._

- [ ] **Live Fleet Map**
  - [ ] Integrate **Deck.gl** with Next.js.
  - [ ] Render 1,000+ moving vehicle icons smoothly using WebGL (GPU).
  - [ ] Color-code vehicles based on status (Green = Moving, Red = Stopped/Crash).
- [ ] **Real-Time Updates**
  - [ ] Implement a **WebSocket Server** in Go to push updates to the frontend immediately upon processing.
  - [ ] "Live Mode" toggle on the UI.
- [ ] **Historical Playback**
  - [ ] Time-scrubber UI to replay a vehicle's route from the past 24 hours.
  - [ ] Speed graph visualization using Recharts.

## 🏆 Bonus / Advanced Features (Stretch Goals)

_Features designed to impress Senior System Design interviewers._

- [ ] **Dead Letter Queue (DLQ):** Route invalid/corrupted messages to a special Kafka topic for debugging instead of silently dropping them.
- [ ] **Driver Score:** Calculate a safety score (0-100) based on harsh braking and over-speeding events.
- [ ] **API Rate Limiting:** Prevent the frontend from spamming the historical data API.
