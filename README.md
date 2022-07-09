![Amazon-AWS-5-Certifications](https://user-images.githubusercontent.com/72511276/178106396-f97a9e4d-da49-4042-b780-47da9eb8d117.jpg)
# Web Application Using AWS

This Projects works with creating a web application on Amazon cloud using the Concepts of Clouding computing like Creating an IAM Role,VPC,S3 Bucket soon. This project the  illustrates every single step performed while creating the Project.

You can also follow along the way. 

The only thing you need to perform this project is having a Amazon Account. If don't have it, Hurry up follow this link to create one and its free :heart_eyes:
[AWS CREATE ACCOUNT](https://www.aws.amazon.com/free/)

## Table of Content

- [About the project](#about-the-project)
- [Stage-1 Creating an IAM role](#stage-1-creating-an-iam-role)
- [Stage-2 Lanching an EC2 User](#stage-2-lanching-an-ec2-user)
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


### Stage-2 Lanching an EC2 User


### Stage-3 Creating a VPC


### Stage-4 Creating an S3 Bucket


### Stage-5 Creating a Dynamo Db Database

### Stage-6 Creating a Auto scaling Group

### Stage-7 Cleaning Up the Resources
