# 🧠 Monitoring Stack (Prometheus + Grafana + Alertmanager + Node Exporter)

A complete monitoring and alerting stack built with **Docker Compose**, including:

- **Prometheus** – metrics collection and storage  
- **Node Exporter** – system metrics exporter for Linux hosts  
- **Alertmanager** – alert processing and routing  
- **Grafana** – visualization and dashboards  

All services are configured to run together on a single Docker network for simplicity.

---

## 📁 Project Structure



monitoring-stack/
│
├── docker-compose.yml
│
├── prometheus/
│ └── prometheus.yml
│
├── alertmanager/
│ └── alertmanager.yml
│
└── grafana/
└── (data persisted in Docker volume)

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/<your-username>/monitoring-stack.git
cd monitoring-stack

2️⃣ Start the Stack

docker compose up -d

This will start:

    Prometheus on port 9090

    Node Exporter on port 9100

    Alertmanager on port 9093

    Grafana on port 3000

Check all containers are running:

docker ps

🌐 Access the Services
Service	URL	Description
Grafana	http://localhost:3000
Visualization UI
Prometheus	http://localhost:9090
Metrics database
Alertmanager	http://localhost:9093
Alert routing
Node Exporter	http://localhost:9100
System metrics exporter

Default Grafana login:

Username: admin  
Password: admin

⚙️ Configuration
🔸 Prometheus

Edit the main config file:

./prometheus/prometheus.yml

Example targets:

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["prometheus:9090"]

  - job_name: "node"
    static_configs:
      - targets: ["node-exporter:9100"]

🔸 Alertmanager

Configure alert routing in:

./alertmanager/alertmanager.yml

You can add email, Slack, or Telegram receivers.
📊 Grafana Setup

Once Grafana is up:

    Login at http://localhost:3000

    Go to Connections → Data Sources

    Add Prometheus

        URL: http://prometheus:9090

    Save and test connection

    Import prebuilt dashboards or create your own

🧩 Docker Volumes

Persistent data is stored in Docker volumes:

    prometheus-data → /prometheus

    grafana-data → /var/lib/grafana

To remove all data (optional cleanup):

docker compose down -v

🧱 Network

All services are connected via a custom Docker network:

networks:
  monitoring:
    name: monitoring

This allows inter-service communication by name (e.g. http://prometheus:9090).
🛠️ Useful Commands
Command	Description
docker compose up -d	Start the stack
docker compose down	Stop the stack
docker compose logs -f	View logs
docker exec -it grafana bash	Access Grafana container
docker exec -it prometheus /bin/sh	Access Prometheus container
🧩 Future Improvements

    Add cAdvisor or Blackbox Exporter for more metrics

    Configure Slack / Telegram alerts in Alertmanager

    Deploy stack with Traefik / Nginx reverse proxy

    Secure Grafana and Prometheus with TLS
