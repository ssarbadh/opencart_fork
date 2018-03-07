# OpenCart

## Overview

OpenCart is a free open source ecommerce platform for online merchants. OpenCart provides a professional and reliable foundation from which to build a successful online store.

## Purpose of this fork is to integrate it with  Code-deploy to have the opencart software deployed fresh via AWS Code-deploy.  

Read the instructions for setting up the end machine for deployment.

 0. Create a instance profile ( or role ) to use in ec2 instance for code-deploy

     Role name: CodeDeploySampleStack-829sf-InstanceRole-1SRRS68NCTZ0D
	 
	 Policy Document (JSON):
	 {
		"Statement": [
			{
				"Action": [
					"autoscaling:Describe*",
					"cloudformation:Describe*",
					"cloudformation:GetTemplate",
					"s3:Get*",
					"s3:List*"
				],
				"Resource": "*",
				"Effect": "Allow"
			}
		]
	}
	 
This will ensure seamless intergration with the shared AMI (if used )
	 
 1. Create a LAMP Stack machine on linux out of Community AMI [Bitnami](https://docs.bitnami.com/aws/infrastructure/lamp/)
 2. Put the following into user-data to have the necessary 
    a. Code-deploy agent 
	b. AWS cli be installed 
	
	#!/bin/bash
	apt-get -y update
	apt-get -y install ruby
    apt-get -y install wget
	apt-get -y install awscli
	cd /home/ubuntu
*	wget https://bucket-name.s3.amazonaws.com/latest/install
	chmod +x ./install
	./install auto

* Find the correct [bucket-name](https://docs.aws.amazon.com/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names) and subsitute here 	
 4. After machine loads up, login to the machine and check if code-deploy agent is running.
	
	sudo service codedeploy-agent status
	
 5. Also check the agent log to ensure nothing is broken.
 
    less /var/log/aws/codedeploy-agent/codedeploy-agent.log
	
 6. If successfully connected, it will show 
 
     2018-03-07 12:10:00 INFO  [codedeploy-agent(1025)]: Version file found in /opt/codedeploy-agent/.version with agent version OFFICIAL_1.0-1.1352_deb.
2018-03-07 12:11:01 INFO  [codedeploy-agent(1025)]: [Aws::CodeDeployCommand::Client 200 61.026193 0 retries] poll_host_command(host_identifier:"arn:a
ws:ec2:ap-south-1:122213125690:instance/i-0083c7f372c858189")

Or,
Use this shared AMI to create a machine that is pre-configured with requisites. (will share it soon)
Still have to create a role for instance. 

