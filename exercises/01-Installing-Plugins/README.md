**Installing Jenkins Plugins**
=====================================================

Jenkins is an incredibly flexible and versitile build server. Out of the box, it can integrate with git for version control, Bash/cmd.exe for scripting, and SMTP for email. For anything else, you'll need to download and install a plugin.

This exercise is about finding a plugin from the Jenkins plugin repository and installing it. You'll be using it in the next exercise.

> Sometimes, when running Jenkins in Docker, installing plugins can cause the container to crash. This is a course demo, so if necessary just restart the container with `docker compose up -d` from the [exercise 00 folder](../00-Environment-Setup/).

**Step 1: Find the Chuck Norris Plugin**
-------------------------------------------

Chuck Norris breaks bones, not builds. All kidding aside, there is a pretty fun plugin for Jenkins that will show a picture of Chuck Norris when invoked from a job. 

First, let's find the page for it on the [Jenkins Plugin Index](https://plugins.jenkins.io/).

You can find it at https://plugins.jenkins.io/.

Click on the **Browse** Button to be taken to a list of Jenkins plugins and information on their health, maintainers, and other details on each plugin.

Use the text bar labeled **Find plugins** to find the Chuck Norris plugin, and click on the card for it.

https://plugins.jenkins.io/chucknorris/

Here you can see the last release of the plugin, the source code in version control, and usage instructions.

There are several ways to install the plugin, which you can see by going to the top-right corner and clicking on the **How to install** button.

**Step 2: Install the Plugin**
-------------------------------------------

For this demo, you'll be using the Jenkins UI to install the plugin. Login to your Jenkins dashboard and navigate to:

**Manage Jenkins** -> **Plugins** -> **Available Plugins**

These are the plugins that are listed on the Plugin Index, minus the ones that are already installed on your Jenkins instance. 

Search for **Chuck Norris** and select it by clicking on the check box on the left of the plugin, then click the install button in the top right. 

Jenkins will take you to the plugin installation page where you can see the installation status.

**Step 3: Restart Jenkins**
-------------------------------------------

Once the plugin is installed, you'll need to restart Jenkins before you can use it. Usually there will be builds running that shouldn't be interrupted, but since this installation is new, we can restart it anytime.

Navigate to http://localhost:8080/restart and click **Yes**, or check the box labeled **Restart Jenkins when installation is complete and no jobs are running** from the plugin installation page.

Once Jenkins restarts, you'll be back to the sign in screen and you'll be able to use the new plugin.

**Step 4: Install Other Course-Required Plugins**
-------------------------------------------

Now that you know how to install plugins, here's some of the other ones we're going to use during the course. Make sure these are installed before finishing the exercise.

- **SonarQube Scanner for Jenkins** 
- **GitHub Integration**
- **GitHub Branch Source**

Let the instructor know you're done and we'll do that in the next exercise.