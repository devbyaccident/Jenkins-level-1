**Polling SCM for Changes**
=====================================================

Now that you have your build steps, it's time to learn how to automate running them!

Like most CI build systems, Jenkins is intended to be used to trigger builds when new changes are found in Source Code Management (SCM).

In this exercise, you'll setup a Git repository and poll it for changes. If any are made, it will trigger the jobs you made in the last exercise.

**Step 1: Setup a Git Repository**
-------------------------------------------

First, you'll need to have a Git repository to make changes to. You're free to make one of your own, or fork the [Demo Repo from my account](https://github.com/devbyaccident/Demo_Repo).

> Note: If you aren't comfortable using Git, please let the instructor know so they can walk through the steps required.

**Step 2: Setup a Github Access Token**
-------------------------------------------

Unless you only want to poll public repositories, and only under Github's rate limit, you're going to need to authenticate your git calls. There are many ways to do this, but for demo purposes, you'll be setting up an simple one today that doesn't require installing an application in Github.

While logged into your Github account, go to https://github.com/settings/tokens.

From here, navigate to **Personal access tokens (classic)** and create a new token. The token should have the scopes below:

- *admin:repo_hook* - for managing hooks (read, write and delete old ones)
- *repo* - to see private repos
- *repo:status* - to manipulate commit statuses

Once the token is created, copy it to notepad for use in the next step.

**Step 3: Setup a Jenkins Credential**
-------------------------------------------

Jenkins has it's own credential store for secrets that need to be used in build pipelines. To add it to Jenkins, go to the Jenkins dashboard, then click on **Manage Jenkins** > **Security** > **Credentials**

On the credentials page, you can see there are Credential Domains that you can use to scope where credentials can be used, or add new credentials to the Global scope. 

To add a new credential, hover over the label `(Global)` under **Stores scoped to Jenkins**, then click the dropdown box that appears and select **Add credentials**

You can see there's several different kinds of credentials you can store. For this one, you can keep **Username and password** as the kind you'll use for this exercise, and fill out the rest of the fields with the values below:

- Username: Your Github username
- Password: The Github access token you created in step 2
- ID: `Github-credentials`

Once that's done, click **Create**

**Step 4: Poll Github from a Freestyle Job**
-------------------------------------------

Now that the credentials are setup, it's time to use them. Open up your **Hello_World_Project** and open the configure page.

Under the **Source Code Management** section, choose **Git**, then fill out the rest of the fields with the values below:

- Repository URL: The HTTPS URL of your git repository from step 1
- Credentials: Use the drop-down menu to choose the credentials you created in step 3
- Branch Specifier: Remove the branch name from this so it will pickup changes from every branch.
- Under **Build Triggers**, check the box next to **Poll SCM**, then enter `* * * * *` in the corresponding text box. This is a [cron](https://en.wikipedia.org/wiki/Cron) expression that will check the repository for a change every minute.

Once these are filled out, click **Save**.

**Step 5: Poll Github from a Pipeline Job**
-------------------------------------------

Pipeline jobs are a little stricter in terms of what they'll allow configuration-wise. Instead of running the pipeline script from Jenkins as you've been doing, you're going to check it into Git now.

> Note: Jenkins refers to Git as an SCM, which stands for **Source Code Management**. Any code versioned in Github uses the Git SCM, but other source code management tools like Bitbucket and Gitlab use it too! 

Go to the pipeline job in your Jenkins console and open the **Configure** page. From here, check the box next to **Poll SCM**, then enter `* * * * *` in the corresponding text box.

Further down in the **Pipeline** section, change `Pipeline Script` to `Pipeline Script from SCM`, then fill out the parameters below.

- Repository URL: The HTTPS URL of your git repository from step 1
- Credentials: Use the drop-down menu to choose the credentials you created in step 3
- Branch Specifier: `dev`

Once these are filled out, click **Save**.

> Pipeline jobs can't run on each branch, only one at a time. If you need to run on more than one branch at a time, you need a Multi-Branch pipeline job, which we'll be covering later in the course.

**Step 6: Checkin the Jenkinsfile**
-------------------------------------------

Take the file named `Jenkinsfile` from this folder and copy it to your local copy of the Github repository you are polling in the Freestyle Job. This is the same one you made in step 1.

Then from your git repository, run these commands:
- `git checkout -b dev`
- `git add Jenkinsfile`
- `git commit -m "Adding Jenkinsfile"`
- `git push --set-upstream origin dev`

Those commands do the following:

- Create a new branch called `dev`
- Stage the new Jenkinsfile
- Commit the Jenkinsfile to the new `dev` branch
- Push the dev branch with the new commit to the remote repository

Once you push the branch, buth the Hello World Freestyle job and the Pipeline job should both trigger!

Let the instructor know you're done and we'll do that in the next exercise.