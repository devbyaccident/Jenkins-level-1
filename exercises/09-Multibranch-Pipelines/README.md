**Creating a Multi-Branch Pipeline Project**
=====================================================



**Step 1: Setup a New Multi-Branch Pipeline Project**
-------------------------------------------

From the Jenkins menu, make a new project. Name it `Multi-Branch` and select the `Multi-Branch Pipeline` type, then click **OK**.

From the configuration menu of the new project, look under **Branch Sources** and click the **Add source** dropdown to add a new GitHub branch source.

> Note: If you don't see GitHub, make sure the GitHub Branch Sources plugin is installed.

On the new Branch Source, fill in the below prompts:

- Credentials: Use the GitHub credentials created earlier
- Repository HTTPS URL: Use the URL of your Demo repository. Click **Validate** to make sure those credentials are authorized to see it.

Leave the rest of the prompts as they are and click **Save**. 

Once you do, Jenkins will start scanning each branch of your repository for a file at the root named `Jenkinsfile`. 

Currently there should only be one `Jenkinsfile` on your `dev` branch, which means it will only run there.

But, there's a bug in this Jenkinsfile. It builds every minute!

Let's fix it by opening a pull request, validating the changes, then merging it into the `dev` branch.

**Step 2: Setup a New Branch**
-------------------------------------------

Open the demo repo in your IDE of choice and make sure you are currently on the `dev` branch.

You can make a new branch with the name `bugfix/jenkinsfile` from the current `dev` branch by running the command below:

```bash
git checkout -b bugfix/jenkinsfile
```

On the new branch, open the Jenkinsfile and remove the following lines:

```
triggers {
    pollSCM '* * * * *'
}
```
Multi-branch pipelines don't have a `pollSCM` trigger like single-branch pipelines do, so Jenkins is interpreting this as a cron expression that builds every minute!

Run the commands below to stage the changes to the Jenkinsfile, commit them to the branch, then push the new branch to the remote repository:

```bash
git add Jenkinsfile
git commit -m "Removing build triggers"
git push --set-upstream dev bugfix/jenkinsfile
```

Now back on the Jenkins page, go to the Multi-branch pipeline project and click **Scan Repository Now** to have Jenkins look for other branches in that repository.

You should see another branch appear called `bugfix/jenkinsfile` and start building. After it's built once, wait another minute to see if it automatically triggers again. If it doesn't, you have fixed the bug, and are ready to move on to Step 3.

**Step 3: Open a Pull Request**
-------------------------------------------

Since the bug is fixed, it only makes sense to merge it back to our `dev` branch. Go to your Demo repo in the GitHub UI and navigate to the **Pull requests** tab at the top.

Here, use the drop-down menus to compare the `bugfix/jenkinsfile` branch to the base `dev` branch. Don't forget to fill in any details that you would want a reviewer to know while reviewing your code changes. 

Once you have, click **Create pull request**, then go back to Jenkins and click **Scan Repository Now** from the multi-branch pipeline project again. 

You should see the `bugfix/jenkinsfile` branch dissapear and a new `PR-1` job appear under the **Pull Requests** tab at the top. When that builds successfully, go back to your Pull Request in GitHub.

Above the green **Merge pull request** button, there will be a green checkmark that says **All checks have passed** and a link for **Show all checks**. Click to show all checks, and you should see two sucessful builds from Jenkins, one for the build on the branch and the other for the build on the pull request. You can click the **Details** link to be taken back to either job.

In an actual environment, you can make these checks required to pass before being allowed to merge changes. Go ahead and merge the change and delete the branch. You should see it removed from Jenkins when you go back and re-scan the repository.

Once you're finished, let the instructor know you are done.