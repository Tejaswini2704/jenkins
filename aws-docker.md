# setup
- launch instancce
- install jenkins and java
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
- log in to jenkins -> setup jenlins
## Plugins 
- go to manage jenkins -> plugins -> available plugins
- install plugins
```
stage view
```

```
docker
```
```
AWS credentials
```
```
S3 publisher
```
```
Maven integration
```
## Tools 
- go to manage jenkins -> tool -> add tools
- add maven
   - name : maven
   - install automatically
- add docker
   - name: docker
   - install automatically
## Credentials
- go to manage jenkins -> credentials -> add global credentials
- add AWS credentials
   - kind : AWS credentials
   - ID : aws-cred
   - add access key and secret key of aws.
   -  create
## Pipeline
- click on new item
   - name: jenkins-docker
   - type: pipeline 
- pipeline
- stage 1 : code pull
- generate the steps code using the pipeline syntax
   - sample : Git: git
   - repo url : https://github.com/abhipraydhoble/Project-InsureMe.git
   - branch : main
   - generate syntax, copy nad paste it on steps. 
```
ptpeline {
    agent any
    tools {
        maven 'maven'
    }
    
    stages {
        stage ('code-pull'){
            steps {
                git branch: 'main', url: 'https://github.com/abhipraydhoble/Project-InsureMe.git'
            }
        }
    }
}
```
- apply ,save and build now
- now configure the code
- stage 2 : code-build
- apply, save and build now
- click on build number -> console output -> workspace -> go to ``/var/lib/jenkins/workspace/jenkins-docker `` folder -> go to ``target`` ->  you will see ``Insurance-0.0.1-SNAPSHOT.jar`` file.
- now we have to add this ``.jar`` file into S3 bucket.
## create S3 bucket
-name : insurance-project-artifact
- enable ACL
- remove the block
- enable bucket versioning
- create bucket.
## install AWS ClI in EC2 server
- install unzip
```
sudo apt install unzip -y
```
- AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```
## back to jenkins
- configure the pipeline
- add environment variables
- and generate pipelin syntax
  -  sample step : withCredentials : bind credentials to variable
   - binding : AWS access key and secret key
   - generate pipeline code.
```
pipeline {
    agent any 
    tools {
        maven 'maven'
    }
    
    environment{
        S3_BUCKET = "insurance-project-artifact " 
        AWS_REGION = "ap-southeast-1"
        warFile = "target/Insurance-0.0.1-SNAPSHOT.jar"
        }
    
    stages{
        stage('code-pull'){
            steps{
                git branch: 'main', url: 'https://github.com/abhipraydhoble/Project-InsureMe.git'
            }
        }
        
        stage('code-build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('upload artifacts'){
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                 sh 'aws s3 cp ${warFile} s3://${S3_BUCKET}/artifacts/ --region ${AWS_REGION}'
              }
            }
        }
        
        stage('code-Deploy'){
            steps{
                sh 'docker build -t image-one .'
                sh 'docker run -itd --name cont-1 -p 8089:8081 image-one '
            }
        }
    }
}
