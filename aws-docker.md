# setup
- launch instancce
- follow the jenkins-s3 file

- install plugins
```
docker
```
```
pipeline {
    agent any 
    tools {
        maven 'maven'
    }
    
    environment{
        S3_BUCKET = "studentapp-project-artifact" 
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
```
