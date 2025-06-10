# setup 
## Launch two sever with following configuration.
- **AMI** - Ubuntu
- **Instance Type** - t2.medium.
- **Security Group** - add port 22 (ssh) and 8080
- **Storage** - 30 GB
- launch both the instances.
## Connect the instances using ssh method
- in master server generate the key pairs using following commands.
```
ssh-keygen
ls -a
cd .ssh
ls
cat id_ed25519.pub (ublic key)
```
- copy the public key and paste it on agent server using following commands
```
ls -a
cd .ssh
vim authorized_keys
```
paste the public key here and save.
## Jenkins setup
- in master server install jenkins and java using following commands
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
- copy the IP adresscof master server and paste it on browser wtih 8080 port (eg. 16.171.59.135:8080  ,(IP_address:8080)).
- setup jenkins.
- go manage jenkins , go to credential.
- create global credential
- add git, docker crenetial.
### now add agent credential
- kind : SSH username with private key
- username : ubuntu
- private key : select enter directly
- paste the private key of master server.
- click on create .
![Screenshot 2025-06-06 160422](https://github.com/user-attachments/assets/2ce15bfa-95ec-4479-ae9d-39a9515fede4)

- Now go to agent aerver and install java
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
```
- create a folder.
```
mkdir jenkins
```
- give permissions.
```
chmod 777 jenkins
```
- now go to jenkins and manage jenkins
-  go to nodes click on new node
![Screenshot 2025-06-06 160959](https://github.com/user-attachments/assets/b6990fd3-8085-4efe-b381-95dd3244dded)
- node name : node-1
- Type : permanent agent - create
- number of executer : 1
- remote root directory : location of directory we created (/home/ubuntu/jenkins)
- label : node-1
- launch method : launch via ssh
- Host : IP address of agent server
- Credential : agent-cred
- host key verification strategy  : non-verifying verification strategy

![Screenshot 2025-06-06 162340](https://github.com/user-attachments/assets/68617e56-0a39-4dfe-8ad3-a8f7f253b9f3)
![Screenshot 2025-06-06 163034](https://github.com/user-attachments/assets/78e66cab-9924-437f-b3a4-985a16845b1b)

