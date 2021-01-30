
## Experience Continous Integration With Jenkins | Ansible | Artifactory | Sonarqube | PHP ##

WARNING! - This project has a bit of initial theoretical concepts. PLEASE READ! If you need to, READ AGAIN! until it sinks in. It is one of the most important and fundamental concept in DevOps.

In the past, you have been working on Linux and Ansible and just a basic overview with hands on Jenkins. Here, there is a lot to understand and do. Therefore, there will be a bit of theory at the start. But trust me, it is all worth it. This will set the stage for you to truly understand why you need to build and configure the things that will be happening in this project.

In previous project, you deployed the tooling website directly into the /var/www/html folder on the dev server. Well, even though that worked, and we are still able to access the website, it is not the best way to go. In the real world, most application code like java .net etc are usually compiled and built to create an executable file. The executable file such as jar file in the case of java will contain all the code embeded, and the necessary library dependencies in which the application needs to run and work successfully. Some other programs like PHP work directly without building into some executable file. That is why we could easily deploy the entire code from git into var/www/html and immediately the webserver is able to render the pages on the browser. However, it is not ideal to download code directly from git on to our servers. There should be a smarter way to package the entire application code, and track the release versions. We cna package the entire code and all its dependencies into some form like .tar.gz or .zip so that it can be easily unpacked on the respective environment’s servers.

In this project, you will understand and get hands on experience around the entire concept around CI/CD from applications perspective. To fully gain real expertise around this idea, it is best to see it in action across different programming languages and from the platform perspective too. From the application perspective, we will be focusing on PHP here; There are more projects ahead that are based on Java, NODEJS, .Net and Python. By the time you start working on Terraform, Docker and Kubernetes projects, you will get to see the platform perspective of CI/CD in action.

What is Continous Integration
In software engineering, continuous integration (CI) is the practice of merging all developers’ working copies to a shared mainline (Git Repository) several times a day. This means multiple git commit, several times everyday.

The general idea behind multiple commits is to avoid what is generally considered as Merge Hell or Integration hell. When a new developer joins a new project, he will have to create a copy of the main codebase by creating a new git feature branch from the mainline. In some organization or team, this could be the develop, main or master branch to develop his own features. If there are tens of developers working on the project, they will all do the same thing. Which is the case in the real world. Some projects even have hundreds of developers working on the same solution. Maybe a mobile application, website or a web app. As each developer work on their own feature branch, the main branch they took a copy from will start drifting away. If this linger on for a very long time without re-conciling the code, then this will cause a lot of code conflict or Merge Hell as rightly said. Imagine such a hell from tens of developers or worse, hundreds. So the best thing to do, is to continously commit code to the mainline. As many times as tens of times per day. With this practice, you can avoid Merge Hell or Integration hell

CI concept does not really end at just commiting your code. There is a general workflow. Lets start exploring the flow…

Run tests locally: Before developers commit their code to the central repository (Git repository), it is best practice to test the code locally before commit. So, test-driven development is used in combination with CI. Developers will write tests for their code called unit-tests, and before they commit their work, they will use some tools to run their tests locally. This practice helps the team to avoid having developer’s work-in-progress code from breaking other developer’s copy of the code-base.

Compile code in CI: After testing the code locally, developers will commit and push their work to GIT. Rather than building the code into an executable locally, a dedicated server for CI will pick up the code and run the build there. For the sake of this particular project, we will use Jenkins as our CI server. This happens either periodically by polling the repository at every X mins/hours configured, or after every commit. Having a CI server where this build runs is a good practice for the team as everyone will have visibility into each commit and its corresponding build.

Run further tests in CI: Even though tests have been ran locally by developers, it is important to also run the unit-tests on the CI server as part of the continous process. But, rather than focusing solely on unit-tests, there are other kinds of tests and code analysis thay can be done using the CI server. These are very critical to determining the overall quality of code being developed, how it interact with other developers work, and how less vulnerable it is to attacks. The CI server uses different tools for Static Code Analysis, Code Coverage Analysis, Code smells Analysis, and Compliance Analysis. In addition, it runs other tests such as Integration, and Penetration tests. Other tasks perfomed on the CI server include producing code documentation from the source code, and facilitate manual quality assurance (QA) testing processes.

