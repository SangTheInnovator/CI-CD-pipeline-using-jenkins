# Build a CI/CD pipeline using Jenkins

![images_ci-cd](./images/CI_CD.png)

## Introduction
In this project, we will use different tools such as Jenkins, SonaQube, Docker, and AWS EC2 to build an automated CI/CD pipeline for a web application. This pipeline includes build, testing, and deploying the web application on AWS EC2 instances.

## Build and setup
1. Create EC2 Instances
2. Setting up Instances
3. Integrating SonarQube for Jenkins
4. Create a Jenkins for Deploying Web Application
   - Configuring Pipeline for SonarQube
   - Configuring Pipeline for Docker

### Create EC2 Instances
In this step, we will use [AWS CLI](https://docs.aws.amazon.com/pdfs/cli/latest/userguide/aws-cli.pdf) to create the EC2 Instances.

- **Create key pair**

A key pair, consisting of a public key and a private key, is a set of security credentials that are used to prove your identity when connecting to an Amazon EC2 instance.
      
To create the key pair for instances, we will use the command:
```
aws ec2 create-key-pair --key-name <keypair-Name> --query 'KeyMaterial' --output text> <keypair-Name.pem>
```

After that, it will generate the RSA PRIVATE KEY inside the .pem file:
    
![keypair](./images/keypair-cli.png)

On the other hand, your PUBLIC KEY appears in the AWS Console:
  
![keypair-console](./images/keypair-console.png)

    
- **Create Security Group**

A security group controls the inbound traffic that's allowed to reach the instances associated with the security group and the outbound traffic that's allowed to leave them.

By using this command, we will create 2 security groups for 2 instances:

```
aws ec2 create-security-group --group-name <security grp Name> --description "<Description>"
```

![security-cli](./images/security-group-cli.png)


![security-console](./images/security-group-console.png)

2 security groups were not set up for the inbound traffic. Therefore, for each security group, we need to set up different rules:

In the first instance:

![instance1-security](./images/instance1-security.png)

In the second instance:

![instance2-security](./images/instance2-security.png)


- **Create two EC2 instances**

The following command will launch the instance:

```
aws ec2 run-instances --image-id <ami-Id> --count 1 --instance-type <type> --keyname <keypair-Name> --security-groups <security grp Name>
```

After creating 2 instances, we should add the tags:
```
aws ec2 create-tags --resources <Instance-Id>--tags Key=Name,Value=<value>
```

![instances-console](./images/instances-console.png)

### Setting up Instances

In this step, we will install Jenkins and Docker on 1st instance and install SonarQube on 2nd instance

To install Jenkins, Docker, and SonarQube on instances, we have to ssh to your instance in your terminal by using command:

```
ssh -i <keypair.pem> ec2-user@<public-ip-v4>
```

- For the first instance:
  
Install Jenkins:
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
Copy the public IPv4 address of instance 1, paste it into your browser, and put the port number 8080. We will see:

![jenkins-unlock](./images/unlock-jenkins.png)

To unlock Jenkins paste the secret key to Administrator password, get the secret key through this command:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

After unlocking Jenkins click on "Install suggested plugins" and then provide the basic details on the "Create First Admin User" page and get to the Jenkins dashboard after completing all the required steps.

![jenkins-dashboard](./images/jenkins-dashboard.png)


Install Docker by using the following commands:

```
sudo apt-get install docker.io
```

- For the second instance:

To install SonarQube to your instance run the following command:
```
sudo apt update
sudo apt install openjdk-17-jre 
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.0.0.68432.zip

sudo apt install unzip -y
unzip sonarqube-10.0.0.68432.zip
cd /home/ubuntu/sonarqube-10.0.0.68432/bin/linux-x86-64
./sonar.sh start
```
Copy the Public Ipv4 of SonarQube instance and paste it on the browser and use port 9000. And login with username: admin and password: admin. 
After login successfully, we will see the dashboard of sonarqube

![sonar-dashboard](./images/sonarqube-dashboard.png)

### Integrating SonarQube for Jenkins
  
### Create Jenkins for Deploying Web Application
  

