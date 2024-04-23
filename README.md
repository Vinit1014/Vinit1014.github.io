# Vinit1014.github.io

Project Name
Overview
This project is a [brief description of your project]. It contains frontend, backend, and MySQL services, each containerized using Docker. This repository provides the necessary Dockerfiles and a Docker Compose configuration (docker-compose.yaml) to run the entire application stack.

Prerequisites
Make sure you have Docker installed on your system. You can download and install Docker from here.

Getting Started
To get started with this project, follow these steps:

Clone this repository to your local machine:
bash
Copy code
git clone https://github.com/your-username/your-repository.git
Navigate to the project directory:
bash
Copy code
cd your-repository
Build the Docker images:
bash
docker pull vinits1014992/cloudia2
# Add commands to build your Docker images
Run the Docker containers using Docker Compose:
bash
Copy code
# Add command to run Docker Compose
Access the application:Once the containers are running, you can access the frontend application at http://localhost:frontend-port, the backend API at http://localhost:backend-port, and MySQL at http://localhost:mysql-port.
Dockerfile Details
Frontend Dockerfile:
Dockerfile
# Use Node.js 18 as the base image
FROM node:18 AS build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Build the application
RUN npm run build

# Start a new stage for the production image
FROM node:18 AS production

# Set the working directory in the container
WORKDIR /app

# Copy only the built artifacts from the previous stage
COPY --from=build /app/dist ./build

# Install serve to run the production build
RUN npm install -g serve

# Expose port 3000 to the outside world
EXPOSE 3000

# Command to run the production build
CMD ["serve", "-s", "build"]


# Add your frontend Dockerfile content here
Backend Dockerfile:
Dockerfile
# Use an official Node.js runtime as the base image
FROM node:latest

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the entire backend directory into the container
COPY . .

# Expose port 8000 to the outside world
EXPOSE 8000

# Command to run the backend server
CMD ["node", "index.js"]

# Add your backend Dockerfile content here
MySQL Dockerfile docker-compose.yaml:
Dockerfile
version: '3'

services:
  frontend:
    build:
      context: ./full-stack  # Path to the directory containing frontend Dockerfile
      dockerfile: Dockerfile
    ports:
      - "5173:3000"  # Map container port 80 to host port 3000
    networks:
      - mynetwork

  backend:
    build:
      context: ./server  # Path to the directory containing backend Dockerfile
      dockerfile: Dockerfile
    ports:
      - "8080:8000"  # Map container port 8000 to host port 8000
    depends_on:
      - db  # Assuming you have a MySQL database service named 'db'
    networks:
      - mynetwork

  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: "password"
      MYSQL_DATABASE: "fullstackcloud"
    ports:
      - "3306:3306"  # Map container port 3306 to host port 3306
    networks:
      - mynetwork

networks:
  mynetwork:

# Add your MySQL Dockerfile content here
Docker Images
Frontend Image: fullstack-frontend
Backend Image: fullstack-backend
MySQL Image: fullstack-mysql
Contributing
Contributions are welcome! Please feel free to submit a pull request or open an issue if you encounter any problems.
