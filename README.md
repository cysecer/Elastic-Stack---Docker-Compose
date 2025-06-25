# 🐳 Elastic Stack with Fleet Server – Docker Compose Setup

This repository contains a `docker-compose.yml` file that sets up an Elastic Stack environment locally using Docker Desktop on **Windows**. It includes:

- **3 Elasticsearch containers** (master + 2 data nodes)
- **Kibana**
- **Fleet Server**

---

## 🚀 Getting Started

### 🛠 Requirements

- Docker Desktop (tested on Windows)
- Docker Compose v2+

---

## ▶️ Running the Stack

To start the Elastic Stack:

```bash
docker compose up
```
💡 Make sure Docker Desktop is running before executing the command.

📈 Accessing Kibana
Once the containers are up, you can access Kibana at:

http://localhost:5601

🔐 Fleet Setup (Manual Steps Required)
To allow the Fleet Server to join the stack, you must manually add a Fleet agent policy in Kibana:

Go to Fleet under Management in Kibana.

Create a new agent policy with the following values:

Policy Name: fleet-server-policy

Fleet Server host URL: https://localhost:8220
(Or use the URL defined in your docker-compose.yml if modified)

Save the policy.

❗ Make sure to change any default passwords in the docker-compose.yml file before using this in production or shared environments.

📜 Add CA Certificate to Agent Hosts
When adding a new Elastic Agent to Fleet:

Download the ca.crt file from the elastic_certs Docker Volume.
You can extract it like this:

bash
Kopieren
Bearbeiten
docker cp $(docker volume inspect --format '{{ .Mountpoint }}' elastic_certs)/ca/ca.crt ./ca.crt
Install ca.crt as a trusted root certificate on the host where the agent is running.

On Windows, use Certificates Manager (certmgr.msc) to import it into Trusted Root Certification Authorities.

On Linux/macOS, follow OS-specific steps to add a trusted root CA.

📂 Project Structure
text
Kopieren
Bearbeiten
.
├── docker-compose.yml
└── README.md
✅ Notes
This setup is intended for local development/testing only.

Only tested on Docker Desktop for Windows.

Do not use in production without securing credentials and certificates properly.

Check the Elastic documentation for additional security hardening and best practices.

📚 Resources
Elastic Stack Documentation

Fleet and Elastic Agent Guide

🧹 Cleanup
To stop and remove all containers:

bash
Kopieren
Bearbeiten
docker compose down -v
🙋‍♂️ Issues or Questions?
If something doesn't work as expected, feel free to open an issue or suggest improvements.

yaml
Kopieren
Bearbeiten

---

Let me know if you want the corresponding `docker-compose.yml` file or help with securing your setup!