Deploy an artifact from CI: At this stage, the difference between CI and CD is spelt out. As you now know, CI is Continous Integration, which is everything we have been discussing thus far. CD on the otherhand is Continous Delivery which ensures that software checked into the mainline is always ready to be deployed to users. The deployment here is manually triggered after certain QA tasks is completely satisfactory. There is another CD known as Continous Deployment which is also about deploying the software to the users, but rather than manual, it makes the entire process fully automated.


Continous Integration In The Real World
To emphasize a typical CI Pipeline further, Let us explore the diagram below a little deeper


Picture

Source: whitesourcesoftware

Version Control: This is the stage where developers’ code gets committed and pushed after they have tested their work locally.

Build: Depending on the type of language or technology, we may need to packaged code dependencies and build to a computer language. Some languages like PHP or Javascript do not require to be built because they are scripting or interpreted languages. While languages such as Java, C, Golang, Scala, .Net, etc are compiled languages. Their syntax cannot be directly understood by the computer. They require the Build phase which means must be compiled to a binary file that can be understood by the computer. here is a good read to learn more about the differences between scripting and programming languages.

Unit Test: Unit tests that have been developed by the developers are tested. Depending on how the CI job is configured, the entire pipeline may fail if part of the tests fail and developers will have to fix this failure before starting the pipeline again. A Job by the way, is a phase in the pipeline. Unit Test is a phase, therefore it can be considered a job on its own.

Deploy: Once the tests pass, the next phase is to deploy the compiled or packaged code into an environment a code repository. This is where all the various versions of code including the latest will be stored. The CI tool will have to pick up the code from this location to proceed with the remaining parts of the pipeline.

Auto Test: Apart from Unit testing, there are many other kinds of tests that are required to analyse the quality of code, and determine how vulnerable the software will be to external or internal attacks. These tests must be automated, and there can be multiple environments created to fulfil different test requirements. For example, a server dedicated to Integration Testing will have the code deployed there to conduct integration tests. Once that passes, there can be othere sub layers in the testing phase in which the code will be deployed to so as to conduct further tests. Such are User Acceptance Testing (UAT), and another can be Penetration Testing. These servers will be named according to what they have been designed to do in those environments. A UAT server is generally be used for UAT, SIT server is for Systems Integration Testing, PEN Server is for Penetration Testing and they can be named whatever the naming style or convention in which the team has adopted. An environment does not necessarily have to be one single server. It will in most cases be a stack as you have defined in your Ansible Inventory. All the servers in the inventory/dev are considered Dev Environment. The same goes for inventory/stage (Staging Environment) inventory/preprod (Pre-production environment), inventory/prod (Production environment) etc. So its all down to naming convention as agreed and used company or team wide.

Deploy to production: Once all the tests have been conducted and either the release manager or whoever has the authority to authorize the release to the production server is happy, he gives the green light to hit the deploy button to production. This is an Ideal Continous Delivery Pipeline. If the entire pipeline was automated and no human is required to manually give the Go decision, then this would be considered Continous Deployment. Because the cycle will be repeated, and everytime there is a code commit and push, it causes the pipeline to trigger and the loop continues over and over.

Measure And Validate: This is where live users are interating with the application and feedback is being gathered for improvement and bug fixes. There are many metrics that must be determined and observed here. We will quickly go through 15 metrics that MUST be considered.

Common Best Practices of CI/CD
Maintain a code repository

Automate the build

Make the build self-testing

Everyone commits to the baseline every day

Every commit (to baseline) should be built

Every bug-fix commit should come with a test case

Keep the build fast

Test in a clone of the production environment

Make it easy to get the latest deliverables

Everyone can see the results of the latest build

Automate deployment

