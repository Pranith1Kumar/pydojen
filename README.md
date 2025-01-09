# pydojen


# Git, Docker, and Jenkins: A Guide

This repository provides an overview of **Git**, **Docker**, and **Jenkins**, essential tools for modern software development workflows. By leveraging these tools, developers can effectively manage source code, deploy applications, and automate software delivery processes.

---

## Git: Distributed Version Control System

**Git** is a distributed version control system that helps developers track changes in their code over time. It enables multiple developers to work on the same project simultaneously without interfering with each other’s work. Git allows you to manage your source code history, collaborate with others, and revert to previous versions if necessary. It is widely used in both open-source and commercial software development.

### Why Use Git?

- Track changes in your code.
- Collaborate seamlessly with team members.
- Revert to previous versions when needed.

### Basic Git Commands

- `git init`: Initialize a new Git repository.
- `git clone <repository-url>`: Clone an existing repository to your local machine.
- `git add <file>`: Add a file to the staging area.
- `git commit -m "message"`: Commit the staged changes with a message.
- `git push`: Push your changes to the remote repository.
- `git pull`: Fetch and merge changes from the remote repository to your local repository.

---

## Docker: Containerization Platform

**Docker** is a platform that allows developers to automate the deployment of applications inside lightweight, portable containers. Containers include everything needed to run an application, such as the code, runtime, libraries, and system tools. This ensures consistent application behavior across environments.

### Why Use Docker?

- Create consistent environments across development, testing, and production.
- Simplify deployment and improve scalability.
- Ideal for microservices architecture.

### Basic Docker Commands

- `docker build -t <image-name> .`: Build a Docker image from a Dockerfile.
- `docker run -p <host-port>:<container-port> <image-name>`: Run a container from an image and map ports.
- `docker ps`: List running containers.
- `docker stop <container-id>`: Stop a running container.
- `docker rm <container-id>`: Remove a stopped container.

---

## Jenkins: Automation Server

**Jenkins** is an open-source automation server that helps automate parts of the software development process, including building, testing, and deploying applications. It is a key tool in Continuous Integration (CI) and Continuous Deployment (CD) pipelines.

### Why Use Jenkins?

- Automate the build, test, and deployment process.
- Quickly catch issues with frequent code integration.
- Improve software quality and reliability.

### Basic Jenkins Concepts

- **Job/Project**: A task or a set of tasks that Jenkins executes.
- **Pipeline**: A series of automated steps to build, test, and deploy applications.
- **Build Trigger**: A mechanism to start a build, such as a code commit or a scheduled time.
- **Workspace**: The directory where Jenkins stores files related to a job.

Jenkins integrates seamlessly with tools like Git for version control and Docker for containerized builds.

---


By mastering **Git**, **Docker**, and **Jenkins**, you can create a robust and automated workflow for your software development projects. These tools work together to ensure your code is always in a deployable state, facilitating collaboration, deployment consistency, and continuous delivery.


Prerequities before strating this project
1. Install Docker Desktop on Windows from `https://www.docker.com/products/docker-desktop/` choose based on your OS.
- Download Docker Desktop from Docker's official site.
- Install Docker Desktop by following the on-screen instructions.
After installation, ensure Docker is running (`docker --version`).

2. Pull the Jenkins Docker Image
- Open a Command Prompt or PowerShell.
- Run the following command to pull the official Jenkins LTS image:

`bash
docker pull jenkins/jenkins:lts
`

3. Create a Docker Volume for Jenkins
- Create a Docker volume to persist Jenkins data:

`bash
docker volume create jenkins-data
`

4. Run the Jenkins Container
- Use the following command to run Jenkins:

`bash
docker run -d --name jenkins \
   -p 8080:8080 -p 50000:50000 \
   -v jenkins-data:/var/jenkins_home \
   -v /var/run/docker.sock:/var/run/docker.sock \
   jenkins/jenkins:lts
`

