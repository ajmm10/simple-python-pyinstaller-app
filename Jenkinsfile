
> User Documentation Home

User Handbook
User Handbook Overview
Installing Jenkins
Platform Information
Using Jenkins
Pipeline
Blue Ocean
Managing Jenkins
Securing Jenkins
System Administration
Scaling Jenkins
Troubleshooting Jenkins
Glossary
Tutorials
Guided Tour
Jenkins Pipeline
Using Build Tools
Resources
Pipeline Syntax reference
Pipeline Steps reference
LTS Upgrade guides
Build a Python app with PyInstaller 
Table of Contents
Prerequisites
Fork and clone the sample repository
Start your Jenkins controller
Create your Pipeline project in Jenkins
Create your initial Pipeline as a Jenkinsfile
Add a test stage to your Pipeline
Add a final deliver stage to your Pipeline
Follow up (optional)
Wrapping up
Cleaning Up Your Environment
This tutorial shows you how to use Jenkins to orchestrate building a simple Python application with PyInstaller.

If you are a Python developer who is new to CI/CD concepts, or you might be familiar with these concepts but don’t know how to implement building your application using Jenkins, then this tutorial is for you.

The simple Python application (which you’ll obtain from a sample repository on GitHub) is a command line tool "add2vals" that outputs the addition of two values. If at least one of the values is a string, "add2vals" instead treats both values as a string and concatenates the values. The "add2" function in the "calc" library (which "add2vals" imports) is accompanied by a set of unit tests. These are tested with pytest to check that this function works as expected and the results are saved to a JUnit XML report.

The delivery of the "add2vals" tool through PyInstaller converts this tool into a standalone executable file for Linux, which you can download through Jenkins and execute at the command line on Linux machines without Python.

Duration: This tutorial takes 20-40 minutes to complete (assuming you’ve already met the prerequisites below). The exact duration will depend on the speed of your machine and network.

You can stop this tutorial at any point in time and continue from where you left off.

If you’ve already run though another tutorial, you can skip the Prerequisites section below and proceed on to forking the sample repository. (Just ensure you have Git installed locally.) If you need to restart Jenkins, simply follow the restart instructions in Stopping and restarting Jenkins and then proceed on.

Prerequisites
For this tutorial, you will require:

A macOS, Linux, Windows, or Chromebook (with Linux) machine with:

2 GB of RAM

2 GB of drive space for Jenkins

The following software installed:

Docker

Docker Compose

Git and optionally GitHub Desktop

Fork and clone the sample repository
Obtain the simple "add" Python application from GitHub, by forking the sample repository of the application’s source code into your own GitHub account and then cloning this fork locally.

Ensure you are signed in to your GitHub account. If you don’t yet have a GitHub account, sign up for a free one on the GitHub website.

Fork the simple-python-pyinstaller-app on GitHub into your local GitHub account. If you need help with this process, refer to the Fork A Repo documentation on the GitHub website for more information.

Clone your forked simple-python-pyinstaller-app repository (on GitHub)locally to your machine. To begin this process, do either of the following (where <your-username> is the name of your user account on your operating system):

If you have the GitHub Desktop app installed on your machine:

In GitHub, click the green Code button on your forked repository, then Open in Desktop.

In GitHub Desktop, before clicking Clone on the Clone a Repository dialog box, ensure Local Path for:

macOS is /Users/<your-username>/Documents/GitHub/simple-python-pyinstaller-app

Linux is /home/<your-username>/GitHub/simple-python-pyinstaller-app

Windows is C:\Users\<your-username>\Documents\GitHub\simple-python-pyinstaller-app

Otherwise:

Open up a terminal/command line prompt and cd to the appropriate directory on:

macOS - /Users/<your-username>/Documents/GitHub/

Linux - /home/<your-username>/GitHub/

Windows - C:\Users\<your-username>\Documents\GitHub\ (although use a Git bash command line window as opposed to the usual Microsoft command prompt)

Run the following command to continue/complete cloning your forked repo:
git clone https://github.com/YOUR-GITHUB-ACCOUNT-NAME/simple-python-pyinstaller-app
where YOUR-GITHUB-ACCOUNT-NAME is the name of your GitHub account.

Start your Jenkins controller
Obtain the latest Jenkins LTS deployment, customized for this tutorial, by cloning the quickstart-tutorials repository.

After cloning, navigate to the quickstart-tutorials directory, and execute the command

docker compose --profile python up -d
to run the example.

Once the containers (one for the controller, one for the agent) are running successfully (you can verify this with docker compose ps), the controller can be accessed at http://localhost:8080.

If you are unable to install docker compose on your machine for any reason, you can still run the example in the cloud for free thanks to GitPod. GitPod is free for 50 hours per month. You need to link it to your GitHub account so you can run the example in the cloud. Click on that link to open a new browser tab with a GitPod workspace where you’ll be able to start the Jenkins controller, and run the rest of the tutorial.

Now, log in using the admin username and admin password.

Create your Pipeline project in Jenkins
In Jenkins, select New Item under Dashboard > at the top left.

Enter your new Pipeline project name in Enter an item name (e.g. simple-python-pyinstaller-app).

Scroll down if necessary and select Pipeline, then click OK at the end of the page.

(Optional) Enter a Pipeline Description.

Select Pipeline on the left pane.

Select Definition, and then choose the Pipeline script from SCM option. This option instructs Jenkins to obtain your Pipeline from the source control management (SCM), which is your forked Git repository.

