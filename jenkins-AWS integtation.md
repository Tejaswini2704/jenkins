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
        stage ('code-build'){
            steps{
                sh 'mvn clean package'
            }
        }
    }
    
}
```
- 
