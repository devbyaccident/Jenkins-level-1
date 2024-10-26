**Code Quality and Coverage**
=====================================================

Unit tests are important for ensuring the functionallity of an application doesn't break with new changes. 

Code coverage and quality tools can help ensure that your tests are testing each line and class, as well as make sure that your code adhears to the best practices for the language you are using.

In this exercise, you'll be setting up a code quality tool called SonarQube, and integrating it with Jenkins. 

**Step 1: Login to SonarQube and Create a Project**
-------------------------------------------

Surprise, you're already running SonarQube as part of your course lab environment!

The same way that your Jenkins Controller is running on http://localhost:8080, SonarQube is running at http://localhost:9000.

Go ahead and open it up, then login with the below credentials:

- Username: admin
- Password: admin

The first time you login, you'll be prompted to set a new password, and then you'll see the SonarQube UI. 

Familiarize yourself with the capabilities of the tool. Click on the **Rules** tab at the top of the page. You'll see a list of rules for each language that SonarQube supports, as well as explainations and remediations for each. 

Next, from the main menu, create a new project manually. Fill in the fields with these values:

- Project display name: DemoApp
- Project key: DemoApp
- Main branch name: main

Next, click the button at the bottom of the page to **Analyze your project locally**, and generate a token using the default settings. Save that token for later, and move on to the next step.

**Step 2: Configure SonarQube in Jenkins**
-------------------------------------------

Right now, even though they are in the same docker network, and even though the SonarQube plugin is installed on Jenkins, the Jenkins controller has no idea that SonarQube exists. Before you can use it in Jenkins, you'll have to add it to the Jenkins settings.

While keeping SonarQube in one tab, open a new one and navigate to Jenkins.

From your Jenkins console, navigate to **Manage Jankins** > **System**, then look for the section titled **SonarQube installations** and click on **Add SonarQube**.

Use the following values to add the SonarQube docker interface to Jenkins:

- Name: SonarQube
- Server URL: http://sonarqube:9000
- Server authentication token: You'll need to make a new Secret Key credential here, similar to how you did in exercise 3. Use the **+ Add** button to create a new credential using the values below.
  - Kind: Secret Text
  - Secret: Enter the project token from SonarQube
  - ID: `sonarqube_demoapp`

Once SonarQube is added, save the Jenkins configuration.

> Note: In a production setting, you wouldn't be using a project-specific SonarQube key in a global server setting. Please keep in mind, this is a lab for demonstration purposes!

Finally, go to **Manage Jenkins** > **Tools**, and look for **SonarQube Scanner installations**. Add a new SonarQube Scanner called `SonarQube Scanner`, and click **Save**

> If you don't see this, go back and check to make sure the **SonarQube Scanner for Jenkins** plugin is installed!

**Step 3: Create a Jenkins Project with SonarQube**
-------------------------------------------

I'm sure you are thinking of all the code you want to check using this tool, but there's already a repository out there for this exercise. 

Navigate to https://github.com/devbyaccident/sonarqube-demo-app, and fork the repository to your account.

You should see a very small C# codebase with a unit test and a SonarQube properties file. Once it's forked, go to Jenkins and create a new Freestyle Project with the following properties:

- Name: SonarQubeDemo
- Source Code Management: Git
- Repository URL: The HTTPS URL of your fork of https://github.com/devbyaccident/sonarqube-demo-app
- Credentials: The Username/Password credentials we setup in exercise 3
- Branch Specifier: `main`

The rest of the build environment and build triggers can be left as-is. However, under **Build Steps**, click on the drop-down menu under **Add build step** and add **Execute SonarQube Scanner**.

Leave all parameters for the scanner blank, then save and build the job. Once it is complete, you should be able to switch back to the SonarQube tab and see the report pass quality check.

> Normally, there is a link directly from Jenkins to SonarQube, but because of the network constraints of the lab environment, that won't work in this exercise.

Once you're ready let the instructor know you're done with this exercise.