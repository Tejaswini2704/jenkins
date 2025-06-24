# Setup
- Launch 2 ec2 instance
   - name: jenkins and eks-cluster
## in eks-cluster
- connect the eks-cluster instance
-  install eksctl CLI tool to create ejs cluster
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```
- move eksctl setup to /usr/local/bin directory
```
sudo mv /tmp/eksctl /usr/local/bin
```
-  To check eksctl version
```
eksctl version
```
- Install kubectl Download the latest release with the command
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
- install kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
- grant permissions
```
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```
- to check the version
```
kubectl version --client
```
- Install AWS CLI on Ubuntu

- Download the aws cli bundle using below command
```
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
### congigure the AWS CLI
- To connect AWS using CLI we have configure AWS user using below command
```
aws configure
```
### create eks cluster
```
eksctl create cluster --name my-ekscluster --region ap-southeast-1 --version 1.32 --nodegroup-name linux-nodes --node-type t2.medium --nodes 2
```
- once the cluster is created, update the eks-cluster
```
aws eks update-kubeconfig --name my-ekscluster
```
- check the nodes
```
kubectl get nodes
```
## create IAM role
- create a IAM role and attach it to jenkins server and eks
- go to IAM -> Roles -> create role
   - trusted intity: AWS service
   - use case : EC2
- add permissions :
   - AdministratorAccess
   - AWSCloudFormationFullAccess
- role name: jenkins-eks
- create role.
- attach to both EC2 instances
   - go to ec2 -> select instnace -> actions -> security -> modify IAM role -> select "jenkins-eks" role -> click on update role.

## connect the jenkins server
- go to EC2 and connect he jenkins server
- install jenkins
```
sudo apt update -y
sudo apt install fontconfig openjdk-21-jre -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
- log in to jenkins
- install plugins
- manage jenkins -> plugins -> available plugins
```
aws credentials
```
```
aws steps
```
```
stage view
```
- add aws credentiasl
- manage jenkins ->credentials -> global -> kind : aws credentials
- add aws credentials

- install docker in jenkins server
```
curl -fsSL get.docker.com | /bin/bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo docker version
```
- install AWS CLI in jenkins instnace
```
sudo apt install unzip 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```
- install kubectl in jenkins instance
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```
