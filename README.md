# Poll Management System

## Table of Contents
- [Poll Management System](#poll-management-system)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Docker Services](#docker-services)
  - [Prerequisites](#prerequisites)
  - [Stopping \& Removing Containers](#stopping--removing-containers)
  - [Installation](#installation)
  - [Project Structure](#project-structure)
  - [Accessing the Application](#accessing-the-application)
  - [Stopping Services](#stopping-services)

## Introduction
The **Poll Management System** is a Docker-based application designed for seamless poll creation and management. It provides a fully functional environment with backend services, a frontend interface, and database management. The backend is built with Laravel, while Alpine.js for frontend interactivity, and Reverb WebSocket for real-time communication..

## Docker Services
This system consists of multiple Docker services:
- **nginx**: Manages HTTP requests and serves the application.
- **app**: Laravel-based backend for handling business logic.
- **worker**: Executes background jobs such as queue processing and WebSocket management (Reverb).
- **database**: Stores persistent data (PostgreSQL).
- **redis**: Enhances performance with caching and queue management.
- **certbot** (optional): Handles SSL certificate management for HTTPS.

## Prerequisites
Ensure that you have the following installed before proceeding:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Stopping & Removing Containers
If you have existing Docker containers that may interfere with this setup, stop and remove them with:

```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

## Installation
Follow these steps to install and configure the project:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/abhihyder/poll-man-docker.git
   cd poll-man-docker
   git clone https://github.com/abhihyder/poll-man.git app
   ```

2. **Set up Docker Compose:**
   Copy the example Docker Compose configuration:
   ```bash
   cp docker-compose.yml.example docker-compose.yml
   ```

3. **Configure environment variables:**
   Copy the `.env.example` file to `.env`:
   ```bash
   cp app/.env.example app/.env
   ```
   The `.env.example` file already contains the required environment variables for easy testing. However, the database configuration must be updated for PostgreSQL by setting the following values in the `.env` file:
   
   ```bash
   DB_CONNECTION=pgsql
   DB_HOST=poll_man_database
   DB_PORT=5432
   DB_DATABASE=poll_man_db
   DB_USERNAME=root
   DB_PASSWORD=secret
   ```

4. **Create a symlink for the `.env` file:**
   To simplify configuration management, link the root `.env` file to the `app/.env` file:
   ```bash
   ln -sf app/.env .env
   ```

5. **Set appropriate permissions for storage:**
   ```bash
   sudo chmod 777 -R app/storage/logs
   sudo chmod 777 -R app/storage/framework
   ```

6. **Build and start Docker containers:**
   Ensure the following ports are available:
   - **8000**: Laravel Web Application
   - **8080**: WebSocket Server (Reverb)
   - **5432**: PostgreSQL Database
   - **6379**: Redis
   
   Run the following command to build and start the containers:
   ```bash
   docker-compose up -d --build
   docker-compose ps
   ```

7. **Install backend dependencies:**
   Access the `app` container and install Laravel dependencies:
   ```bash
   docker-compose exec app sh
   composer install
   php artisan migrate --seed
   ```

8. **Install frontend dependencies and build assets:**
   Inside the `app` container, install Node.js dependencies and compile assets:
   ```bash
   npm install
   npm run build
   ```

## Project Structure
The repository is structured as follows:

```bash
poll-man-docker
├── .docker              # Docker-specific configurations
├── app/                 # Backend application (Laravel)
│   └── ...
├── .dockerignore        # Docker ignore rules
├── .gitignore           # Git ignore rules
├── .env                 # Environment variables
├── docker-compose.yml.example  # Example Docker Compose configuration
├── README.md            # Project documentation
```

## Accessing the Application
Once the setup is complete, open your browser and visit:

```
http://127.0.0.1:8000
```

## Stopping Services
To stop all running Docker services, run:

```bash
docker-compose down
```

This will shut down the containers and free up system resources.

