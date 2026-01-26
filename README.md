# 🔗 URL Shortener

A **production-ready URL Shortener service** built with **Spring Boot**, using **MongoDB** for persistence, **Redis** for caching, and **Docker** for containerized deployment. The service allows you to create short, memorable URLs with optional expiration dates and is designed to resemble real-world backend systems.

![Java](https://img.shields.io/badge/Java-21-orange.svg)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-brightgreen.svg)
![MongoDB](https://img.shields.io/badge/MongoDB-Latest-green.svg)
![Redis](https://img.shields.io/badge/Redis-Latest-red.svg)
![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)

---

## ✨ Features

* **URL Shortening** – Convert long URLs into short, shareable links
* **Duplicate Detection** – Same URL always returns the same short link
* **Expiration Support** – Optional expiration dates for temporary links
* **High Performance** – Redis caching for fast redirects
* **Auto Expiration** – MongoDB TTL indexes clean up expired URLs automatically
* **Docker Ready** – One-command startup with Docker Compose
* **Production Setup** – Nginx reverse proxy included
* **RESTful APIs** – Clean and predictable API design

---

## 🏗️ Architecture

### System Overview

```
Client
   │
   ▼
Nginx (Port 80)
   │
   ▼
Spring Boot (Port 8082)
   │
   ├── MongoDB (Primary Storage)
   └── Redis (Cache)
```

### Request Flow

**Shorten URL**

1. Client → Nginx → Spring Boot
2. Spring Boot → MongoDB (store URL)
3. Spring Boot → Redis (cache result)
4. Response returned to client

**Redirect Short URL**

1. Client → Nginx → Spring Boot
2. Redis lookup
3. MongoDB fallback (if cache miss)
4. HTTP 302 redirect

---

## 🧱 Tech Stack

* **Java 21** – Modern Java features
* **Spring Boot 3.4.5** – Backend framework
* **Spring Data MongoDB** – Persistence layer
* **Spring Data Redis** – Caching layer
* **MongoDB** – URL storage with TTL support
* **Redis** – High-speed in-memory cache
* **Docker & Docker Compose** – Containerization
* **Nginx** – Reverse proxy
* **Maven** – Build and dependency management

---

## 📋 Prerequisites

* Java 21+
* Docker Desktop
* Maven 3.6+
* Git

**Windows Users**

* WSL 2 enabled
* Virtualization enabled in BIOS

---

## 🚀 Quick Start

### Option 1: Docker Compose (Recommended)

```bash
git clone <repository-url>
cd url-shortener
cp .env.example .env
```

Edit `.env` if required:

```
APP_PORT=8082
MONGO_PORT=27017
MONGO_USER=root
MONGO_PASSWORD=password
MONGO_DB=url-shortener-db
REDIS_PORT=6379
```

Start the application:

```bash
docker compose --profile production up --build -d
```

Access:

* Direct: [http://localhost:8082](http://localhost:8082)
* Via Nginx: [http://localhost:80](http://localhost:80)

---

### Option 2: Local Development

Start dependencies:

```bash
mongod
redis-server
```

Run the app:

```bash
./mvnw spring-boot:run
```

---

## 📡 API Documentation

### Create Short URL

**POST** `/shorten`

Request:

```json
{
  "url": "https://www.example.com",
  "expiresAt": "2025-12-31T23:59:59"
}
```

Response:

```json
{
  "shortUrl": "http://localhost:8082/abc123",
  "expiresAt": "2025-12-31T23:59:59"
}
```

---

### Redirect Short URL

**GET** `/{id}`

Responses:

* `302` → Redirects to original URL
* `404` → URL not found or expired

---

## 🐳 Docker Commands

```bash
docker compose up -d
docker compose down
docker compose logs -f app
docker compose ps
```

---

## 📁 Project Structure

```
url-shortener/
├── src/main/java/com/thenriquedb/url_shortener
│   ├── configuration
│   ├── controllers
│   ├── dtos
│   ├── repositories
│   ├── services
│   └── shared
├── docker-compose.yml
├── docker-swarm.yml
├── Dockerfile
├── nginx.conf
└── README.md
```

---

## 🚢 Deployment (Docker Swarm)

Create secrets:

```bash
echo 8082 | docker secret create APP_PORT -
echo mongo | docker secret create MONGO_HOST -
echo 27017 | docker secret create MONGO_PORT -
echo root | docker secret create MONGO_USER -
echo password | docker secret create MONGO_PASSWORD -
echo url-shortener-db | docker secret create MONGO_DB -
echo cache | docker secret create REDIS_HOST -
echo 6379 | docker secret create REDIS_PORT -
```

Deploy:

```bash
docker swarm init
docker stack deploy -c docker-swarm.yml url_shortener
```

---

## 🐛 Troubleshooting

* **Port in use** → Change `APP_PORT`
* **MongoDB not reachable** → `docker compose logs mongo`
* **Redis issues** → `docker compose logs cache`
* **500 errors** → `docker compose logs app`

---

## 📊 Performance Notes

* Redis caching reduces DB reads
* MongoDB TTL cleans expired URLs automatically
* Duplicate detection avoids redundant entries

---

## 🤝 Contributing

Pull requests are welcome.

```bash
git checkout -b feature/your-feature
git commit -m "Add new feature"
git push origin feature/your-feature
```

---

## 📝 License

MIT License

---

## 👤 Author

**Anbu**
GitHub: [@anbu-max](https://github.com/anbu-max)

---

⭐ If this project helped you learn backend systems, consider starring the repo.
