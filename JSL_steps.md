## Demo Project - Create a Jenkins Shared Library

This demo shows the process of creating a Jenkins Shared Library to extract common build logic

## Steps to Create a Jenkins Shared Library to Extract Common Build Logic

1. **The first thing I did was change the name of the multi-branch pipeline I made in Jenkins UI to "microservice-user-auth"**

2. **I then created another multi-branch pipeline job called "microservice-payment" and copied it from the "microservice-user-auth" job**

**The background for this is that I have an application that is composed of about 10 microservices, each with it's own pipeline**

**This means I will have multiple projects that all have their own Jenkinsfile, that are building a pipeline in Jenkins, and if all or some of those microservices are also Java Maven applications (and I'm building Docker Images from each one), then I'll have functions or logic inside each pipeline that are 90% the same**

**Jenkins Shared Library is an extension to pipeline and JSL is its own repository (which is written in groovy), and I wrote all the logic that was going to be shared across the different applications/microservices in the Shared Library. I then referenced that logic from the Jenkinsfile in each project**

**The benefits of using JSL: code reusability between multiple teams working on different projects in the same company, consistency and standardization of shared code between teams and employees in same company, faster pipeline creation, and improved collaboration**

**It is highly probably that if you are in a medium or small project you will have to use JSL**

3. **In order to use Jenkins Shared Library in my pipeline, I had to first create a repository for JSL and write groovy code in there**

```bash
cd <path to project directory>
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install groovy
groovy -version
mvn archetype:generate -DgroupId=com.github.Learnubq -DartifactId=jenkins-shared-library -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

**I then opened the project directory in VSCode and created a new "Main.groovy" file**

**I then created a "vars" folder in the project directory. Within it I created a <function_name>.groovy file for each function. I needed to add "#!/usr/bin/env" to the beginning of each .groovy file - it lets the editor detect that you are working with a groovy scipt**

**I then created a repository on gitlab so I could use it in Jenkins**

**I then intialized the git repo and pushed from it to the remote repo**

```bash
cd <path to project directory>
git status
git init
git remote add origin git@gitlab.com:learn.ubq-group/jenkins-shared-library.git
git add .
git commit -m "Initial commit"
git push -u origin master
```


**I then had to make the Shared Library available globally or for specific pipeline job(s)/project(s). I did this by going to manage Jenkins settings - system. I then scrolled down to "Global Trusted Pipeline Libraries" and added the JSL - "jenkins-shared-library". I made the default library the branch e.g. master, but I could have made it the tag  or the commit hash**

**The Retrieval method was "SCM" and the Source Code Management was "Git" as it is a Gitlab repository for the JSL. The project repository was the SSH endpoint and the credentials were the Gitlab credentials. I then clicked save**

**I then had to use the JSL or functions in the JSL in the Jenkinsfile for a pipeline. For this I went back to my java-maven-app repo on Gitlab and created a new branch from master called "jenkins-shared-library" - this is where I was going to test the changes**

**In VSCode terminal I changed to the new "jenkins-shared-library" branch in the java-maven-app repo. In the Jenkinsfile I added a groovy line at the top so VSCode could detect the groovy script**

4. **I then scanned multibranch pipeline log in Jenkins UI for "java-maven-app" - the new "jenkins-shared-library" was now detected in the java-maven-app multibranch pipeline job**

5. **I added a parameter to Docker build in Jenkinsfile for java-maven-app. I also aded a parameter to call Docker build with a parameter in the buildImage and buildJar .groovy files. I then built the the Jenkins Shared Library single step job, within the java-maven-app multibranch pipeline in Jenkins UI**

6. **I then added .groovy scripts that held the logic to be extracted into Groovy classes, to the "src" folder in the Jenkins Shared Library repository**

**I added a package called "com.example" to the src folder. Within that I added a class (a file called "Docker.groovy")**

**I then integrated and used the Jenkins Shared Library in the Jenkins Pipeline job, both globally and for the specific project in the Jenkinsfile**

<img width="1862" height="849" alt="global1" src="https://github.com/user-attachments/assets/2fde440a-6d48-4ee4-bd1c-eee3d9388e86" />

<img width="1555" height="228" alt="global2" src="https://github.com/user-attachments/assets/30028bec-e51a-4609-8a6f-d829e206d753" />

**I then tested it worked by running the jenkins-shared-library pipeline job**
