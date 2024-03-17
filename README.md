Deploying a two-tier application with Docker-compose involves defining a YAML file to specify the services, networks, and volumes for a multi-container application. It simplifies the deployment of front-end and back-end components, enabling efficient scaling and orchestration of containers. Docker-compose streamlines the setup, configuration, and interconnection of these components, making it easier to manage and run the application.

Update the System & Install Docker.
sudo apt-get update # Update the System
sudp apt-get install docker.io # Install Docker

Add the current user to docker group
sudo usermod -aG docker $USER

Make Dockerfile.
```bash
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# install required packages for system
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Copy the requirements file into the container
COPY requirements.txt .

# Install app dependencies
RUN pip install mysqlclient
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Specify the command to run your application
CMD ["python", "app.py"]

```


Clone the source-code repository url from GitHub. 
https://github.com/CloudOpsRahul/Docker-Project.git

Create a Docker image  from Dockerfile 
docker build -t flaskapp:latest .

Following is the command to run the multiple containers by docker-compose.
docker-compose up -d

Then we can check if application is running or not by public IP of EC2 instance followed by port number.
