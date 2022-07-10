![Amazon-AWS-5-Certifications](https://user-images.githubusercontent.com/72511276/178106396-f97a9e4d-da49-4042-b780-47da9eb8d117.jpg)
# Web Application Using AWS

This Projects works with creating a web application on Amazon cloud using the Concepts of Clouding computing like Creating an IAM Role,VPC,S3 Bucket soon. This project the  illustrates every single step performed while creating the Project.

You can also follow along the way. 

The only thing you need to perform this project is having a Amazon Account. If don't have it, Hurry up follow this link to create one and its free :heart_eyes:
[AWS CREATE ACCOUNT](https://www.aws.amazon.com/free/)

## Table of Content

- [About the project](#about-the-project)
- [Stage-1 Creating an IAM role](#stage-1-creating-an-iam-role)
- [Stage-2 Lanching an EC2 Instance](#stage-2-lanching-an-ec2-instance)
- [Stage-3 Creating a VPC](#stage-3-creating-a-vpc)
- [Stage-4 Creating an S3 Bucket](#stage-4-creating-an-s3-bucket)
- [Stage-5 Creating a Dynamo Db Database](#stage-5-creating-a-dynamo-db-database)
- [Stage-6 Creating a Auto scaling Group](#stage-6-creating-a-auto-scaling-group)
- [Stage-7 Cleaning Up the Resources](#stage-7-cleaning-up-the-resources)

### About the project
---

We are going to build a `Employee Directory Application` by creating a multi subnet VPC for hosting a App-data base (Dynamo Db), S3 bucket to store media files  and creating a auto scaling group using Cloud monitoring.

We are going to use  EC2 as compute engine to load up the Flask application code in the user data channel before lanching the instances.

In order to connect this serves to the compute instances we are going to setup a `IAM role` from the aws policies.

### Stage-1 Creating an IAM role

>**Creating an IAM role**

- Make sure that you are  logined as `IAM user` rather than root user.
- Keep a eye on the region your are working on make sure your are using the services that are located in the same region.
- Search for `IAM` in the search bar.

![image](https://user-images.githubusercontent.com/72511276/178115615-f2c9e4d3-ac8c-4eb9-aae8-77bf88337c5e.png)

- You will be redirected to this page

![Untitled](https://user-images.githubusercontent.com/72511276/178116361-363cb595-01b6-449e-ab78-966215ea3a06.png)

- After going into  roles click on create role

![image](https://user-images.githubusercontent.com/72511276/178116563-1e4ba505-2b9e-4f0a-ada3-1b61d81c588d.png)

> Creating a role

- select the desired trusted entry type from the pool of options.
          - Select `AWS services` 
          - In the AWS Services select `EC2` as the Use case then click on next.

![image](https://user-images.githubusercontent.com/72511276/178116972-aa14b8b3-0f6b-430e-bc1a-5b607de7a66d.png)

- we need to assign the permissions policies of  the services to the role in order to do so search for `amazons3full` in the filter policies search bar and choose 
`AmazonS3FullAccess`.

![image](https://user-images.githubusercontent.com/72511276/178117354-9cebdc02-2b04-40b7-8336-432265ef473f.png)

- Search for `amazonDynamodb` in the filter policies search bar and choose `AmazonDynamoDBFullAcess` then click on next.

![image](https://user-images.githubusercontent.com/72511276/178117441-da061076-17b6-4e82-8e06-a5eccc58895b.png)

- Give name for the role and provide a on point description for the role, then add a tag( optional-name) for the role then click on ` create role`.

![image](https://user-images.githubusercontent.com/72511276/178117604-2c252258-0e18-480f-a467-ea5b1a43ab12.png)
![image](https://user-images.githubusercontent.com/72511276/178117667-858bc2ce-37a7-4771-aef8-10d722f620ef.png)

**NOTE:** Using full access policies not a best practise to be followed. we can fine to this policies to  more specific and restrictive permissions.

![image](https://user-images.githubusercontent.com/72511276/178117903-9822166e-adfb-4109-932b-36e48a9823dc.png)


### Stage-2 Lanching an EC2 Instance

To host a Employee Directory application we need to setup a compute enginne in this case we are using a `EC2 Virtual machine`.

> Types of compute services
- Instances (Virtual Machines) *Ex* - EC2
- Serverless *Ex* - Lambda
- Containers *Ex* - ECS and EKS

-----
#### Setup a EC2 Instance
------

Every EC2 instances needs to exist inside a Network and on AWS. this are called VPC (Virtual Private Network) we are going to set up the VPC in the next stage for now we are going to lanch it on the default aws network provided by the aws.

- Search EC2 in the search bar then click on EC2 this will redirect to the EC2 Dashboard.

![image](https://user-images.githubusercontent.com/72511276/178132681-5c904a68-e2e3-424a-b8f7-9cda3b5da2ac.png)

**NOTE :**Keep an Eye on the region your are  lanching your services. (US East (N. Virginia))

- Click on Instances to lanch the instance 

![image](https://user-images.githubusercontent.com/72511276/178132830-3c64bdf9-fe48-43c5-bac3-c58a7b8ee34a.png)

- Click on Lanch instance

![image](https://user-images.githubusercontent.com/72511276/178132946-01c0a838-50aa-4faf-9c60-ec9f10544d78.png)

- You will be redirected to the lanch setup Dashboard which looks something like this 

![image](https://user-images.githubusercontent.com/72511276/178133014-a5307b5c-2fb6-4178-a009-40000b15b046.png)

This setup contains `7 stages`

1. Choose AMI
   - **Step 1: Choose an Amazon Machine Image (AMI)**
   - An AMI is a template that contains the software configuration  required to launch your instance.
   - You can select an AMI provided by
     - [x] AWS
     - [ ] Our user community 
     - [ ] AWS Marketplace
     - [ ] You can select one of your own AMIs.
   - We are going to select the AMI that was provided by the AWS. which will be under the `free tier`
   - select `Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type` 
   ![image](https://user-images.githubusercontent.com/72511276/178133495-32a3ac53-8f16-4c1d-9df2-94a14a25a099.png)
    
2. Choose Instance Type

- Amazon EC2 provides a wide selection of instance types optimized to fit different use cases
- They have varying combinations of CPU, memory, storage, and networking capacity, and give you the flexibility to choose the appropriate mix of resources for your applications. 

![image](https://user-images.githubusercontent.com/72511276/178133810-073238d2-7bc5-49cb-b3b1-3a96e618d1d4.png)

- We are going to use t2.micro instance as the instance type (which is under free tier) then click on Next 

3. Configure Instance
- In this section we are going to configure instance to suit your requirements
![image](https://user-images.githubusercontent.com/72511276/178134051-7d8b4d94-e21a-41d5-a8a5-126d3c76fa14.png)
- we can choose the network type in which the instance load and subnet in the network for now we are going to use the default network.
- Drag down to connect the IAM role to the instance.
![image](https://user-images.githubusercontent.com/72511276/178134106-bfe3792e-9325-42ba-8b52-6dbde26d4c14.png)
- select the created IAM role (RoleforWEBAPP)
- Drag down to the advanced options and fill the userdata with the following code.

```bash
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install python3 mysql
pip3 install -r requirements.txt
amazon-linux-extras install epel
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=<us-east-1>
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```

![image](https://user-images.githubusercontent.com/72511276/178134179-7708f675-8257-44c4-82ba-cd71427559f6.png)
- then click on next to add storage.

```
Change the following line to match your region:

Note: You can find this at the top right next to your user name.  

export AWS_DEFAULT_REGION=<INSERT REGION HERE>
Example:

Note: US West (Oregon) 

export AWS_DEFAULT_REGION=us-east-1

Note: You will modify this User Data script again to use your Amazon S3 bucket  in a later lab. For now, just leave the ${SUB_PHOTOS_BUCKET} in the script.
```

4. Add Storage

- we didn't need to add any storage element for now so click on next to add tags.

5. Add Tags

Click on `Add tag` button to add a tag 

- Type `Name` in the key place holder and give the desired name example : `WEBAPP_INSTANCE` then Click on next 

![image](https://user-images.githubusercontent.com/72511276/178136539-caea98e8-faa7-4ca9-a0c9-c77b8bb64e54.png)

6. Configure Security Group

>A security group is a set of firewall rules that control the traffic for your instance.

- You can add rules to allow specific traffic to reach your instance. For example, if you want to set up a web server and allow Internet traffic to reach your instance, add rules that allow unrestricted access to the HTTP and HTTPS ports. 

![image](https://user-images.githubusercontent.com/72511276/178137264-2dedac57-4b97-4be5-b416-ccf602d1d581.png)


- You can create a new security group or select an existing security group
- For now we are going to create a new security group
  - Click on create a new security group
  - Give name to the security group in the place holder example `app-sg`
  - Discription for the same example `app security group`
  - At last we are going to add rules
  - Rules are the one who are going to define the traffic to the instance 
  - we are going to allow HTTP connection from every source.
  - Before that choose X at the right to remove the SSH access
  - Click on add rule.
  
  |Type   | Protocol | Port Range | Source      |
  |-------|----------|------------|-------------|
  |HTTP   | TPC      | 80         | Anywhere    |
  
  - Click on review and lanch.
  
7. Review

- Review the stages and click on lanch 
- You will be forward with this interface

![image](https://user-images.githubusercontent.com/72511276/178137396-7907faa2-f919-4a2e-87ea-983b9da9d242.png)

> A Key pair consist of a public key that AWS stores, and a private key file that we store. Together , they allow you to connect your instance securely.

**NOTE**: we need to download the private key file ( .pem file ) because this will you to connect to your instance you will not be able to download this file again so store it in a secure place.

- After lanching the instance you will be redirected to the lanch status 

![image](https://user-images.githubusercontent.com/72511276/178137622-d2777127-da83-4ff8-9598-2ed20c24de5c.png)

- now your instances are up and running you can click on view instance to view the instance details.

![image](https://user-images.githubusercontent.com/72511276/178137711-87b54721-babc-4d7b-b0d1-1bf9545f5f61.png)

- After the Status check are passed you can connect to your instances.


Next to Name, choose the checkbox to select the instance.  Under the Details tab, copy down the Public IPv4 address. 

Paste it into a new browser tab. You should see a Employee Directory placeholder. Right now you will not be able to interact with it as it's not currently connected to our DynamoDB database. 

Congrats! You've successfully created an EC2 instance hosting the employee directory application. After you've finished looking around, it's time to stop and terminate your instance, so that you don't incur future costs. 

Back in the AWS Management Console, the employee-directory-app should still be selected. Now, choose Instance state at the top and choose Stop instance. Choose Stop. The Instance state will eventually go into the Stopped state. 

Next, you will terminate the instance. Again, select the checkbox next to the instance Name. Choose Instance state and choose Terminate instance. Choose Terminate. 









### Stage-3 Creating a VPC


### Stage-4 Creating an S3 Bucket


### Stage-5 Creating a Dynamo Db Database

### Stage-6 Creating a Auto scaling Group

### Stage-7 Cleaning Up the Resources
