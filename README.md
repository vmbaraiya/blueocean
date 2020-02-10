
# Cloud DevOps Engineer - JENKINS PIPELINE FOR AWS

The blueocean project to practice skills with deployment and pipelines using Jenkins Blueocean.

## Getting Started

*A continuous delivery (CD) pipeline is an automated expression of your process for getting software from version control right through to your users and customers.*

### Prerequisites

Below software and files are required to create blueocean pipeline

```
Jenkins
Blueocean Plugin
Jenkinsfile
index.html file
AWS Account
S3 Bucket
```

### Installing

Install Jenkins on one of the EC2 Instance ( I am using Ubuntu here)

```shell
sudo apt-get update
sudo apt install -y default-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get install -y jenkins
```

 Access jenkins using  [JenkinsURL](http://<hostname>:8080) - http://hostname:8080
 Access the Admin password from secret file:

```shell
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
 ```
  
 Install BlueOcean Plugin and configure scan repository triggers 
  
 ```GUI
  * check the box Periodically if not otherwise run
  * provide interval to check for any changes ( example 1 minute)
 ```
 
 ### Practice Exercise 1 
 * **Set up a “Hello World” pipeline in Blue Ocean.**
 Create Jenkinsfile with the following content in the repository and commit.

```groovy
pipeline {
  agent any 
  stages {
    stage('Build') {
      steps {
        sh 'echo "Hello World"'
        sh '''
                  echo "Multiline shell steps works too"
                  ls -lah
               '''
      }
    }
  }
}
```

Here, simple Hello World Pipeline executes with the commit.

 ### Practice Exercise 2
 * **Lint test of HTML**
 * Install “lint” onto your Jenkins Master. This is to check the HTML code for malformed tags
```shell
sudo apt install tidy
```
create an index.html file in your repo.
Jenkinsfile to setup a Lint Job

```groovy
pipeline {
  agent any
  stages {
    stage('Lint HTML') {
      steps {
        sh 'tidy -q -e *.html'
      }
    }
  }
}
```

 ### Practice Exercise 3
  * **Upload in S3 bucket using Jenkins pipeline**
     * Set up your AWS credentials with your access key and secret access key in Jenkins Credentials.
          * Jenkins Home Page > Credentials > System > Gloabal Credentials > Add Credentials > select kind "AWS Credentials"
     * Create your S3 bucket (Name must be unique)
     * Set up your pipeline
          
```groovy
pipeline {
    agent any
    stages {
      stage('Lint HTML') {
        steps {
          sh 'tidy -q -e *.html'
        }
      stage('Upload to AWS') {
        steps {
          withAWS(region:'us-east-2',credentials:'S3Pipeline') {
            s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'awscodepipeline-blueocean')
          }
        }
      }
```
Here is the snapshot my blueocean pipeline:
![Pipeline Output](https://github.com/vmbaraiya/blueocean/blob/master/output.png)

## Reference Documents for more details:

* [Setting up a CI/CD pipeline by integrating Jenkins with AWS CodeBuild and AWS CodeDeploy](https://aws.amazon.com/blogs/devops/setting-up-a-ci-cd-pipeline-by-integrating-jenkins-with-aws-codebuild-and-aws-codedeploy/) 
* [Developing an AWS CodePipeline – Creating a Jenkins Pipeline to Build and Test](https://blog.toadworld.com/2017/12/30/developing-an-aws-codepipeline-creating-a-jenkins-pipeline-to-build-and-test) 
* [CI/CD Pipeline with Jenkins on AWS](https://medium.com/faun/ci-cd-pipeline-with-jenkins-and-aws-s3-c08a3656d381) 
* [Canary Release](https://martinfowler.com/bliki/CanaryRelease.html)
* [Blue-Green Deployment](https://aws.amazon.com/quickstart/architecture/blue-green-deployment/)