Why are we doing everything we are doing? - 13 DevOps Success Metrics
At the end of it all, DevOps is all about continous delivery or deployment, and being able to ship out quality code as fast as possible. This is a very ambitious thing to desire, therefore we must be careful not to break things as we are moving very fast. By tracking these metrics, we can determine our delivery speed and bottle necks before breaking things. Ultimately, the goals of DevOps is Velocity, Quality, and Performance. But how do we track these? Let us have a look at the 13 metrics to watch out for.

1 Deployment frequency: Tracking how often you do deployments is a good DevOps metric. Ultimately, the goal is to do more smaller deployments as often as possible. Reducing the size of deployments makes it easier to test and release. I would suggest counting both production and non-production deployments separately. How often you deploy to QA or pre-production environments is also important. You need to deploy early and often in QA to ensure time for testing.

2 Lead time: If the goal is shipping code quickly, this is a really key DevOps metric. I would define lead time as the amount of time that occurs between starting on a work item until it is deployed. This helps you know that if you started on a new work item today, how long would it take on average until it gets to production

3 Customer tickets: The best and worst indicator of application problems is customer support tickets and feedback. The last thing you want is for your users to find bugs or have problems with your software. Because of this, they also make a good indicator of application quality and performance problems.

4 Percentage of automated tests pass: To increase velocity, it is highly recommended that the development team makes extensive usage of unit and functional testing. Since DevOps relies heavily on automation, tracking how well automated tests work is a good DevOps metrics. It is good to know how often code changes are causing tests to break.

5 Defect escape rate: Do you know how many software defects are being found in production versus QA? If you want to ship code fast, you need to have confidence that you can find software defects before they get to production. Defect escape rate is a great DevOps metric to track how often those defects make it to production.

6 Availability: The last thing we ever want is for our application to be down. Depending on the type of application and how we deploy it, we may have a little downtime as part of scheduled maintenance. It is highly recommended to track this metric and all unplanned outages. Most software companies build status pages to track this. Such as this Google Products Status Page

7 Service level agreements: Most companies have some service level agreement (SLA) that they operate with. It is also important to track compliance with SLAs. Even if there are no formal SLA, there probably are application requirements or expectations to be achieved.

8 Failed deployments: We all hope this never happens, but how often do our deployments cause an outage or major issues for our users? Reversing a failed deployment is something we never want to do, but it is something you should always plan for. If you have issues with failed deployments, be sure to track this metric over time. This could also be seen as tracking *Mean Time To Failure8 (MTTF).

9 Error rates: Tracking error rates within the application is super important. Not only are they an indicator of quality problems, but also ongoing performance and uptime related issues. In software development, errors are also known as exceptions, and good exception handling best practices are very critical. If these are not handled nicely, we can figure it out while monitoring the rate of errors.

Bugs – Identify new exceptions being thrown in the code after a deployment

Production issues – Capture issues with database connections, query timeouts, and other related issues.

Presenting error rate metrics like this simply gives greater insights into where to focus attention.












Ansible Inventory should look like this
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat













Running Ansible Playbook From Jenkins
Now that you have a broad overview of a typical Jenkins pipeline. Lets get the actual ansible deployment to work by

Installing Ansible on Jenkins

Installing Ansible plugin in Jenkins UI

Starting the Jenkinsfile coding from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)

You can watch a 10 mins video here to guide you through the entire setup

Note: Ensure that Ansible runs against the Dev environment successfully. Errors to watch out for:

Ensure that the git module in Jenkinsfile is Checking out SCM to main branch instead of master (Github has discontinued the use of Master due to Black Lives Matter. You can read more here)

Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deploymemnt. Then, using the Pipeline Syntax tool in Ansible, generate the sytax to create environment variables to set.

https://wiki.jenkins.io/display/JENKINS/Building+a+software+project

_images/Jenkins-Workspace-Env-Var.png

Possible issues to watch out for while you implement this

Remember that ansible.cfg must be exported to environment variable so that Ansible knows where to find Roles. But because you will possibly run Jenkins from different git branches, the location of Ansible roles will change. Therefore, you must handle this dynamically. You can use Linux Stream Editor sed to update the section roles_path each time there is an execution. You may not have this issue if you run only from the main branch.

