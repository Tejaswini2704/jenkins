# launch EC2 instance
- launch ec2 instance.
- install java and jenkins.
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
- go to manage jenkins
- go to plugins and click on **available pligins**
- search **"maven integration"** and install it.
- now go to **Tools** and click on **add maven**
   - name : maven
   - apply and save
- add git credentials.
 # build pipeline
- click on **new item**
  - name : studentapp
  - item type : pipeline
  - write down the script
  ```
    pipeline {
    agent any
    stages {
        stage ('code-pull'){
            steps {
        }
    }
  ```
- click on pipeline syntax
- sample step: Git:git
- Repository URL :https://github.com/abhipraydhoble/Student-App-Code.git
- branch : main
- credentials: our git credentials
- click on generate pipeline syntax.
- copy the syntax and paste it on the pipeline code
```
pipeline {
    agent any
    stages {
        stage ('code-pull'){
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/abhipraydhoble/Student-App-Code.git'
            }
        }
    }
}
```
- apply and save.
- click on build now.
- check on our ec2 instance, if the code is pulled or not.
- now edit the code by using the **configure** tab
```
pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages {
        stage ('code-pull'){
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/abhipraydhoble/Student-App-Code.git'
            }
        }
        stage ('code-build'){
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```
- apply and save, click on build now.
- check if target directory is created or not.
![Screenshot 2025-06-10 123304](https://github.com/user-attachments/assets/e1ef2160-2aa4-42de-9539-3d9610f0a0f1)