-p 8080:8080: Maps port 8080 of the container to port 8080 on your machine (Web UI).
-p 50000:50000: Maps port 50000 for Jenkins agents.
-v jenkins-data:/var/jenkins_home: Mounts the Docker volume for Jenkins data.
-v /var/run/docker.sock:/var/run/docker.sock: Allows Jenkins to interact with Docker.


5. Access Jenkins UI
- Open your browser and navigate to `http://localhost:8080`
- Jenkins will ask for an administrator password.

6. Retrieve the Administrator Password

- Check the directory where Jenkins is installed 
- Run the following command to get the initial admin password:
`bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
`

If using powershell use the move to the jenkins installed directory and check for secrets folder and run the command in cmd or powershell `Get-Content initialAdminPassword` .

t give the secreat password which intially created by Jenkins by default.

- Copy the password and paste it into the Jenkins UI.

7. Complete Jenkins Setup
- Install recommended plugins or customize the installation.
- Create an admin user and configure basic settings.

8. Verify Installation
- Test Jenkins by creating and running a basic pipeline or freestyle job.
- Your Jenkins setup on Windows using Docker is now complete! 🎉


# Python Web Application with CI/CD Using Jenkins and Docker

This project demonstrates how to build a simple Python web application using Flask, containerize it with Docker, and set up a CI/CD pipeline using Jenkins. The source code is managed with Git, and the entire process is automated to ensure efficient and reliable deployments.

---

## Project Overview

We will:
1. Create a Python web application with Flask.
2. Containerize the application using Docker.
3. Set up a Jenkins pipeline to automate the build and deployment process.

---

## Prerequisites

Ensure you have the following installed on your machine:
- **Python** (Version 3.9 or later)
- **Docker**
- **Jenkins**
- **Git**

Additionally, basic knowledge of Python, Docker, and Jenkins is required.

---

## Step 1: Set Up the Python Web Application

1. Create a new directory for the project and navigate into it:
   ```bash
   mkdir python-docker-jenkins
   cd python-docker-jenkins
   
2. Initialize a Git repository:

  `bash
  git init`

3. Create a virtual environment and activate it:

`bash
  python3 -m venv venv
  source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
`
4. Install Flask:

`bash
  pip install Flask
`

5. Create a Flask application in a file named app.py:

```from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Docker and Jenkins!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

6. Test the application locally:

```bash
python app.py
```
Open ```http://localhost:5000``` in your browser to see the "Hello, Docker and Jenkins!" message.

## Step 2: Dockerize the Application

1. Create a Dockerfile in the project directory:

Dockerfile
```
# Use the official Python image from the Docker Hub
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install flask

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

2. Build the Docker image:

```bash
docker build -t python-docker-jenkins .
```

Run the Docker container:

```bash
docker run -p 5000:5000 python-docker-jenkins
```
Open ```http://localhost:5000``` to see the application running in the Docker container.

## Step 3: Set Up Jenkins for CI/CD

Install Jenkins from ```https://www.jenkins.io/doc/book/installing/windows/``` or pull the Jenkinsn image from docker and run on the specific port.


- Open Jenkins and create a new Freestyle Project.

- In the "Source Code Management" section, select Git and enter your repository URL:

```bash
https://github.com/yourusername/python-docker-jenkins.git
```
replace yourusername with your user name of GitHub.
In the "Build Triggers" section, set up a trigger to poll the repository or configure webhooks.
- Add a webhook
- 


- In the "Build" section, add a build step to execute shell commands. Use the following commands to build and run the Docker container:

```bash
docker build -t python-docker-jenkins .
```
```
docker run -d -p 5000:5000 python-docker-jenkins
```
- Save the project and click Build Now to trigger the build.

## Step 4: Push Your Code to GitHub
1. Create a new repository on GitHub and copy the repository URL.

2. Add the remote repository and push your code:

bash
```
git remote add origin https://github.com/yourusername/python-docker-jenkins.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

## You have successfully:

- Built a Python web application using Flask.
- Containerized the application with Docker.
- Set up a Jenkins CI/CD pipeline for automated builds and deployments.
- This project serves as a foundation for creating more advanced CI/CD pipelines for your software projects. 🎉
