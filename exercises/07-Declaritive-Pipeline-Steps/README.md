**Using the Pipeline Steps Reference**
=====================================================

Pipeline jobs are a little more difficult to setup than Freestyle jobs becuase they require some knowledge of Groovylang synax and how each plugin operates. To help, each Jenkins installation has a tool built-in to generate steps and directives for declaritive pipelines, and there's a significant amount of documentation online to help sort through the wide variety of available plugins and options.

In this exercise, you'll learn how to use the Jenkins documentation online and through the Pipeline steps generator.

**Step 1: Open the Jenkins Online Documentation**
-------------------------------------------

The Jenkins homepage has a large reference of pipeline steps and syntax available at [https://www.jenkins.io/doc/](https://www.jenkins.io/doc/). 

Navigate to the User Documentation Home page and scroll down the navigation menu on the left side to [Pipeline Syntax Reference](https://www.jenkins.io/doc/book/pipeline/syntax/). Here you'll find examples and explainations of each type of declaritive statement, including some you'll already be familiar with from other sections of this course.

Next, from the left-side navigation menu, open the link for [Pipeline Steps Reference](https://www.jenkins.io/doc/pipeline/steps/). This is a large collection of each plugin under the [Jenkins Plugin page](https://plugins.jenkins.io/) and the steps provided by that plugin. You can search here for steps that you may want to use when building your own pipelines, but are not sure if the plugins on your system support the operations you want to use.

Use the search bar to look for `SonarQube` and click on the **SonarQube Scanner for Jenkins** from the list that we used in a previous exercise. From here, you can see other steps and methods provided by that plugin, as well as examples on how to use it.

**Step 2: Use the Snippet Generator**
-------------------------------------------

You can see a similar reference from Jenkins for currently installed plugins without ever having to go online. 

Open the Hello-World-Pipeline job you made way back in exercise 2. On the left side navigation panel, click on the button labeled **Pipeline Syntax**. This will take you to a page called **Snippet Generator** as well as some links for the **Declarative Directive Generator** and the online documentation.

The Snippet Generator can help you generate build steps and post-build steps that are syntatically correct. Use it to create a new `readFile` step to read the file `README.md` and save the command for later.

Next, use it to generate a `cleanWs` post-build step that will clean any files that will exclude any files written in markdown, then save the command for later.

**Step 3: Use the Declarative Directive Generator**
-------------------------------------------

In the same way the Snippet Generator creates steps, the Declarative Directive Generator generates pipeline code. It can be used to create different blocks in pipeline scripts to specify build agents, triggers, options and more.

Go ahead and use it to create a new stage called **ReadFile**. This stage should contain steps, as well as a Post Stage or Build Condition.

Next, add the build step and post-build step you generated earlier into the new stage and add it to the pipeline.

Finally, make a new stage to put after **ReadFile** called **ListAgain**. This new stage should list the contents of the workspace by executing a shell command.

<details>

<summary>Click here for solution</summary>

```
pipeline {
    agent any
    triggers {
      pollSCM '* * * * *'
    }

    stages {
        stage('Initialize') {
            steps {
                checkout scmGit(
                    branches: [[name: '**']], 
                    extensions: [], 
                    userRemoteConfigs: [[
                        credentialsId: 'Github-credentials', 
                        url: 'https://github.com/devbyaccident/Demo_Repo'
                    ]]
                )
            }
        }
        stage('Hello') {
            steps {
                echo "Hello World!"
                echo "This is build number $BUILD_NUMBER running in folder $WORKSPACE"
                sh("ls -altr")
            }
        }
        stage('ReadFile') {
            steps {
                readFile './README.md'
            }

            post {
                always {
                    cleanWs(patterns: [[pattern: '*.md', type: 'EXCLUDE']])
                }
            }
        }
        stage('ListAgain') {
            steps {
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

</details>


With your pipeline script finished, go ahead and check it into git on the `dev` branch and wait for the build to start.

Once you've verified that it ran without errors, let the instructor know you are ready to proceed.