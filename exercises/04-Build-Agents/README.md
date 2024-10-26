**Creating a Jenkins Agent**
=====================================================

Build agents are how you'll be running most of your Jenkins jobs. Not only is it considered a [best practice](https://www.jenkins.io/doc/book/using/best-practices/#build-on-agents), but it means you can also tightly control the build environment your jobs run on without installing anything on your Jenkins controller.

In a production Jenkins instance, all of your jobs will run on agents insetad of the controller, but since this is a lab environment, you'll only be using them in this exercise for this course.

In this exercise, you'll connect an agent to the Jenkins controller, label the agent, then run a job that uses that agent.

**Step 1: Create the Node Agent**
-------------------------------------------

First, you need to tell the Jenkins controller to expect to use a Jenkins agent running on a Node. Nodes can be other physical computers, virtual machines, Kubernetes pods, or even docker containers, as long as they are on the same network as the controller. 

You can setup a new node agent by navigating to **Manage Jenkins** > **Nodes** and clicking on **New Node**.

Give it the name `docker-agent` and select the radio button to make it a **Permenant Agent**, then click **Create**

On the next window, fill out the following parameters and leave the rest as they are:

- **Remote root directory**: `/var/jenkins`
- **Labels**: `agent`

Click **Save, then from the nodes window, click on the new node. That should show you the commands to connect it to the controller via JNLP once you have another node running on the same network. 

Save the commands to connect a Unix agent and proceed to the next step.

**Step 2: Connect the Agent to the Controller**
-------------------------------------------

Surprise, you've already got a Jenkins node running in your docker environment! 

Agents are required to have network connectivity to the controller, so for a lab environment like this, the agent has to run in a docker container on the same network.

> Note: Some of the commands in this exercise will change `localhost` to the name of a container in the same network. You ideally won't have to do that in a real Jenkins environment that has been configured correctly, but it's unfortunatley unavoidable in a lab such as this. Please let the instructor know if you need any help with this!

Start by opening a shell on the agent using the command below:

`docker exec -it agent /bin/bash`

You can get the docker container ID by running `hostname` from this shell window. Save that for later, you'll need it in a later step.

Pull up the commands to connect a Unix agent from the end of step 1, and replace all instances of `localhost` with `jenkins`. This is because that is the container name of the jenkins controller defined in the docker compose file, and the agent will be able to find it on the docker network.

You'll also need to replace `curl -s0` with `wget` because curl is not included on these agent container images.

It should look something like this when you're done:

```bash
wget http://jenkins:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://jenkins:8080/ -secret <NotMyRealSecret> -name "docker-agent" -workDir "/var/jenkins"
```

If it's run successfully, you'll see **INFO: Connected** in your terminal. Leave that terminal open and go back to the Jenkins console. The new node should show as connected.

**Step 3: Run a Job on the Agent**
-------------------------------------------

Now you're ready to connect to the agent and run a job from it. Go to your freestyle job and check the box next to **Restrict where this project can be run**. Use the text box to enter the same label you gave the node agent in step 2, `agent`.

Before building the job, go down to the **Execute shell** build step and add `hostname` at the end. It should look like this after:
```bash
echo "Hello World!"
echo "This is build number $BUILD_NUMBER running from git branch $BRANCH_NAME in folder $WORKSPACE"
ls -altr
hostname
```

Save the changes and click on **Build Now**. Open the console output for that run and you should see the hostname of the jenkins agent you saved in step 2.

You can do the same in your pipeline job by opening your `Jenkinsfile` in a code editor and replacing the line `agent any` with the block below, then checking in the changes:
```
agent {
  label 'agent'
}
```

Once you're done, exit the terminal and remove the agent, then let the instructor know you're ready to continue. 