# 🧠 Monitoring Stack (Prometheus + Grafana + Alertmanager + Node Exporter)

A complete monitoring and alerting stack built with **Docker Compose**, including:

- **Prometheus** – metrics collection and storage  
- **Node Exporter** – system metrics exporter for Linux hosts  
- **Alertmanager** – alert processing and routing  
- **Grafana** – visualization and dashboards  

All services are configured to run together on a single Docker network for simplicity.

---

## 🗺️ Architecture

            ┌────────────────────────────┐
            │        Node Exporter       │
            │  (Host metrics: CPU, RAM)  │
            └─────────────┬──────────────┘
                          │
                          ▼

┌─────────────────────────────┴──────────────────────────────┐
│ Prometheus │
│ Collects metrics from exporters and stores time-series data│
└─────────────┬──────────────────────────────────────────────┘
│
│ (sends alerts)
▼
┌────────────────────┐
│ Alertmanager │
│ Routes alerts to │
│ Email/Slack/etc. │
└─────────┬──────────┘
│
│ (visualizes metrics)
▼
┌────────────────────┐
│ Grafana │
│ Dashboards & Alerts│
└────────────────────┘


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

Main config file:

./prometheus/prometheus.yml

Example configuration:

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

You can add receivers for Email, Slack, Telegram, etc.
📊 Grafana Setup

Once Grafana is running:

    Login at http://localhost:3000

    Go to Connections → Data Sources

    Add Prometheus

        URL: http://prometheus:9090

    Click Save & Test

    Import dashboards or create your own visualizations

🧩 Docker Volumes

Persistent data is stored in Docker volumes:

    prometheus-data → /prometheus

    grafana-data → /var/lib/grafana

To clean everything (including data):

docker compose down -v

🧱 Network

All services share a Docker network called monitoring, defined in the compose file:

networks:
  monitoring:
    name: monitoring

This allows inter-service access by container name, e.g. http://prometheus:9090.
🛠️ Useful Commands
Command	Description
docker compose up -d	Start all services
docker compose down	Stop and remove containers
docker compose logs -f	Stream live logs
docker exec -it grafana bash	Enter Grafana container
docker exec -it prometheus sh	Enter Prometheus container
🧩 Future Improvements

    Add cAdvisor or Blackbox Exporter for advanced metrics

    Configure Slack / Telegram alerts in Alertmanager

    Secure Grafana and Prometheus with TLS

    Add reverse proxy (Traefik / Nginx) for HTTPS access

    Deploy on Docker Swarm / Kubernetes for scalability
