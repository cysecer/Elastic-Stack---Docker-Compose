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
## 💡 Prerequisites

Make sure **Docker Desktop is running** before executing any commands.

---

## 📈 Accessing Kibana

Once the containers are up, you can access Kibana at:

👉 **[http://localhost:5601](http://localhost:5601)**

---

## 🔐 Fleet Setup (Manual Steps Required)

To allow the **Fleet Server** to join the stack, you must manually add a Fleet Agent policy in **Kibana**:

1. Go to **Fleet** under **Management** in Kibana.
2. Create a new Agent policy with the following details:
   - **Policy Name**: `fleet-server-policy`
   - **Fleet Server host URL**: `https://localhost:8220`  
     _(Or use the URL defined in your `docker-compose.yml` if modified)_
3. Save the policy.

> ❗ **Important**: Change all default passwords in the `docker-compose.yml` file before using this in any shared or production-like environment.

---

## 📜 Add CA Certificate to Agent Hosts

When adding a new **Elastic Agent** to Fleet:

1. Download the `ca.crt` file from the `elastic_certs` **Docker Volume**:

   ```bash
   docker cp $(docker volume inspect --format '{{ .Mountpoint }}' elastic_certs)/ca/ca.crt ./ca.crt
   ```
2. Install `ca.crt` as a **trusted root certificate** on the host where the Elastic Agent is running:

- **Windows**: Use **Certificates Manager** (`certmgr.msc`) to import the certificate into **Trusted Root Certification Authorities**.
- **Linux/macOS**: Follow your OS-specific procedure to trust a custom root CA (e.g., place the cert in `/usr/local/share/ca-certificates/` and run `update-ca-certificates` on Debian-based systems).

---

## 📂 Project Structure

```text
.
├── docker-compose.yml
└── README.md
```
---

## ✅ Notes

- This setup is intended for **local development/testing only**.
- Tested exclusively on **Docker Desktop for Windows**.
- **Not suitable for production** without:
  - Securing all passwords and credentials
  - Managing TLS certificates correctly
  - Enabling proper network security
- Refer to the official Elastic documentation for **hardening** and **deployment best practices**.

---

## 📚 Resources

- 📘 [Elastic Stack Documentation](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html)
- 🧭 [Fleet and Elastic Agent Guide](https://www.elastic.co/guide/en/fleet/current/fleet-overview.html)

---

## 🧹 Cleanup

To stop and remove all containers and volumes, run:

```bash
docker compose down -v


