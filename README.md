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

- Create key pair

    A key pair, consisting of a public key and a private key, is a set of security credentials that are used to prove your identity when connecting to an Amazon EC2 instance.
    To create the key pair for instances, we will use the command:
    ```
    aws ec2 create-key-pair --key-name ci-cd-keypair --query 'KeyMaterial' --output text> ci-cd-keypair.pem
    ```
    After that, it will generate the RSA PRIVATE KEY inside the ci-cd-keypair.pem:
    
    ![keypair](./images/keypair-cli.png)

    On the other hand, your key pair also appears in the AWS Console:
    ![keypair-console](./images/keypair-console.png)
    
- Create Security Group
    
- Create two EC2 instances
- 
### Setting up Instances
  
### Integrating SonarQube for Jenkins
  
### Create Jenkins for Deploying Web Application
  

