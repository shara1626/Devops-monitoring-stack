# 🚀 DevOps Monitoring Stack

A production-ready monitoring and observability stack built with open source tools, deployed on AWS EC2 using Docker Compose.

## 📐 Architecture
                     ┌─────────────────────┐
                     │     EC2 Instance    │
                     │  (Docker Host)      │
                     └─────────┬───────────┘
                               │
     ┌─────────────────────────┼──────────────────────────┐
     │                         │                          │
     ▼                         ▼                          ▼
┌──────────────┐      ┌──────────────┐        ┌────────────────┐
│ Prometheus   │      │ Filebeat     │        │ Node Exporter  │
│ (Metrics)    │      │ (Logs ship)  │        │ (System metrics│
└──────┬───────┘      └──────┬───────┘        └──────┬─────────┘
       │                     │                          │
       ▼                     ▼                          ▼
┌────────────────────────────────────────────────────────────┐
│                  Elasticsearch                             │
│               (Log Storage & Indexing)                    │
└───────────────────────────┬────────────────────────────────┘
                            ▼
                     ┌──────────────┐
                     │   Kibana     │
                     │ Log Analysis │
                     └──────────────┘

                            │
                            ▼
                     ┌──────────────┐
                     │   Grafana    │
                     │ Dashboards   │
                     └──────────────i

## 🛠️ Tech Stack

| Tool | Purpose | Port |
|------|---------|------|
| Prometheus | Metrics collection | 9090 |
| Node Exporter | System metrics agent | 9100 |
| Grafana | Metrics visualization | 3000 |
| Elasticsearch | Log storage & indexing | 9200 |
| Filebeat | Log shipping | — |
| Kibana | Log visualization | 5601 |

## ✅ Prerequisites

- AWS EC2 instance (Amazon Linux 2)
- Docker & Docker Compose installed
- Ports 3000, 9090, 9200, 5601 open in Security Group

## 🚀 Quick Start

### 1. Clone the repository
```bash
git clone git@github.com:YOUR_USERNAME/devops-monitoring-stack.git
cd devops-monitoring-stack
```

### 2. Start the stack
```bash
docker-compose up -d
```

### 3. Verify containers are running
```bash
docker-compose ps
```

### 4. Access the dashboards

| Service | URL | Credentials |
|---------|-----|-------------|
| Grafana | http://\<EC2-IP\>:3000 | admin / admin123 |
| Prometheus | http://\<EC2-IP\>:9090 | — |
| Kibana | http://\<EC2-IP\>:5601 | — |

## 📊 Grafana Setup

1. Login to Grafana → **Configuration** → **Data Sources**
2. Add **Prometheus** → URL: `http://prometheus:9090`
3. Import dashboard ID **1860** (Node Exporter Full)

## 📋 Kibana Setup

1. Open Kibana → **Stack Management** → **Index Patterns**
2. Create index pattern: `filebeat-*`
3. Go to **Discover** to view logs

## 📁 Project Structure

monitoring-stack/
├── docker-compose.yml        # Main orchestration file (all services)
├── prometheus/
│   └── prometheus.yml        # Prometheus scrape configuration
├── filebeat/
│   └── filebeat.yml          # Filebeat log shipping configuration
├── grafana/                  # Grafana persistent data storage
└── README.md                 # Project documentation

## 🔧 Useful Commands

```bash
# Start stack
docker-compose up -d

# Stop stack
docker-compose down

# View logs
docker-compose logs -f

# Restart a service
docker-compose restart grafana
```

## 📌 What I Learned

- Setting up a full observability stack from scratch
- Containerizing multiple services with Docker Compose
- Collecting system metrics with Prometheus & Node Exporter
- Shipping and indexing logs with ELK stack
- Building dashboards in Grafana and Kibana
---

# 🔄 Recovery & Redeployment

## (Before Terminating EC2)

```bash
cd ~/monitoring-stack

# Check what's not yet committed
git status

# Add everything
git add .

# Commit changes
git commit -m "feat: add full monitoring stack configs"

# Push to GitHub
git push
```

### Ensure These Files Exist in Your Repository

```text
monitoring-stack/
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml
├── filebeat/
│   └── filebeat.yml
├── screenshots/
└── README.md
```

---

# 🚀 Redeploy on a New EC2 Instance

## 1. Install Docker

```bash
sudo yum install -y docker
sudo systemctl start docker
sudo usermod -aG docker ec2-user
newgrp docker
```

## 2. Install Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

## 3. Clone the Repository

```bash
git clone git@github.com:YOUR_USERNAME/devops-monitoring-stack.git
cd devops-monitoring-stack
```

## 4. Start the Monitoring Stack

```bash
docker-compose up -d
```

---

✅ Full monitoring stack restored and running within minutes.
