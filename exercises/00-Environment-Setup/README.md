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

Next, you'll need to make a Jenkins test environment. This will let you go through the course materials without making permenant changes to your local machine or production environment. 

To do that, we're going to use Docker Compose. In this folder, you should see all of the services required for this course in a configuration file called **docker-compose.yml**.

This configuration file:

- Defines services for Jenkins, SonarQube, and a Jenkins agent. Don't worry about what these are yet, you'll learn more abou them as you go through the course.
- Creates a shared network so these services can connect to each other. 
- Makes a volume called `jenkins_home` available inside the container.

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
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

2.  **Install the Reccomended Plugins**: You'll install more later, but these are a good start for now.
3.  **Create a new Admin user**: Enter your details, choose an admin password, and save.
4. **Set your Hostname**
Since we're just running this as a lab environment, we can leave this as `http://localhost:8080`, but in a production environment, this would be the URL which you use to access Jenkins. For now, click **Save and Finish**

Congratulations! You have now set up a Jenkins server running on Docker Compose. This is a basic example to get you started with the exercises locally, and you will be learning more about this setup and making changes to it throughout the course.

To shut these down at anytime, run `docker compose down` from this directory. 

For now, go ahead and let the instructor know you're done.
