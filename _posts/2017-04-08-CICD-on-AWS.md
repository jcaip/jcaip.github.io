---
layout: post
title: CICD on AWS

images:

    - url: /images/cicd/cicd_cover.png
      alt: CICD
      title: CICD
---

## Using Jenkins and Docker to easily deploy web applications

### Dockerized Web Application
Dockerizing your web applications can be kind of confusing. I'm going to use a sample web application that uses Flask and React that you can find [here](https://github.com/jcaip/react_flask_dockerized).

There's more documentation about this project [here]().

### Configuring AWS

First we need to create a new EC2 instance. Log in to your AWS dashboard and then create a new EC2 instance per your own specifications.

When you review your Instance Launch, change the Security Group settings.
![page](/images/cicd/aws_instance.png)

Add a rule to allow traffic on port 8080 (This is the defualt port that Jenkins uses). You should also add a rule for HTTP/HTTPS traffic and any other ports you may need. 
![settings](/images/cicd/security_group_after.png)

After this, you'll be asked to either use an exsisting key pair or generate a new key pair, if this is your first instance. In any case, be sure to have a copy of the physical key file on hand. 
We need to ssh into your instance. You can by clicking on the instance and then clicking "Connect" and following the commands that pop up.
There's more documenation [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

Run the following commands, which install jenkins & docker, makes them start on boot, and configures some user settings

```bash
    sudo yum update -y
    sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
    sudo yum update -y

    sudo yum install jenkins docker -y
    sudo service jenkins start
    sudo service docker start

    sudo usermod -a -G docker jenkins
    sudo usermod -a -G docker ec2-user
```

Alternatively, you can run 
```bash 
    curl https://gist.githubusercontent.com/jcaip/3638eb4b25c36d62b329942e493a1f2d/raw/b021640532b5e7fee8ce6e4ba6da66ec00e448a2/run.sh | bash
```
which will pull this script from my github and then run it.

### Configuring Jenkins
You may have to restart the AWS instances for the user group changes to take place. 

Next access your Jenkins page by going to http://<your_server_public_DNS>:8080
If you are unable to connect to the Jenkins page make sure that the Security Group settings are correct.
You'll have to enter the password located at /var/lib/jenkins/secrets/initialAdminPassword and then create a user account.

You should be prompted to install plugins at this point. Installing just the suggested plugins should be fine. 

Now configure a build job in Jenkins. Pointing it towards your GitHub repository. 
For your build commands. you should just be able to put make. 
