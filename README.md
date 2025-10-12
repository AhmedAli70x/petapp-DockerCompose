# Petty - Pet Reporting System

A 3-tier web application for reporting and tracking lost pets, built with Docker, Node.js, and MySQL.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Troubleshooting](#troubleshooting)

## 🎯 Overview

Petty is a pet reporting system that allows users to:
- Report lost or found pets
- Search for pets by name, type, or description
- View all pet reports with location information
- Update and manage pet reports

## 🏗️ Architecture

This is a 3-tier architecture application:

```
┌─────────────────┐
│    Frontend     │  Node.js + Express + EJS
│   (Port 3000)   │  petty_network2
└────────┬────────┘
         │
┌────────▼────────┐
│     Backend     │  Node.js + Express + MySQL2
│   (Port 5000)   │  petty_network1 + petty_network2
└────────┬────────┘
         │
┌────────▼────────┐
│    Database     │  MySQL 8.0
│   (Port 3306)   │  petty_network1
└─────────────────┘
```

### Network Segmentation

- **petty_network1**: Database ↔ Backend communication
- **petty_network2**: Backend ↔ Frontend communication
- Frontend cannot directly access the database (security)

## 📦 Prerequisites

- Docker (version 20.10+)
- Docker Compose (version 2.0+)
- Git

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone [https://github.com/yourusername/petty-app.git](https://github.com/AhmedAli70x/petapp-DockerCompose)
cd petty-app
```

### 2. Create Environment File

Create a `.env` file in the project root:

```bash
# MySQL Database Configuration
MYSQL_ROOT_PASSWORD=rootpassword123
MYSQL_DATABASE=petty_db
MYSQL_USER=petty_user
MYSQL_PASSWORD=petty_password

# Backend Configuration
NODE_ENV=development
PORT=5000
DB_HOST=database
DB_PORT=3306
DB_NAME=petty_db
DB_USER=petty_user
DB_PASSWORD=petty_password

# Frontend Configuration
API_URL=http://localhost:5000/api
```

### 3. Build and Start Containers

```bash
# Build and start all containers
docker-compose up --build -d

# Check if containers are running
docker-compose ps
```

### 4. Access the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000/api
- **Database**: localhost:3306 (internal access only)

## 💻 Usage

### Starting the Application

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Check status
docker-compose ps
```

### Stopping the Application

```bash
# Stop all services
docker-compose down

# Stop and remove volumes (⚠️ deletes database data)
docker-compose down -v
```

### Restarting After Machine Reboot

```bash
cd /path/to/petty-app
docker-compose up -d
```

## 📚 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Endpoints

#### Get All Reports
```http
GET /api/reports
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Buddy",
      "animal": "Dog",
      "description": "Friendly golden retriever",
      "road": "123 Main St",
      "area": "Downtown",
      "city": "Springfield"
    }
  ]
}
```

#### Get Single Report
```http
GET /api/report/:id
```

#### Search Reports
```http
GET /api/search/:name
```

#### Create Report
```http
POST /api/report/
Content-Type: application/json

{
  "name": "Max",
  "animal": "Dog",
  "description": "Black labrador",
  "road": "456 Oak Ave",
  "area": "Westside",
  "city": "Springfield"
}
```

#### Update Report
```http
PUT /api/report/:id
Content-Type: application/json

{
  "name": "Max Updated",
  "description": "Black labrador with red collar"
}
```

#### Delete Report
```http
DELETE /api/report/:id
```

## 📁 Project Structure

```
petty-app/
├── docker-compose.yml          # Docker services configuration
├── .env                        # Environment variables
├── .gitignore                  # Git ignore rules
├── README.md                   # This file
│
├── backend/                    # Backend API (Node.js)
│   ├── Dockerfile
│   ├── package.json
│   ├── index.js               # Express API server
│   └── .dockerignore
│
├── frontend/                   # Frontend (Node.js + EJS)
│   ├── Dockerfile
│   ├── package.json
│   ├── app.js                 # Express frontend server
│   ├── views/                 # EJS templates
│   │   ├── index.ejs
│   │   ├── reports.ejs
│   │   ├── report.ejs
│   │   ├── reportDetails.ejs
│   │   ├── editReport.ejs
│   │   ├── quiz.ejs
│   │   └── error404.ejs
│   └── public/                # Static files
│       ├── css/
│       ├── js/
│       └── images/
│
└── database/                   # Database initialization
    └── init.sql               # MySQL schema and sample data
```

## 🔧 Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `MYSQL_ROOT_PASSWORD` | MySQL root password | rootpassword |
| `MYSQL_DATABASE` | Database name | petty_db |
| `MYSQL_USER` | MySQL user | petty_user |
| `MYSQL_PASSWORD` | MySQL password | petty_password |
| `NODE_ENV` | Node environment | development |
| `PORT` | Backend port | 5000 |
| `DB_HOST` | Database host | database |
| `API_URL` | Backend API URL | http://backend:5000/api |


