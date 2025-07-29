# 📘 Joplin Docker Compose Stack

[![MIT License](https://img.shields.io/github/license/Vantasin/Joplin?style=flat-square)](LICENSE)
[![Docker Compose](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)](https://docs.docker.com/compose/)
[![Docker](https://img.shields.io/badge/Docker-compose-2496ED?logo=docker&logoColor=white&style=flat-square)](https://www.docker.com/)
[![ZFS](https://img.shields.io/badge/ZFS-OpenZFS-blue?style=flat-square)](https://openzfs.org/)

[![Joplin](https://img.shields.io/badge/Joplin-Note%20Server-blue?logo=joplin&logoColor=white)](https://joplinapp.org/)

A self-hosted Joplin Server setup using Docker Compose. Easily sync notes across devices with end-to-end encryption, PostgreSQL backend, and optional reverse proxy support.

---

## 📁 Directory Structure

```bash
tank/
├── docker/
│   ├── compose/
│   │   └── joplin/              # Git repo lives here
│   │       ├── docker-compose.yml  # Main Docker Compose config
│   │       ├── .env                # Runtime environment variables and secrets (gitignored!)
│   │       ├── env.example         # Example .env file for reference
│   │       └── README.md           # This file
│   └── data/
│       └── joplin/              # Volume mounts and persistent data
```

---

## 🧰 Prerequisites

* Docker Engine
* Docker Compose V2
* Git
* (Optional) ZFS on Linux for dataset management

> ⚠️ **Note:** These instructions assume your ZFS pool is named `tank`. If your pool has a different name (e.g., `rpool`, `zdata`, etc.), replace `tank` in all paths and commands with your actual pool name.

---

## ⚙️ Setup Instructions

1. **Create the stack directory and clone the repository**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/compose/joplin
   cd /tank/docker/compose/joplin
   sudo git clone https://github.com/Vantasin/Joplin.git .
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/compose/joplin
   cd ~/docker/compose/joplin
   git clone https://github.com/Vantasin/Joplin.git .
   ```

2. **Create the runtime data directory** (optional)

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/data/joplin
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/data/joplin
   ```

3. **Configure environment variables**

   Copy and modify the `.env` file:

   ```bash
   sudo cp env.example .env
   sudo nano .env
   sudo chmod 600 .env
   ```

   > **Note:** Be sure to update the `POSTGRES_PASSWORD`, `APP_BASE_URL`, and if necessary the `JOPLIN_DATA_VOLUME`.

   > **Note:** `APP_BASE_URL` is the base public URL where the service will be running.
   > If Joplin Server needs to be accessible over the internet, configure `APP_BASE_URL` as follows: `https://joplin.example.com`.
   
   > **Tip:** You can create the `APP_BASE_URL` using [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy for HTTPS certificates via Let's Encrypt.
   
   > **Optional:**  If Joplin Server does not need to be accessible over the internet, set the `APP_BASE_URL` to your server's hostname. 
   > For Example: `http://localhost:22300` or replace `localhost` with your server’s IP address. The base URL can include the port.
   
4. **Start joplin**

   ```bash
   docker compose up -d
   ```

---

## 🌐 Accessing Joplin Web UI

Once deployed, access **Joplin** using:

- **Web Interface:** Enter the URL that was set for `APP_BASE_URL`. Eg. `https://joplin.example.com` or `http://localhost:22300`.
  > **Default Email:** `admin@localhost`
  > **Default Password:** `admin`
- **Initial Setup:** After logging in with the default email and password be sure to change the default email address and password.
  > **Note:**
  > Email address and password can be changed via the profile tab in the top right corner.
  > Changing the email address requires going to the **Admin tab -> Email tab -> Confirm your new Joplin Server account email**

---

## 🙏 Acknowledgments

- [ChatGPT](https://openai.com/chatgpt) — for assistance in generating setup scripts and templates.
- [Docker](https://www.docker.com/) — for container orchestration and runtime.
- [OpenZFS](https://openzfs.org/) — for advanced local filesystem features, dataset organization, and snapshotting.
- [Joplin](https://joplinapp.org/) — the open-source note-taking and to-do app with synchronization and encryption.