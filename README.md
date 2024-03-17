Deploying a two-tier application with Docker-compose involves defining a YAML file to specify the services, networks, and volumes for a multi-container application. It simplifies the deployment of front-end and back-end components, enabling efficient scaling and orchestration of containers. Docker-compose streamlines the setup, configuration, and interconnection of these components, making it easier to manage and run the application.

##Update the System & Install Docker.
```bash
sudo apt-get update # Update the System
sudp apt-get install docker.io # Install Docker
```

##Add the current user to docker group
```bash
sudo usermod -aG docker $USER
```

##Clone the source-code repository url from GitHub. 
```bash
https://github.com/CloudOpsRahul/Docker-Project.git
```

##Make Dockerfile.
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
##Create a Docker image  from Dockerfile 
```bash
docker build -t flaskapp:latest .
```
##Now, go to your EC2 instance security group open port no. 5000 & save it.

##If you want to run both containers in one command - flaskapp, mysql.
##Install docker-compose.
```bash
sudo apt-get install docker-compose -y
```
##Make docker-compose.yml file.
```bash
version: '3'
services:

  backend:
    build:
      context: .
    ports:
      - "5000:5000"
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
      MYSQL_DB: myDb
    depends_on:
      - mysql

  mysql:
    image: mysql:5.7
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: myDb
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    volumes:
      - ./message.sql:/docker-entrypoint-initdb.d/message.sql   # Mount sql script into container's /docker-entrypoint-initdb.d directory to get table automatically created
      - mysql-data:/var/lib/mysql  # Mount the volume for MySQL data storage

volumes:
  mysql-data:
```
Now run docker-compose command. It will create network automatically.
```bash
docker-compose up -d  # it creates network automatically.
docker-compose down   # it down the access
```

##Finally, access your flask app -> publicip:5000
