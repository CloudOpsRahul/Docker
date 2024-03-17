Deploying a two-tier application with Docker-compose involves defining a YAML file to specify the services, networks, and volumes for a multi-container application. It simplifies the deployment of front-end and back-end components, enabling efficient scaling and orchestration of containers. Docker-compose streamlines the setup, configuration, and interconnection of these components, making it easier to manage and run the application.

Clone the source-code repository url from GitHub. 
https://github.com/CloudOpsRahul/Docker-Project.git

Create a Docker image  from Dockerfile 
docker build -t flaskapp:latest .

Following is the command to run the multiple containers by docker-compose.
docker-compose up -d

Then we can check if application is running or not by public IP of EC2 instance followed by port number