Choose Git from the options in SCM.

Enter the URL of your repository in Repositories/Repository URL. This URL can be found when clicking on the green button Code in the main page of your GitHub repo.

Hit the Save button at the end of the page. You’re now ready to create a Jenkinsfile to check into your locally cloned Git repository.

Create your initial Pipeline as a Jenkinsfile
You’re now ready to create your Pipeline that will automate building your Python application with PyInstaller in Jenkins. Your Pipeline will be created as a Jenkinsfile, which will be committed to your locally cloned Git repository (simple-python-pyinstaller-app), and then pushed to GitHub, where Jenkins will be able to find it.

This is the foundation of "Pipeline-as-Code", which treats the continuous delivery pipeline as part of the application to be versioned and reviewed like any other code. Read more about Pipeline and what a Jenkinsfile is in the Pipeline and Using a Jenkinsfile sections of the User Handbook.

First, create an initial Pipeline with a "Build" stage that executes the first part of the entire production process for your application. This "Build" stage compiles your simple Python application into byte code.

Using your favorite text editor or IDE, create and save a new text file with the name Jenkinsfile at the root of your local simple-python-pyinstaller-app Git repository.

Copy the following Declarative Pipeline code and paste it into your empty Jenkinsfile:

pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
    }
}
The agent section with the any parameter specified at the top of this Pipeline code block means that any agent could be allocated for the entire Pipeline’s execution and that each stage directive does not have to specify its own agent section.
Defines a stage (directive) called Build that appears on the Jenkins UI.
This sh step (of the steps section) runs the Python command to compile your application and its calc library into byte code files (each with .pyc extension), which are placed into the sources workspace directory (within the /var/jenkins_home/workspace/simple-python-pyinstaller-app directory in the Jenkins container).
This stash step (of the basic steps section) saves the Python source code and compiled byte code files (with .pyc extension) from the sources workspace directory for use in later stages.
Save your edited Jenkinsfile and commit it to your local simple-python-pyinstaller-app Git repository. E.g. Within the simple-python-pyinstaller-app directory, run the commands:
git add .
then
git commit -m "Add initial Jenkinsfile"
and in the end
git push

In Jenkins, select Build Now on the left pane.

After making a clone of your local simple-python-pyinstaller-app Git repository itself, Jenkins:

Initially queues the project to be run on the agent.

Runs the Build stage defined in the Jenkinsfile on the agent.

You should now see on the left a green check mark and #1, indicating that your first Pipeline has run successfully.

You should also see in the main part of the page a Stage View of your Pipeline, which shows the Build stage run, indicating that your first Pipeline has run successfully.

first maven job success

You can now click on #1 to see the details of the build. You will then see how much time the build took waiting in the queue and how much time it took to run.

Initial Pipeline first run details On the left, you can click on Pipeline Overview to see the stages of the Pipeline.

Initial Pipeline runs successfully If you click on the Build stage, you will see more information about the stage, including the output of the python command if you click on the green python section.

Build stage overview You can now click on simple-python-pyinstaller-app (if that’s the name you chose for your pipeline) on the top left to return to your pipeline main page.

Add a test stage to your Pipeline
Go back to your text editor/IDE and ensure your Jenkinsfile is open.

Copy and paste the following Declarative Pipeline syntax immediately under the Build stage of your Jenkinsfile:

        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
so that you end up with:

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') { 
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py' 
            }
            post {
                always {
                    junit 'test-reports/results.xml' 
                }
            }
        }
    }
}
Defines a stage (directive) called Test that appears on the Jenkins UI.
This sh step (of the steps section) executes pytest’s py.test command on sources/test_calc.py, which runs a set of unit tests (defined in test_calc.py) on the "calc" library’s add2 function (used by your simple Python application add2vals). The:
--junit-xml test-reports/results.xml option makes py.test generate a JUnit XML report, which is saved to test-reports/results.xml (within the /var/jenkins_home/workspace/simple-python-pyinstaller-app directory in Jenkins).

This junit step (provided by the JUnit Plugin) archives the JUnit XML report (generated by the py.test command above) and exposes the results through the Jenkins interface. The post section’s always condition that contains this junit step ensures that the step is always executed at the completion of the Test stage, regardless of the stage’s outcome.
Save your edited Jenkinsfile and commit it to your local simple-python-pyinstaller-app Git repository. E.g. Within the simple-python-pyinstaller-app directory, run the commands:
git add .
then
git commit -m "Add 'Test' stage" and in the end
git push

In Jenkins, navigate back to the Dashboard if necessary, then simple-python-pyinstaller-app and launch another build thanks to Build Now.

After a while, you should see a new column labeled Test appear in the Stage View.

Test stage runs successfully

You can click on #2 or on the number representing your last build on the left, under Build History. You will then see the details of the build.

You can now select Pipeline Overview to see the stages of the Pipeline.

Test stage runs successfully

Notice the additional "Test" stage. You can select the "Test" stage checkmark to access the output from that stage.

Test stage runs successfully (with output)

Add a final deliver stage to your Pipeline
Go back to your text editor/IDE and ensure your Jenkinsfile is open.

Copy and paste the following Declarative Pipeline syntax immediately under the Test stage of your Jenkinsfile:

        stage('Deliver') {
            steps {
                sh "pyinstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
and add a skipStagesAfterUnstable option so that you end up with:

pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh "pyinstaller --onefile sources/add2vals.py" 
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals' 
                }
            }
        }
    }
}