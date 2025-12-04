    github.com/yourname/fleet-tracker/
    ├── apps/
    │ ├── dashboard/ <-- Next.js (The Frontend)
    │ │ ├── src/
    │ │ └── Dockerfile
    │ │
    │ ├── ingestion/ <-- Go + Kafka Producer (The High-Speed Ingestor)
    │ │ ├── main.go
    │ │ └── Dockerfile
    │ │
    │ ├── processor/ <-- Go + TimescaleDB Writer (The Data Consumer)
    │ │ ├── main.go
    │ │ └── Dockerfile
    │ │
    │ └── simulator/ <-- Python (The Traffic Generator)
    │ ├── sim.py
    │ └── Dockerfile
    │
    ├── packages/ <-- Shared Code
    │ └── proto/ <-- (Bonus Points) Shared Protocol Buffers definitions
    │ └── vehicle.proto
    │
    ├── docker-compose.yml <-- The magic button that starts EVERYTHING
    └── README.md
