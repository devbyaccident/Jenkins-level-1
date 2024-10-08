**Setting Up a Jenkins Server with Docker Compose**
=====================================================

Welcome to the course!

Before we get started with the rest of the exercises, you'll need to setup an Jenkins enviornment using Docker Compose.

Docker Compose allows us to define and run multi-container Docker applications in a single command. 

> Note: If you're using Windows, you'll need to setup the Windows Subsystem for Linux (WSL). MacOS and Linux users can run as-is.

**Step 1: Install Docker and Docker Compose**
-------------------------------------------

Before we can proceed, you need to have Docker and Docker Compose installed on your system. You can download the installation packages from the official Docker website:

*   [Docker](https://www.docker.com/get-docker)
*   [Docker Compose](https://docs.docker.com/compose/install/)


You can also use the script below if you are running MacOS or Linux

```bash
curl -fsSL https://get.docker.com | bash
```

Once you've installed Docker and Docker Compose, verify that they are working by running the following commands in your terminal:

```
docker --version
```

**Step 2: Create a Jenkins Server Configuration File**
---------------------------------------------------

Next, take a look at the file named `docker-compose.yml` in this exercise directory. Run `cat docker-compose.yml` and it'll show you the contents of the text box below.

```yml
version: '3.8'
services:
  jenkins:
    image: jenkins/jenkins:lts-jdk17
    container_name: jenkins-jdk-17
    privileged: true
    user: root
    restart: always
    ports:
      - 8080:8080
      - 50000:50000
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/bin/docker:/usr/bin/docker

volumes:
  jenkins_home:
    name: jenkins_home
```

This configuration file:

*   Specifies the version of Docker Compose to use.
*   Defines a service called `jenkins`.
*   Uses the official Jenkins server running JDK17 (`lts-jdk17`) images from Docker Hub.
*   Allows the container to run with root access to devices on the host machine. 
**    You will need this for Jenkins to be able to build containers later on, but it's usually a good idea to avoid running contianers in privelaged mode.
*   Restarts the container if it fails.
*   Maps port 8080 on the host machine to port 8080 in the container.
*   Volumes the `/var/jenkins_home` directory within the container for persistent data and maps the host's `/var/run/docker.sock` file as a volume to enable communication between containers.
*   Makes a volume called `jenkins_home` available inside the container.

**Step 3: Start the Jenkins Server Containers**
----------------------------------------------

Now that we have our configuration file, let's start the containers by running the following command in your terminal:

```bash
cd exercises/00-Environment-Setup
docker compose up -d
```

This will:

*   Change into the directory with the **docker-compose.yml** file.
*   Create the necessary Docker network and containers defined in that file.
*   Run the containers in detached mode (`-d`).

**Step 4: Initialize the Jenkins Server**
---------------------------------------------------------

Navigate to `http://localhost:8080` in your web browser. Follow these steps:

1.  **Enter the initial admin password**: You can get this from the jenkins container by running the command below.

```bash
sudo docker exec jenkins-jdk-17 cat /var/jenkins_home/secrets/initialAdminPassword
```

2.  **Install the Reccomended Plugins**: You'll install more later, but these are a good start for now.
3.  **Create a new Admin user**: Enter your details, choose an admin password, and save.

Congratulations! You have now set up a Jenkins server running on Docker Compose. This is a basic example to get you started with the exercises locally, and you will be learning more about this setup and making changes to it throughout the course.

For now, go ahead and let the instructor know you're done.
