# setup 
- Launch EC2 instance
- install java and jenkins
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
- log in to jenkins
## Plugins
- go to manage jenkins -> plugins -> available plugins
- install following plugins
```
AWS credentials
```
```
S3 publisher
```
```
Maven integration
```
- go to manage jenkins -> Tools
- add Maven
  - name: maven
  - install automatically
  - apply and save
- go to credentials -> global
- add credentials
  - kind : AWS credentials
  - ID : aws-cred
  - desp :aws-cred
  - access_key : aws access key
  - secret_key : aws secret key
  - create
-  click new item
  - name : jenkins-s3
  - type: pipeline  
- pipeline code:
- stage 1  :code-pull
```
pipeline{
    agent any
    tools{
        maven 'maven'
    }
    
    stages {
        stage ('code-pull'){
            steps{
                 git branch: 'main', url: 'https://github.com/abhipraydhoble/Student-App-Code.git'
            }
        }
    }
    
}
```
- apply,save and build now.
- note for steps code use pipeline syntax to genrate the code.
   - sample : Git:git
   - repo URL : ``https://github.com/abhipraydhoble/Student-App-Code.git``
   - branch : main.
   - genrate the code and paste it on steps section.
- stage 2 : code-build
- now now configure the code.
- make following changes in the code.
```
pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage ('code-pull'){
            steps{
                git branch: 'main', url: 'https://github.com/abhipraydhoble/Student-App-Code.git'
            }
        }
        
        stage ('code-build'){
           steps{
               sh 'mvn clean package'
           }
        }
    }
}
```
- apply ,save and bild now
- click on build number -> go to console uotput -> go to workspace folder -> click on /var/lib/jenkins/workspace/jenkins-s3 -> go to /target folder -> you will see "studentapp-2.2-SNAPSHOT.war" file.
- now we have to add this ``.war`` file into S3 bucket.
## create S3 bucket
   - name : studentapp-project-artifact
   - enable ACL
   - remove the block
   - enable bucket versioning
   - create bucket.
- install AWS ClI in the instance
- install unzip
```
sudo apt install unzip -y
```
- aws CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```
- - back to jenkins.
- add environment variable
- and generate pipelin syntax
   - sample step : withCredentials : bind credentials to variable
   - binding : AWS access key and secret key
   - generate pipeline code.
- final code
```
pipeline {
    agent any
    tools{
        maven 'maven'
    }
    environment{
        S3_BUCKET ="studentapp-project-artifact"
        AWS_REGION = "ap-southeast-1"
        warFile = "target/studentapp-2.2-SNAPSHOT.war"
        }
    
    stages{
        stage ('code-pull'){
            steps{
                git branch: 'main', url: 'https://github.com/abhipraydhoble/Student-App-Code.git'
            }
        }
        
        stage ('code-build'){
           steps{
               sh 'mvn clean package'
           }
        }
        
        stage ('upload-artifact'){
            steps{
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                 sh 'aws s3 cp ${warFile} s3://${S3_BUCKET}/artifact/ --region ${AWS_REGION}'
              }
            }
        }
    }
}
```
- go to aws -> check the s3 bucket -> artifact folder will be there, with "studentapp-2.2-SNAPSHOT.war" file
![Screenshot 2025-06-17 155204](https://github.com/user-attachments/assets/82fec3d7-a0f3-459d-8a81-83d74699d60d).


