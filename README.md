# Ayush Terminology Bridge - Local Development Environment

Welcome to the **Ayush Terminology Bridge** project! This guide will help you set up the complete local development environment on your machine. The goal is to run all required services such as databases and servers using **Docker**, making it easy and hassle-free.

## 📦 What is Docker?

Docker packages applications and their dependencies into lightweight, self-contained "boxes" called **containers**. For example, instead of installing PostgreSQL or Redis directly on your system, Docker allows you to run them inside containers with all necessary configurations.

We use a file called `docker-compose.yml` as a recipe to define which containers are needed and how they should communicate.

This means you only need Docker installed — everything else is handled for you!

---

## ✅ Prerequisites

Before setting up the environment, make sure you have the following installed:

- **[Git](https://git-scm.com/downloads)** — For cloning the project repository.
- **[Node.js](https://nodejs.org/en/download/)** — Required for running backend services.
- **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** — Includes Docker and Docker Compose for managing containers.

---

## 🚀 Environment Setup: Step-by-Step

### Step 1 – Clone the Repository

```bash
git clone https://github.com/Zynergy-SIH-2025/ayush-bridge-v1.git
cd ayush-bridge-v1
``` 

### Step 2: Configure Environment Variables
This is a crucial step for keeping sensitive information like passwords secure. We use an "environment file" to store this data so it's not written directly in our code.

In the project folder, you will find a file named .env.example. This is a template.

Create a copy of this file and name it .env. You can do this with the following command:

```bash
cp .env.example .env
```

Now, open the new .env file in a text editor. It will look like this:

```bash
# PostgreSQL Database Credentials
POSTGRES_USER=ayushuser
POSTGRES_PASSWORD=ayushpass
POSTGRES_DB=ayushdb
```

For local development, you can leave these default values as they are. This file tells our Docker setup what username, password, and database name to use when creating the PostgreSQL database container.

Important: The .gitignore file is configured to ignore the .env file, so you will never accidentally commit your secrets to a public repository.

### Step 3: Launch the Services with Docker Compose
Now for the magic. With Docker Desktop running, use your terminal to run one single command from the project's root folder:

```bash
docker-compose up -d
```

**docker-compose up**: This command reads the docker-compose.yml file and starts all the services defined inside it. Docker will download the necessary "images" (like the blueprints for PostgreSQL and Redis) if you don't have them already.

**-d**: This "detached" flag runs the containers in the background, so you can continue to use your terminal.

### Step 4: Verify the Services are Running
To check that everything is working correctly, run the following command:

```bash
docker-compose ps
```

You should see a list of the running services, and their State should be Up. It will look something like this:

```bash
      Name                     Command               State           Ports
-----------------------------------------------------------------------------------
ayush_postgres        docker-entrypoint.sh postgres    Up      0.0.0.0:5432->5432/tcp
ayush_redis           docker-entrypoint.sh redis ...   Up      0.0.0.0:6379->6379/tcp
hapi_fhir_server      /usr/bin/tini -- /usr/bi ...     Up      0.0.0.0:8080->8080/tcp
icd11_service         "/docker-entrypoint.sh ng ..."   Up      0.0.0.0:8083->80/tcp
```

***Congratulations!*** The complete background environment for the Ayush Terminology Bridge is now running on your machine.

## ⏹️ How to Stop the Environment
When you are finished working, you can stop all the services. Run this command from the project folder:
```bash
docker-compose down
```

This will gracefully stop and remove the containers. Your data in the PostgreSQL database will be saved in a "volume," so it will still be there the next time you run docker-compose up -d.