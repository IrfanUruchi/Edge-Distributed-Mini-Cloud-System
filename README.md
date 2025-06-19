# Edge-Distributed-Mini-Cloud-System

---

## Overview  
This repository contains a two-node Docker deployment running on a Raspberry Pi 4B and a MacBook Pro. Services include file sync (Nextcloud), SMB sharing (Samba), interactive notebooks (Jupyter Lab), local LLM inference (LLaMA & LM Studio), a web-based Chat UI, and a full Prometheus-Grafana monitoring stack.


---

## Architecture  
![Deployment Diagram](./diagrams/System_architecture_diagram.pdf)  
- **Raspberry Pi 4B (192.168.0.110)** hosts 10 containers:  
  Nextcloud, Samba, Jupyter Lab, LLaMA-CPP, static web server, Chat-UI (Nginx), Prometheus, Grafana, cAdvisor, Node Exporter.  
- **MacBook Pro (169.254.1.1)** runs LM Studio (127.0.0.1:1234), an Nginx proxy (0.0.0.0:1234) and Node Exporter.  
- A dedicated USB-Ethernet link (169.254.1.x/16) carries API calls and monitoring traffic.


---

## Prerequisites  
- **Raspberry Pi 4B** with Docker & Docker Compose  
- **Other node** with Docker Desktop and LM Studio installed  
- **USB-Ethernet cable** for a direct link between hosts  


---


## Setup 

### 1. Other node (LM Studio proxy)  
```bash
cd llm-proxy-docker
docker build -t lmstudio-proxy .
docker run -d --name lm-proxy -p 1234:1234 lmstudio-proxy
```

### 3. Raspberry Pi (or other device as main node)

```bash
git clone https://github.com/IrfanUruchi/Edge-Distributed-Mini-Cloud-System
cd Edge-Distributed-Mini-Cloud-System/
docker-compose up -d
```

This brings up Nextcloud, Samba, Jupyter Lab, LLaMA inference, Chat-UI, Prometheus, Grafana, cAdvisor, and Node Exporter.


---


## Usage

- Chat UI: http://<PI_IP>:3001/

- Jupyter Lab: http://<PI_IP>:8888/

- Nextcloud: http://<PI_IP>:8080/

- Grafana: http://<PI_IP>:3000/ (default admin credentials)


---