If you push new changes to Git so that Jenkins failure can be fixed. You might observe that your change may sometimes have no effect. Even though your change is the actual fix required. This can be because Jenkins did not download the latest code from github. Ensure that you start the Jenkinsfile with a clean up step to always delete the previous workspace before running a new one. Sometimes you might need to login to the Jenkins Linux server to verify the files in the workspace to confirm that what you are actually expecting is what is there. Otherwise you can spend hours trying to figure out why Jenkins is still failing, when you have pushed up possible changes to fix the error.

Another possible reason for Jenkins failure sometimes, is because you have indicated in the Jenkinsfile to checkout the main git branch, and you are running a pipeline from another branch. So, always verify by logging onto the Jenkins box to check the workspace, and run git branch command to confirm that the branch you are expecting is what is there.

If everything goes well for you, it means, the Dev environment has an up-to-date configuration. But what if we need to deploy to other environments?

Are we going to manually update the Jenkinsfile to point inventory to those environments? such as sit, uat, pentest etc…

Or do we need a dedicated git branch for each environment, and have the inventory part hard coded there.

Think about those for a minute and try to work out which one sounds more like a better solution.

Manually updating the Jenkinsfile is definitely not an option. And that should be obvious to you at this point. Because we try to automate things as much as possible.

Well, unfortunately, we won’t be doing any of the highlighted options. What we will be doing is to parameterise the deployment. So that at the point of execution, the appropriate values are applied.
















Build Failed Issue :

Check for below command in Jenkins Server in linux Env:
`whereis git`
you will get the path like /usr/bin/git

Place it in Manage jenkin>Global Tool Configuration> under git path mention /usr/bin/git

Rerun job again
---------------------------------------------------------------------
Notice that this pipeline is a mutlibranch one. This means, if there were more than 1 branch from git, Jenkins will scan the repository to discover them all and we will be able to trigger a build against each branch.

Lets see this in action.

Create a new git branch and name it feature/jenkinspipeline-stages

Currently we only have the Build stage. Lets add another stage Test. 
Paste the code snippet below and push the new changes to git.
Below this add the folowing:



git clone https://github.com/sidahal2018/config-mgt-ansible.git
ls -l
cd config-mgt-ansible/
git branch
git branch feature/jenkinspipeline-stages
git checkout feature/jenkinspipeline-stages
ls -l
cd deploy
ls -l
vi Jenkinsfile
git status
git add .
git status
git commit -m "adding test stage on jenkinsfile"
git push -u origin feature/jenkinspipeline-stages

---------------------------------------------------------
QUICK TASK FOR YOU!
1. Create a pull request so as to merge the latest code into the `main branch`
2. After merging the `PR`, go back into your terminal and switch into the `main` branch.
git checkout master
cd deploy and we check the jenkins file to see if the latest changes has been merged or not
vi Jenkinsfile
No latest chnages reflected yet.
3. Pull the latest change.
to get the latest chnages we do 
git pull origin master

4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an `echo` command like we have in `build` and `test` stages)
   1. Package 
   2. Deploy 
   3. cleanup
   
git branch feature/jenkinspipeline-more-stages
git checkout feature/jenkinspipeline-more-stages
cd deploy
ls -l
vi Jenkinsfile

pipeline {
    agent any


  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }


    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
	  stage('Package') {
      steps {
        script {
          sh 'echo "Packaging Stage"'
        }
      }
    }
	  stage('Deploy') {
      steps {
        script {
          sh 'echo "Deploying Stage"'
        }
      }
    }
	
	  stage('cleanup') {
      steps {
        script {
          sh 'echo "cleaning Stage"'
        }
      }
    }
	
	
    }
}

git status
git add .
git commit -m "adding more stages on jenkinsfile"
git push -u origin feature/jenkinspipeline-more-stages

   
5. Verify in Blue ocean that all the stages are working, then merge your feature branch to the main branch

We do the Pull request and merge to main branch
6. Eventually, your main branch should have a successful pipeline like this in blue ocean
Naviage to config-mgt-ansible
Cick on scan repository New

In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline to show up for the new branch.
----------------------------------------------------------------------------------------------------------------------------
















