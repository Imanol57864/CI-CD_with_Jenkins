# Jenkins Pipeline (my-take)
This repo contains some scripts from SampleApp_Linux and the AWS CloudFormation template to launch a Jenkins pipeline in AWS.

(I changed the AMIs on the template to allow access to instances from EC2 Instance Connect)

Just a simple, highly available, automated deployment web application.

If you want to replicate this pipeline, follow the steps below.
![Architecture 1](https://user-images.githubusercontent.com/67908214/156058092-5e417cd4-ec8c-4fd7-8009-33be469d5cc1.png)

## Step 1: Configure the Jenkins server
### Note: the CF launches an ALB + ASG, the only way to http is via the ALB DNS
1. Run the CF template and configure the Jenkins server. 
(Remember to open port 8080, and access via http DNS:8080) (You will find a folder directory)
2. Access the Jenkins Server instace via EC2 Instance Connect and run
> $ sudo su

> $ cat [the directory you found]
3. Paste the password you found on Jenkins
4. Install suggested plugins, and then GitHub Integration Plugin + AWS CodeDeploy Plugin for Jenkins

## Step 2: Configure the Github Repository
1. Simple, have your own repo with these files.
2. Create a Webhooks by clicking on Settings inside your repo
3. The Payload URL is -> DNS:8080/github-webhook/
4. Application/json
5. Just push event + Active 

## Step 3: Configure the CodeDeploy Deployment
1. On the Jenkis GUI, create a new job
2. Check Github project and add the repo url
3. Check Source Code from Git and add the Git Clone url from the repository
4. Check GitHub hook trigger for GITScm polling
5. Lastly, add the action Deploy an Application to AWS CodeDeploy
(Fill the 1st 5 boxes with the info from AWS CodeDeploy and the Bucket created by S3)
(Check "Use Access/Secret keys" and use some keys from an IAM User)

## Test your pipeline
Go to your github repo and play with the index file, check if your changes where automatically deployed to your instances by doing http to the ALB DNS
