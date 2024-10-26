**Parameterized Builds**
=====================================================

Sometimes you might need to run a Jenkins job with input from a person, such as a deployment pipeline where someone will need to select an environment to deploy an application to, or one that requires a file be uploaded in order to run.

Jenkins parameters allow values to be passed to a project before it runs. In this exercise, you'll learn how to use parameters in Freestyle and Pipeline jobs, as well as trigger downstream projects with and without parameters.

**Step 1: Setup a Parameterized Freestyle Project**
-------------------------------------------

From the Jenkins menu, create a new Freestyle job. Under General Settings, look for a button called **This project is parameterized** and click it. 

You'll see a drop-down menu labeled **Add Parameter**. Click on that and add a new String Parameter with the below values:

- Name: `city`
- Default Value: `Philadelphia`
- Description: `Name of your city`

Next, add a second, Multi-line String Parameter with the below values:

- Name: `teams`
- Default Value: 
```
Eagles
Flyers
Union
76ers
```
- Description: `Sports teams from your city`

Finally, go to the Build Steps section at the bottom of the page and add a new **Execute Shell** step with the script below:
```bash
echo "Hello ${city}!"
for team in ${teams}; do
    echo "Go ${team}!"
done
```

Save the configuration. When you save it, you should see **Build with Parameters** on the left side. Click on that to be taken to a page where you can change the parameters before building the job.

Set them however you'd like and click **Build**, then check the console output to see what happened.

> Note: If the job is triggered by automation, if a parameter isn't specified as an input, the default values will be used.

**Step 2: Setup a Parameterized Pipeline Project**
-------------------------------------------

With pipeline projects, you can define important parts of the pipeline in code without needing to use the console. 

This can be useful when creating pipelines that are dependant on things like Jenkins options, Environment Variables, Parameters, or even Build Triggers.

From the Jenkins console, create a new pipeline project. From here, without using the UI to set any other properties, enter the pipeline script below.

```
pipeline {
    agent any
    triggers {
        cron("* * * * *")
    }
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
    }
}
```

After it's first manual run, this job will continue to build under 2 conditions:

- Automatically every minute with the default parameters
- Manually with custom parameters

Go ahead and build it, changing the parameters however you like. Then compare your run to one done on schedule.

Afterwards, go back into the job configuration. You should see that several of the jobs configuration items were set by the pipeline script. 

So it doesn't continue to build in the background on your system, go ahead and delete this pipeline, then let the instructor know you are ready to proceed.