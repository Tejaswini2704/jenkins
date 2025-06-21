# Jenkins -AWS EKS setup
## Launch EC2 instance 
- install jenkins and Java
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
- install terraform
```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```
- setup jenkins
## Plugins
- install folowing plugins
- go to manage jenkins -> plugins -> available plugins
```
Stage view
```
```
aws credentials
```
```
Pipeline: AWS Steps
```
```
terraform
```
- go to manage jenkins -> credentials  -> add global credentials
- add aws credentials
   - kind: aws credentials
   - add access key and secret key.
- go to manage jenlins -> tools -> add terraform
   - name: tf
   - install automatically 
- click on "new item"
   - name: jenkins-eks
   - type: pipeline
```
pipeline{
    agent any
    tools{
        terraform 'terraform'
    }
    stages{
        stage ('code-pull'){
            steps{
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/abhipraydhoble/jenkins-tf.git'
            }
        }
        stage ('eks-cluster'){
           steps{
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
    sh 'terraform init'
    sh 'terraform validate'
    sh 'terraform plan'
    sh 'terraform apply -auto-approve'
}
           }
        }
    }
}
```
