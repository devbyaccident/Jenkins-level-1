**Setting and Referencing Environment Variables**
=====================================================

Just about anything you want to use is an environment variable in Jenkins. You've seen and used a few already, but in this exercise you'll find where to see all of the ones available to you, as well as set a few of your own.

**Step 1: View the Default System Environment Variables**
-------------------------------------------

First, let's take a look at what Jenkins provides by default. 

Navigate to **Manage Jenkins** > **System Information** > **Environment Variables**.

Here you can see the environment variables Jenkins sets by default, which are hidden by default. Reveal `JAVA_HOME` and `JENKINS_HOME` to see the paths and take note of them.

**Step 2: Set Global System Environment Variables**
-------------------------------------------

You can also set your own environment variables at the system level in Jenkins that will be accessible to every project managed by that Jenkins controller.

Go to **Manage Jenkins** > **System**, and scroll down to **Global Properties**.

Click on the button next to **Environment variables**, then click the **Add** button and set two variables with the following names and values:

1.
- Name: `NAME`
- Value: `Sir Jenkins`

2.
- Name: `PURPOSE`
- Value: `to seek the holy grail`

**Step 3: View Default Project-Specific Environment Variables**
-------------------------------------------

In addition to the global system variables, Jenkins also sets additonal environment variables for each project that are only accessible to that project at runtime.

Create a new freestyle job and add a new **Execute shell** build step. Add the below commands to the text box.

```bash
echo "Hello World!"
echo "This is ${NAME} from build number ${BUILD_NUMBER} running in folder ${WORKSPACE}"
echo "My purpose is ${PURPOSE} and the tools I have are in ${JAVA_HOME} and ${JENKINS_HOME}"
```

Above the text box, there will also be a link to see `the list of available environment variables`. Click on that link to see the variables Jenkins sets for each job.

**Step 4: Set Step-Specific Environment Variables**
-------------------------------------------

In pipeline projects, environment variables can also be set for each step!

Create a new pipeline project and copy the below pipeline script into the script area at the bottom of the configuration page:

```
pipeline {
    agent any

    environment {
        ANIMAL="rabbit"
    }
    stages {
        stage('Hello') {
            steps {
                echo "Hello World!"
                echo "This is ${NAME} from build number ${BUILD_NUMBER} running in folder ${WORKSPACE}"
                echo "My purpose is ${PURPOSE} and the tools I have are in ${JAVA_HOME} and ${JENKINS_HOME}"
            }
        }
        stage('Blue') {
            environment {
                COLOR="blue"
            }
            steps {
                echo "My favorite color is ${COLOR} and my favorite animal is a ${ANIMAL}!"
            }
        }
        stage('Green') {
            environment {
                COLOR="green"
            }
            steps {
                echo "No, my favorite color is ${COLOR} and my favorite animal is a ${ANIMAL}!"
            }
        }
    }
}
```

Even though the same variable was set earlier on in the pipeline, adding the `environment` block setting the `COLOR` variable to each step allowed us to change the value of it for each step. 

Similarly, adding the `environment` block before `stages` allowed us to set an environment variable for the entire pipeline.

Once you're finished, let the instructor know you are done so we can move on.