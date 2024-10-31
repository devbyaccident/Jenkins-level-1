**Creating a Freestyle Project**
=====================================================

Now that you've got a new Jenkins server setup and some plugins installed, it's time to take it for a test drive. 

Let's pretend that you've got a git repository, and you want to trigger a workflow each time a new change is checked into the `main` branch. 

In this exercise, you'll setup some build steps and run them without attaching them to a git repository. (We'll add that next exercise.)

**Step 1: Setup a Freestyle Project**
-------------------------------------------

Navigate to the Jenkins console and click on **New Item** from the menu on the top left. You'll see a list of project types that Jenkins can create, each with a different purpose. For this exercise, we'll use the **Freestyle Project** type.

Select **Freestyle Project** and name it `Hello_World_Project`, then click **Ok**. From here, you'll be taken to the job configuration page.

Freestyle jobs have a few different sections, each with a different purpose. For this exercise, you only need to be concerned with **Build Steps** and **Post-build Actions**.

Add a new build step using the dropdown menu under **Build Steps**, and choose *Execute shell* from the list.

**Step 2: Add Build Steps**
-------------------------------------------

Since this Jenkins server where this job will run is running in a linux docker container, we need to use this step to run bash commands from the command line. Enter the following in the Command textbox:

```bash
echo "Hello World!"
echo "This is build number $BUILD_NUMBER running from git branch $BRANCH_NAME in folder $WORKSPACE"
ls -altr
```
> Note: This shell script has a problem that we'll come back to later.

When Jenkins runs the project, it'll put these commands into a script, make a new Workspace directory on the instance, and run the script in that workspace. 

The actions in a build step vary, but can include anything from but not limited to:

* Building/Compiling Code
* Unit testing
* Calling a REST API
* Anything you can do from the command line

Shell scripts are most commonly used here, but there are many plugins that can add additional features and steps here. For now, these two lines are enough, and we can proceed to post-build actions

**Step 3: Add Post-build Actions**
-------------------------------------------

Once a project has finished building, it's usually helpful to report the status of that project somewhere. How exactly this looks will depend on your organization's communications tools and style, but there are plugins to support a wide variety of tools.

Go under the **Post-build Actions** dropdown menu and look at the available options. Some of the things you'll typically see in post-build actions include:

* Sending a notification email/slack/teams message with the build status
* Aggrigating unit test results and publish a report
* Publishing a git release/tag

As you have not configured any communication tools for this instance, we can give our build the only type of validation that anyone should need outside of production: A thumbs up from Chuck Norris.

Go ahead and select **Activate Chuck Norris** from the dropdown menu, then click **Save**.

**Step 4: Run the Freestyle Project**
-------------------------------------------

With the freshly-made project in place, click on **Build Now**, wait a few seconds, then refresh the page.

You should see a green checkmark on the left-hand side with the number `1` next to it. Clicking on the number will show you some information about that specific run of the project, including:

* Who or what started the job
* How long it took to run and when
* Unit test reports (Or Chuck Norris)

To go a step further, click on **Console Output** on the left side of the screen. Here you'll see each each line that was run and the output from each line.

> You remember when I said the shell commands had a problem? Can you see what that problem is and why it happened?

**Step 5: Run it as a Pipeline**
-------------------------------------------

Pipelines are going to be the main way you define jobs in Jenkins. They're defined in a Groovylang DSL, and can be checked in with code, meaning that build instructions are versioned alongside the application.

There's a lot of tools built-in to a Jenkins installation, but to start out, I'll give you the pipeline script equivalent to the freestyle job you just ran. Open up the Jenkins menu, click on **New Item** and make a new pipeline.

Instead of choosing the **Freestyle Project** type, this time choose the **Pipeline** type and name it `Hello_World_Pipeline`, then click **Ok**.

Once you're at the job configuration page, scroll down to the bottom. Under **Pipeline**, you'll see a text box for a pipeline script. Copy and paste the code below into that box, then click **Save**.

```
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo "Hello World!"
                echo "This is build number $BUILD_NUMBER running in folder $WORKSPACE"
                sh("ls -altr")
            }
        }
    }
    post {
      always {
        chuckNorris()
      }
    }
}
```

Once you build the project and check the command output, you should see similar output to the freestyle job.

**Wrap Up**
-------------------------------------------

If you finished this exercise early, you can play around with doing other things in the freestyle or pipeline jobs like:

* Downloading a package from the intenet and listing it in the Jenkins Workspace
* Explore the Pipeline Syntax generator from the Pipeline job configuration
* Take a look at the [Pipeline Steps Reference](https://www.jenkins.io/doc/pipeline/steps/)

Once you're done trying those, or if you'd like to skip, let the instructor know you're done.