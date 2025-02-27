
# Todo App Docker with Jenkins

This project demonstrates how to deploy a simple Todo application using Docker and Jenkins for continuous integration and deployment (CI/CD). The app is containerized using Docker, and Docker Compose is used to manage multi-container setups.

## Features

- Dockerized Python Django application
- Easy deployment with Docker Compose
- Supports Dockerfile-based deployment for any platform
- Jenkins pipeline for automated deployment (not included in this README)

## Prerequisites

Ensure that you have the following installed:

- Docker
- Docker Compose
- Git (for cloning the repository)
- Jenkins (for automated deployment, if required)

## Getting Started

### 1. Clone the Repository

First, clone the repository to your local machine:

```bash
git clone https://github.com/Aadi-Sonwane/todo_app_docker_jenkins.git

```
```bash
cd todo_app_docker_jenkins
```


# 2. Dockerfile
The project uses a Dockerfile to containerize the application.

Dockerfile:

```bash
# Use the official Python image as the base image
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy requirements.txt file into the container
COPY requirements.txt /app/

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the entire project into the container
COPY . /app/

# Expose the port the app runs on
EXPOSE 8000

# Run the application on the specified port
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

```


# 3. Docker Compose
Docker Compose is used to manage multiple containers and run them together. In this project, you can use Docker Compose to run both the Django application and any required services (like MySQL, if you use it).
docker-compose.yml:
[docker-compose.yml](/todo_app_docker_jenkins/docker-compose.yml)
```bash [docker]
version: "3.8"

services:
  django_app:
    build:
      context: .
    image: toda_app_compose
    container_name: toda_app_dj
    ports:
      - "8000:8000"
    environment:
      - PYTHONUNBUFFERED=1
    volumes:
      - .:/app
    networks:
      - toda_app_network
    restart: always

  db:
    image: mysql:5.7
    container_name: db_cont
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_DATABASE=toda_app_db
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - toda_app_network
    restart: always

networks:
  toda_app_network:
    driver: bridge

volumes:
  db_data:

```
# 4. Build and Run with Docker Compose
To build and run the application using Docker Compose, follow these steps:
1. Build and start the containers:
```bash
docker-compose up -d --build
```

2. Check the status of the running containers:
```bash
docker compose ps 
```
3. Access the Django application:
```bash
- Visit http://localhost:8000/ in your browser.
```
4. If you need to stop the containers:
```bash
docker-compose down
```


#5. Running the Application Using Dockerfile (Without Docker Compose)
If you don't want to use Docker Compose, you can directly use the Dockerfile to build and run the application.
1. Build the Docker image:
```bash
docker build -t toda_app .
```
2. Run the Docker container:
```bash
docker run -d -p 8000:8000 --name toda_app_container toda_app
```

3.Access the Django application:
- Visit http://localhost:8000/ in your browser.

4. To stop the container
   ```bash
    docker stop toda_app_container
   ```

# 6. Environment Variables (Optional)
- You can configure the app using environment variables. Here are a few commonly used ones:
```json
DATABASE_URL: The URL for connecting to the database. 
Example: DATABASE_URL=mysql://root:rootpassword@db/toda_app_db
```
- Make sure to set the necessary environment variables in the docker-compose.yml file or in a .env file.

# Troubleshooting
DisallowedHost Error: 
If you encounter an error like Invalid HTTP_HOST header, make sure to update **ALLOWED_HOSTS** in settings.py with the appropriate host or IP address 
_(e.g., ['localhost', '0.0.0.0', '127.0.0.1', 'your-server-ip'])._
