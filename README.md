# Expensy - Expense Tracker

A full-stack expense tracking application built with **Next.js** (frontend) and **Node/Express** (backend), using **MongoDB** for storage and **Redis** for caching.

## Architecture

```
┌──────────────────┐     ┌──────────────────┐     ┌─────────────┐
│   Next.js        │────►│   Express API    │────►│  MongoDB    │
│   Frontend       │     │   Backend        │     │             │
│   (port 3000)    │     │   (port 8706)    │     └─────────────┘
└──────────────────┘     │                  │     ┌─────────────┐
                         │   /api/expenses  │────►│  Redis      │
                         │   /metrics       │     │  (cache)    │
                         └──────────────────┘     └─────────────┘
```

## Quick Start

### 1. Start databases

```bash
# MongoDB
docker run --name mongo -d -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=example \
  mongo:latest

# Redis
docker run --name redis -d -p 6379:6379 \
  redis:latest \
  redis-server --requirepass someredispassword
```

### 2. Start backend

```bash
cd expensy_backend
npm install
npm start
```

### 3. Start frontend

```bash
cd expensy_frontend
npm install
npm run dev
```

## Environment Variables

### Backend (`expensy_backend/.env`)

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `8706` | Server port |
| `DATABASE_URI` | `mongodb://root:example@localhost:27017` | MongoDB connection |
| `REDIS_HOST` | `localhost` | Redis host |
| `REDIS_PORT` | `6379` | Redis port |
| `REDIS_PASSWORD` | `someredispassword` | Redis password |

### Frontend (`expensy_frontend/.env.production`)

| Variable | Default | Description |
|----------|---------|-------------|
| `NEXT_PUBLIC_API_URL` | `http://localhost:8706` | Backend API URL |

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/expenses` | List all expenses (cached in Redis for 5 min) |
| `POST` | `/api/expenses` | Create a new expense (clears cache) |
| `GET` | `/metrics` | Prometheus metrics endpoint |

## Tech Stack

- **Frontend**: Next.js 14, React 18, TypeScript, Tailwind CSS, Recharts
- **Backend**: Express, TypeScript, Mongoose, ioredis, prom-client
- **Database**: MongoDB
- **Cache**: Redis

## Infrastructure Best practices ( Security, Compliance linting, formatting, validataion )
Ingregate the following tools in the CI-CD pipeline before the Infrastructure is deployed. 

| Capability    | Tool                    |
|-------------- |-------------------------|
| Formatting    | terraform fmt           |
| Validation    | terraform validate      |
| Linting       | TFLint                  |
| Security scan | tfsec . /trivy config . |
| Compliance    | Checkov            |

- **tfscec vs trivy** :
tfsec may still have slightly more Terraform-specific checks in some cases and some teams keep tfsec for legacy pipelines. While tfsec can provide deeper Terraform-specific checks, Trivy offers broader multi-platform coverage, making it more suitable for unified DevSecOps pipelines. But trend is move to Trivy.


