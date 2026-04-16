#  FastAPI Dockerized Application

This project is a simple FastAPI application containerized using Docker and orchestrated with Docker Compose. It provides basic user APIs and demonstrates data persistence using Docker volumes.

---

##  Features

* FastAPI-based REST API
* Dockerized application
* Docker Compose orchestration
* Persistent storage using volumes
* JSON-based data storage (no database)
* Easy deployment on AWS EC2

---

##  Project Structure

```
.
├── app/
│   ├── main.py
│   └── ...
├── data/
│   └── users.json
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

##  Requirements

### System Requirements

* Docker (latest version recommended)
* Docker Compose (v2+)
* Git

### Verify Installation

```bash
docker --version
docker compose version
git --version
```

---

##  Python Dependencies

Defined in `requirements.txt`:

```
fastapi
uvicorn
```

Example:

```
black==23.1.0
fastapi==0.92.0
uvicorn==0.20.0
```

---

##  Deploy on AWS EC2

---

### Step 1: Launch EC2 Instance

Go to AWS Console → EC2 → Launch Instance

* Instance Type: `t2.medium`
* OS: `Ubuntu 22.04`
* Storage: 8 GB+
* Key Pair: Download `.pem`

---

### Step 2: Configure Security Group

Allow:

| Type | Port |
| ---- | ---- |
| SSH  | 22   |
| HTTP | 80   |
| TCP  | 8000 |

---

### Step 3: Connect to EC2

```bash
ssh -i saurav.pem ubuntu@<YOUR_PUBLIC_IP>
```

---

### Step 4: Install Docker

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

---

### Step 5: Enable Docker

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
```

(Optional - run without sudo)

```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

---

### Step 6: Verify Docker

```bash
docker run hello-world
docker compose version
```

---

### Step 7: Clone Repository

```bash
git clone https://github.com/SauravBagade/docker-fastapi-test.git
cd docker-fastapi-test
```

---

### Step 8: Run Application

```bash
docker compose up -d --build
```

---

### Step 9: Access Application

```
http://<YOUR_PUBLIC_IP>:8000/docs
```

---

##  API Endpoints

| Method | Endpoint | Description     |
| ------ | -------- | --------------- |
| GET    | /        | Hello message   |
| GET    | /users   | Fetch all users |
| POST   | /users   | Add new user    |

---

##  Data Persistence

Data stored at:

```
/app/data/users.json
```

Volume mapping:

```yaml
volumes:
  - ./data:/app/data
```

---

##  Test Persistence

1. Add user via `/docs`
2. Stop container:

```bash
docker compose down
```

3. Restart:

```bash
docker compose up -d
```

4. Check `/users`

 Data persists

---

##  Useful Commands

Check containers:

```bash
docker ps
```

Logs:

```bash
docker compose logs -f
```

Stop:

```bash
docker compose down
```

Remove volumes:

```bash
docker compose down -v
```

---

##  Troubleshooting

### Docker not found

```bash
docker: command not found
```

 Install Docker properly and restart terminal

---

### Port already in use

```bash
Error: port 8000 already in use
```

Fix:

```bash
sudo lsof -i :8000
kill -9 <PID>
```

---

### Permission denied (Docker)

```bash
permission denied while connecting to Docker
```

Fix:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---