
# Docker Advanced Project

A brief description of what this project does and who it's for

## Introduction:
This documentation outlines the usage and purpose of the Dockerfile Multistage project.

## Project Overview:
The Dockerfile Multistage project aims to streamline the containerization process by leveraging Docker's multistage build functionality. This approach enables the creation of lightweight and efficient Docker images by incorporating multiple stages within a single Dockerfile.

## Usage

### 1. Clone the Repository:

`git clone https://github.com/harshm1225/reactjsdemo`


### 2. Change directory to the ReactJS demo:

`cd reactjsdemo`

### 3. Build the image

`docker build -t <image_name> -f Dockerfile-multistage .`

### 4. Run the Docker Container:

`docker run -d --name <container_name> -p <host_port>:80 <image_name>`

### 5. Access the application

Once the Docker container is running, you can access the application by opening a web browser and navigating to:

`http://<public_ip>:<host_port>`

Replace <public_ip> with the public IP address of your host machine and <host_port> with the port number specified when running the Docker container.

For example, if your public IP address is 123.456.789.0 and you specified port 8080 as the host port, you would visit:

`http://123.456.789.0:8080`

This will direct you to the application running inside the Docker container on your host machine.

