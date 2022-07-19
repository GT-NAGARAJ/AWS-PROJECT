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
- Instances (Virtual Machines) Ex - EC2
- Serverless Ex - Lambda
- Containers Ex - ECS and EKS

-----
#### Setup a EC2 Instance
------

Every EC2 instances needs to exist inside a Network and on AWS. this are called VPC (Virtual Private Network) we are going to set up the VPC in the next stage for now we are going to lanch it on the default aws network provided by the aws.

- Search EC2 in the search bar then click on EC2 this will redirect to the EC2 Dashboard.

![image](https://user-images.githubusercontent.com/72511276/178132681-5c904a68-e2e3-424a-b8f7-9cda3b5da2ac.png)

**NOTE**: Keep an Eye on the region your are  lanching your services. (US East (N. Virginia))

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


- Next to Name, choose the checkbox to select the instance.
- Under the Details tab, copy down the Public IPv4 address. 

Paste it into a new browser tab. You should see a Employee Directory placeholder. Right now you will not be able to interact with it as it's not currently connected to our DynamoDB database. 
Explicit (i.e defined by the user ) 
Congrats! You've successfully created an EC2 instance hosting the employee directory application. After you've finished looking around, it's time to stop and terminate your instance, so that you don't incur future costs. 


### Explore the EC2 Instance Lifecycle.

An EC2 instance transitions between different states from the moment you create it all the way through to its termination.

![vgBOJqw6TMyATiasOizM8g_8d882217f0044106a24b7d6c69549207_image-11-](https://user-images.githubusercontent.com/72511276/178268333-828b22a6-d2cc-4942-a8c9-ff8fe7478eff.png)

> When you launch an instance, it enters the `pending state (1)`. 

- When the instance is pending, billing has not started. 
- At this stage, the instance is preparing to enter the running state. 
- Pending is where AWS performs all actions needed to set up an instance, such as copying the AMI content to the root device and allocating the necessary networking components.

> When your instance is `running (2)`.

- It's ready to use. This is also the stage where billing begins. As soon as an instance is running, you are then able to take other actions on the instance, such as reboot, terminate, stop, and stop-hibernate.

> When you `reboot an instance (3)`.

- It’s different than performing a stop action and then a start action. 
- Rebooting an instance is equivalent to rebooting an operating system. 
- The instance remains on the same host computer and maintains its public and private IP address, and any data on its instance store.

It typically takes a few minutes for the reboot to complete.

> When you `stop` and `start` an instance (4)

- Your instance may be placed on a new underlying physical server. 
- Therefore, you lose any data on the instance store that were on the previous host computer. 
- When you stop an instance, the instance gets a new public IP address but maintains the same private IP address.

> When you `terminate an instance` (5).

- The instance store are erased, and you lose both the public IP address and private IP address of the machine. 
- Termination of an instance means you can no longer access the machine.


| Instance state | Description                   | Instance usage billing |
|----------------|-------------------------------|------------------------|
| pending        |  The instance is preparing to enter the running state. An instance enters the pending state when it launches for the first time, or when it is started after being in the stopped state                    | Not billed             |
| running        | The instance is running and ready for use.                  | Billed             |
| stopping       |  The instance is preparing to be stopped or stop-hibernated.| Not billed if preparing to stop Billed if preparing to hibernate           |
| stopped        |  The instance is shut down and cannot be used. The instance can be started at any time.                    | Not billed             |
| terminated     |  The instance has been permanently deleted and cannot be started.                   | Not billed             |


### Stopping a EC2 Instance

![image](https://user-images.githubusercontent.com/72511276/178265838-ccdf948e-143f-4591-a90b-2a7cc72cc66c.png)


- Back in the AWS Management Console, the employee-directory-app should still be selected. 
- Now, choose Instance state at the top and choose Stop instance. 
- Choose Stop. 
- The Instance state will eventually go into the Stopped state. 

![image](https://user-images.githubusercontent.com/72511276/178266104-bd49a3dd-79d5-41fe-9137-9904be5c8a5c.png)

- Next, you will terminate the instance. 
- Again, select the checkbox next to the instance Name. 
- Choose Instance state and choose Terminate instance. Choose Terminate. 

![image](https://user-images.githubusercontent.com/72511276/178266376-abbdc283-9594-48aa-bd44-fad683ff21b3.png)

![image](https://user-images.githubusercontent.com/72511276/178271777-034e61ad-2dd9-4d8d-bf33-74fd26a9343d.png)


### Stage-3 Creating a VPC

## Table of Content

- [About the VPC](#about-the-vpc)
  - [Creating a VPC](#creating-a-vpc)
- [About the Subnets](#about-the-subnets)
  - [Creating the Subnets](#creating-the-subnets)
- [About Internet Gateway](#about-internet-gateway)
  - [Creating Internet Gateway](#creating-internet-gateway)
- [About Route Table](#about-route-table)
  - [Creating a Route Table](#creating-a-route-able)
    

## About the VPC

- A VPC is an isolated network you create in the AWS cloud, similar to a traditional network in a data center. When you create a VPC, you need to choose three main things. 

  - The  name of your VPC.

  - A Region for  your VPC to live in. Each VPC spans multiple Availability Zones within the  Region you choose.

  - A IP range for your VPC in CIDR notation. This determines the size of your network. Each VPC can  have up to four /16 IP ranges.

![kE8VCk32RPiPFQpN9kT4Lw_af022fa3bad0433d9b8b3e6d90064abb_introVPC_1 (1)](https://user-images.githubusercontent.com/72511276/178143410-eeee7d2b-b898-42bf-921a-f7bb24c1a320.jpeg)

## Creating a VPC

![image](https://user-images.githubusercontent.com/72511276/178145841-97abe546-bc0c-40a6-81ca-d4f5dc5db4ba.png)

- Search for VPC in the search bar at the top. Choose VPC.
- 
![image](https://user-images.githubusercontent.com/72511276/178145792-6501b8c7-ac07-40d9-9a2e-ec1ba71c1453.png)

- Choose Your VPCs in the left panel. 
- Choose Create VPC. 

![image](https://user-images.githubusercontent.com/72511276/178145931-8717d38a-b9c4-47a1-b4a2-463c0fae536d.png)

- Under Name tag paste in` app-vpc`.
- For the IPv4 CIDR block paste in `10.1.0.0/16`.
- Choose the Tenancy as Default.
- Choose Create VPC. 



## About the Subnets

> After you create your VPC, you need to create subnets inside of this network. Think of subnets as smaller networks inside your base network—or virtual area networks (VLANs) in a traditional, on-premises network. In an on-premises network, the typical use case for subnets is to isolate or optimize network traffic. In AWS, subnets are used for high availability and providing different connectivity options for your resources.

- When you create a subnet, you need to choose three settings.

  - The  VPC you want your subnet to live in, in this case VPC (10.0.0.0/16).

  - The Availability  Zone you want your subnet to live in, in this case AZ1.

  - A CIDR  block for your subnet, which must be a subset of the VPC CIDR block, in  this case 10.0.0.0/24.


- When you launch an EC2 instance, you launch it inside a subnet, which will be located inside the Availability Zone you choose.


## Creating the Subnets

<img width="685" alt="D5Pd-oYISyCT3fqGCLsgxg_de756a8af57b48c0a4d99508da6f6218_Screen-Shot-2021-01-19-at-1 07 15-PM (1)" src="https://user-images.githubusercontent.com/72511276/178144323-538ea6c8-252e-471d-9f65-1f4964c36ca8.png">

> Things to do

- Creating 4 Subnets
  - Two Public subnets (Public subnet in 1st AZ,Public subnet in 2nd AZ )
  - Two Private Subnets (Private subnet in 1st AZ,Private subnet in 2nd AZ )
  - Auto Assign the public Ip address to the both public subnets. 

![image](https://user-images.githubusercontent.com/72511276/178146022-78883be3-05c6-4a3b-8a19-97467602dbcc.png)

- Choose Subnets at the left. 
- Choose Create subnet. 

![image](https://user-images.githubusercontent.com/72511276/178146870-d225dab6-d34a-4cb7-bbb3-43cb4defcff8.png)

- Under VPC ID, 
- select the app-vpc from the drop down list. 

![image](https://user-images.githubusercontent.com/72511276/178147036-4f77809f-e603-4f2c-bea6-39a3b62fd941.png)


- Under Subnet settings and Subnet name, paste in `Public Subnet 1`.  
- Under Availability Zone, choose the 1st AZ i.e `us-east-1a`.  
- For the IPv4 CIDR block paste in `10.1.1.0/24`. 
- Choose Add new subnet. 

![image](https://user-images.githubusercontent.com/72511276/178147098-e950997a-c432-477d-9ee6-b22a0fa0d249.png)

- Under Subnet settings and Subnet name, paste in `Public Subnet 2`.  
- Under Availability Zone, choose the 2nd AZ i.e is `us-east-1b`.  
- For the IPv4 CIDR block paste in `10.1.2.0/24`. 
- Choose Add new subnet. 

![image](https://user-images.githubusercontent.com/72511276/178147155-1d167a9c-ff15-4691-adc3-994d05d71cd0.png)


- Under Subnet settings and Subnet name, paste in `Private Subnet 1`.  
- Under Availability Zone, choose the 1st AZ i.e is `us-east-1a`.  
- For the IPv4 CIDR block paste in `10.1.3.0/24`. 
- Choose Add new subnet. 

![image](https://user-images.githubusercontent.com/72511276/178147220-7f89aac1-f64e-4d6f-89ab-00b73767f03a.png)


- Under Subnet settings and Subnet name, paste in `Private Subnet 2`.  
- Under Availability Zone, choose the 1nd AZ i.e is `us-east-1a`.  
- For the IPv4 CIDR block paste in `10.1.4.0/24`. 

Finally choose Create subnet. 

- After creating the Subnets you will be redirected to the subnet Dashboard. which looks somthing like this.

![image](https://user-images.githubusercontent.com/72511276/178147317-5de1750f-f705-4016-880c-966ce043b0d0.png)

  
  ## Auto assign Public Ipv4 adress
  
  ![image](https://user-images.githubusercontent.com/72511276/178147360-0f2ac690-fcce-4eb4-87f2-856db98ade26.png)
  
- Select the checkbox next to Public Subnet 1 after the subnets have been created. 
- Choose Actions and click on edit subnet option

![image](https://user-images.githubusercontent.com/72511276/178147440-ef39cb07-780b-4ae0-a7e4-5dd80fe48378.png)

- Modify auto-assign IP settings. 
- Under Auto-assign IPv4, 
- choose Enable auto-assign public IPv4 address. 
- Choose Save. 

Do the same for the public subnet 2
- De-select Public Subnet 1. 
- Select the checkbox next to Public Subnet 2. 
- Choose Actions and Modify auto-assign IP settings. 
- Under Auto-assign IPv4, 
- choose Enable auto-assign public IPv4 address. 
- Choose Save.


> Now we have created the VPC and Subnets we need to route the traffic through subnets for which we are going to use `Route Tables` but before routing the traffic we need to pass the internet for the vpc this will be done by the `Internet Gateway`

## About Internet Gateway

Internet Gateway

To enable internet connectivity for your VPC, you need to create an internet gateway. Think of this gateway as similar to a modem. Just as a modem connects your computer to the internet, the internet gateway connects your VPC to the internet. Unlike your modem at home, which sometimes goes down or offline, an internet gateway is highly available and scalable. After you create an internet gateway, you then need to attach it to your VPC.



## Creating Internet Gateway

![image](https://user-images.githubusercontent.com/72511276/178147540-d0c62a44-49dd-4599-9312-c06287dfc4e4.png)

- Choose Internet Gateways in the left panel. 
- Choose Create internet gateway. 

![image](https://user-images.githubusercontent.com/72511276/178147620-e941c890-78d2-49cc-9059-f0fef804b3dd.png)


- Under Name tag paste in `app-igw`. 
- Choose Create internet gateway. 

> Now we have created the internet gateway we need to add this gateway to the VPC that we have created Earlier.

![image](https://user-images.githubusercontent.com/72511276/178147665-3e136444-ed92-4716-9011-cb5532a09c62.png)

- Choose Actions and Attach to VPC. 

![image](https://user-images.githubusercontent.com/72511276/178147735-de3399b5-15dc-4d2d-9d26-554aa68f48e9.png)


- Under Available VPCs choose the `app-vpc`. 
- Choose Attach internet gateway. 


## About Route Table

- A route table specifies how packets are forwarded between the subnets within your VPC, the internet, and your VPN connection.

- The Main Route Table
  - When you create a VPC, AWS creates a route table called the main route table. A route table contains a set of rules, called routes, that are used to determine where network traffic is directed. AWS assumes that when you create a new VPC with subnets, you want traffic to flow between them. Therefore, the default configuration of the main route table is to allow traffic between all subnets in the local network. 
There are two main parts to this route table.

- Custom Route Tables
  - While the main route table controls the routing for your VPC, you may want to be more granular about how you route your traffic for specific subnets. For example, your application may consist of a frontend and a database. You can create separate subnets for these resources and provide different routes for each of them.

- If you associate a custom route table with a subnet, the subnet will use it instead of the main route table. By default, each custom route table you create will have the local route already inside it, allowing communication to flow between all resources and subnets inside the VPC.

<img width="804" alt="69m1UQ52QS6ZtVEOdsEuvg_6d98f06359fd4ce9bd3233180011cf7d_Screen-Shot-2021-01-19-at-1 24 00-PM (1)" src="https://user-images.githubusercontent.com/72511276/178145093-292c92e3-fd74-490c-b336-20c94e857829.png">


## Creating a Route Table

> Things to do 

- we are going to create two route tables.
  - one for the public subnets(app-routetable-public) which will connect tp public subnet 1 and public 1b
    - As this is the public subnet we are going to route `igw` to this route table.
  - one for the private subnets(app-routetable-private) which will connect to private subnet 1 and private subnet 2
    - As this is the private subnet we are `notgoing to route`the `igw` to this route table.

## public route table

![image](https://user-images.githubusercontent.com/72511276/178185613-2c961c83-902f-429e-95c6-6c91123598fd.png)

- Choose Route Tables at the left. 
- Choose Create route table. 

![image](https://user-images.githubusercontent.com/72511276/178185868-30350af5-9c37-4eaa-b83e-a04db6366c6c.png)

- Under Name tag, paste in `app-routetable-public`.
- Under VPC, choose the `app-vpc`. Choose Create. 
- Choose Close. 

## private route table


- Choose Create route table. 

![image](https://user-images.githubusercontent.com/72511276/178186359-5e9f5c87-c900-49fb-96fc-344593004eb9.png)

- Under Name tag, paste in `app-routetable-private`.
- Under VPC, choose the `app-vpc`. Choose Create. 

> As of now we have created the route tables we need to edit the routes and associate the subnets to  route tables


## Routing for public rt 

- Select the `app-routetable-public` from the list. 

![image](https://user-images.githubusercontent.com/72511276/178188340-979ae4ba-02f9-4186-a160-92a06104dba2.png)

- Choose the actions. 
- Choose Edit routes. 
- Choose Add route. 

![image](https://user-images.githubusercontent.com/72511276/178188833-f6e5e8cf-aa5f-4758-b400-1351a6afa085.png)

- For Destination, paste in `0.0.0.0/0.` For Target choose `Internet Gateway`. 

![image](https://user-images.githubusercontent.com/72511276/178214070-a295face-1ca7-4996-9ec0-d382a207a7df.png)

- Choose the app-igw you set up in the VPC section. 
- Select Save routes. 
- Choose Close. 

> Now as we connecred the flow of internet to the app-routable-public we need to associate the routable to the subnets 

## Subnet Associations for public rt

In the Route table dashboard you will find the subnet association besides routes

![image](https://user-images.githubusercontent.com/72511276/178215039-3b68b555-71db-49fb-b74c-ff5c28d00aea.png)

- Choose the Explicit (i.e defined by the user ) Subnet Associations tab. 
- Choose Edit subnet associations. 

![image](https://user-images.githubusercontent.com/72511276/178216180-751bf2d8-4442-465c-8ba3-661cd9ae01b6.png)

- Select the 2 Public subnets (Public Subnet 1 & Public subnet 2) you created in the Subnet section. 
- Choose Save. 

## Subnet Associations for private rt

> As this is a private route table we didn't need to connect any internet i.e is `(igw)`

- Deselect the `app-routetable-public`. 

![image](https://user-images.githubusercontent.com/72511276/178216702-bf204550-490f-4de5-9f11-51e8640bd10e.png)

- Select the `app-routetable-private` from the list. 
- Choose the Subnet Associations tab. 
- Choose Edit subnet associations.

![image](https://user-images.githubusercontent.com/72511276/178216897-35c74dd7-e7f4-4b29-9194-7d22d05cf75c.png)

- Select the 2 Private subnets (Private Subnet 1 & Private Subnet 2) you created in the Subnet section. 
- Choose Save.

---
Now we have created the VPC the only thing left is to lanch the ec2 servers in this VPC 

As we have disscued earlier on how to lanch an instance in the default vpc now we are going to lanch the instance in the VPC we created you can take a look at the EC2 Lanch in the previous section.


### Lanching EC2 in our Own VPC 

> Every Step follows the same except the stage - 3 `Configure Instance`

> We are going to lanch our instance in the `app-vpc` in the `public subnet 1`

3. Configure Instance

- In this section we are going to configure instance to suit your requirements

![image](https://user-images.githubusercontent.com/72511276/178259640-2435b752-8960-41a2-8147-873776a1e55b.png)

- Next to Network choose the `app-vpc` from the list. 
- Next to Subnet choose `Public Subnet 1` from the list. 
- Next to Auto-assign Public IP choose Enable. 
- Next to IAM role choose the S3DynamoDBFullAccessRole. 

![image](https://user-images.githubusercontent.com/72511276/178260448-af80bb2f-a2d4-43d4-bb71-a2bfc7b5b34b.png)

- Scroll down to Advanced Details. Paste in the following into the User data box:

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




- With this instance configure lanch the instance.
- Wait till all the status checks are passed then.

![image](https://user-images.githubusercontent.com/72511276/178263278-0e795344-fce9-4228-8a36-0b3578fbed41.png)

- In the instance Dashboard click on the `WEBAPP_INSTANCE` check box to open up the details tab, copy the Ipv4 Adress 


- Paste it into a new browser tab/window. 
- You should see a Employee Directory placeholder.
- Right now, you will not be able to interact with it as it's not currently connected to a database. 


### Stopping an EC2 Instance

![image](https://user-images.githubusercontent.com/72511276/178264442-3b78cf4f-ff6b-4bb7-8b0c-95d54d723164.png)

>Choose Instance state and `Stop instance`.
>Choose Stop. 
>The Instance state will eventually go into the `Stopped state`. 

```
As of now we have created a vpc and A EC2 instance which contains your flask app code embedded in the user data

But we haven't connected our database and the s3 bucket to store info and images of the employes respectively.
In the next stage i.e stage-4 we are going to create a S3 bucket 

```
### Stage-4 Creating an S3 Bucket

- [Storage in AWS](#storage-in-aws)
- [About S3 buckets](#about-s3-bucket)
  - [Creating a S3 Bucket](#creating-a-s3-bucket)
   


# Storage in AWS

There are three types of cloud storage: 
1. Object
2. File
3. Block

> Object storage -----Example : Amazon Simple Storage Service (S3)-------

- Objects are stored in a flat structure instead of a hierarchy. 
- Each object is a file with a unique identifier. This identifier, along with any additional metadata, is bundled with the data and stored.

- Changing just one character in an object is more difficult than with block storage. 
- When you want to change one character in a file, the entire file must be updated.

- With object storage, you can store almost any type of data, and there is no limit to the number of objects stored, making it easy to scale. 
- Object storage is generally useful when storing large data sets, unstructured files like media assets, and static assets, such as photos.

 > File Storage ------ Example : Amazon Elastic File System  ( EFS )----------

- File storage is ideal when you require centralized access to files that need to be easily shared and managed by multiple host computers. 
- Typically, this storage is mounted onto multiple hosts and requires file locking and integration with existing file system communication protocols. 


> Block Storage --------Example : Amazon Elastic Block Storage ( EBS )

-  Block storage splits files into fixed-size chunks of data called blocks that have their own addresses. 
-  Since each block is addressable, blocks can be retrieved efficiently. 
-  when you want to change a character in a file, you just change the block, or the piece of the file, that contains the character. This ease of access is why block storage solutions are fast and use less bandwidth.
- Since block storage is optimized for low-latency operations, it is a typical storage choice for high-performance enterprise workloads, such as databases or enterprise resource planning (ERP) systems, that require low-latency storage. 

# About S3 buckets

- Amazon S3 is an object storage service. Object storage stores data in a flat structure, using unique identifiers to look up objects when requested. 
- An object is simply a file combined with metadata and that you can store as many of these objects as you’d like. All of these characteristics of object storage are also characteristics of Amazon S3. 
- All objects you put inside a bucket are redundantly stored across multiple devices, across multiple Availability Zones. This level of redundancy is designed to provide Amazon S3 customers with `99.999999999% durability` and `99.99% availability` for objects over a given year.

- In Amazon S3, you have to store your objects in containers called buckets. 
- You can’t upload any object, not even a single photo, to S3 without creating a bucket first. 
- When you create a bucket, you choose, at the very minimum, two things: 

  - The bucket name  ( A bucket name should be unique across all AWS accounts )
  - The AWS Region you want the bucket to reside in.
- Everything in Amazon S3 is private by default. 

- If you decide that you want everyone on the internet to see your photos, you can choose to make your buckets public.

![KRgCnEr0RY-YApxK9HWPyQ_78588d339d804b78b26f905033288350_bucket2](https://user-images.githubusercontent.com/72511276/178994525-ebf54531-8cfd-46b0-a804-d82d00e4b3d9.jpeg)

# Understand S3 Bucket Policies

- S3 bucket policies are similar to IAM policies, in that they are both defined using the same policy language in a JSON format. 
-  S3 bucket policies are only attached to buckets. S3 bucket policies specify what actions are allowed or denied on the bucket.
- S3 Bucket policies can only be placed on buckets, and cannot be used for folders or objects. However, the policy that is placed on the bucket applies to every object in that bucket.


# Storage Classes in S3?

- When you upload an object to Amazon S3 and you don’t specify the storage class, you’re uploading it to the default storage class—often referred to as standard storage. 
- S3 storage classes let you change your storage tier as your data characteristics change. 
- For example, if you are now accessing your old photos infrequently, you may want to change the storage class those photos are stored in to save on costs.

> There are six S3 storage classes. 

1. Amazon S3 Standard
2. Amazon S3 Intelligent-Tiering
3. Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
4. Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)
5. Amazon S3 Glacier
6. Amazon S3 Glacier Deep Archive

## Creating a S3 Bucket

- Search for S3 in the search bar at the top. Choose S3. 

![image](https://user-images.githubusercontent.com/72511276/179155073-b782d0f3-6575-4e6e-8ebf-e547d9dd8834.png)

- Choose Create bucket. 

![image](https://user-images.githubusercontent.com/72511276/179155201-d7740956-41b1-4b08-a54e-93e73ae0cc83.png)

- For the Bucket name `photo-bucket` then use your initials and a unique number.

![image](https://user-images.githubusercontent.com/72511276/179155440-3117adc5-fbf1-48f7-817f-db7c8cb705f9.png)


Example:

photo-bucket-al-007

> Make sure the Region is the region where you have created the other services. Again, this can be found at the top right( choose : us-east-1a ). 

> Leave everything as default then scroll down to create bucket.

- Choose Create bucket.

![image](https://user-images.githubusercontent.com/72511276/179155950-563e67bd-c3b0-4d6d-9d5e-fb6746e460fb.png)

- Choose your newly created bucket by clicking on the name of your bucket. 

![image](https://user-images.githubusercontent.com/72511276/179156321-96c23e82-342e-4d62-9424-e32134aebe8e.png)

- Choose Upload. ( You can also add folders )

![image](https://user-images.githubusercontent.com/72511276/179156581-1b3bf25f-485b-49d1-aa74-c45dc7af5a9f.png)

- Choose Add files. Choose a photo of your choice on your computer. 

![image](https://user-images.githubusercontent.com/72511276/179157208-e58333e3-227b-4567-a6c7-be8939b79823.png)

- Choose Upload.

![image](https://user-images.githubusercontent.com/72511276/179157712-29381dcd-a63a-488f-bfca-888b94ec1dcf.png)

- At the top, you should see Upload succeeded in green. Choose Exit.

In order to access this file click on the object to open up its url

![image](https://user-images.githubusercontent.com/72511276/179158105-89a8f651-34d0-4750-8cf0-6cdeadf6cd50.png)

![image](https://user-images.githubusercontent.com/72511276/179158250-12fb9a36-8b21-46f9-9051-8e18dc1e2f4f.png)

![image](https://user-images.githubusercontent.com/72511276/179158554-0aa4f743-6e6f-4352-ae70-eca593929cca.png)

We can access the file using this open button as this accessed  from the user side (i.e Owner )

But when you access this from the below URL you will find 

![image](https://user-images.githubusercontent.com/72511276/179158927-30fda498-f514-45cf-83db-95e68b6aa9d4.png)

![image](https://user-images.githubusercontent.com/72511276/179159099-29aeb130-36ab-4c0d-a4bf-7496880f4a3c.png)


> Now we have created our bucket and uploaded a image in the bucket we need to access this image but as the default setting is set to private we can't access this files in order to access them we need to modify the S3 bucjet policies.



## Modify the S3 bucket policy


- Choose the Permissions tab. Scroll down to Bucket policy.

- Choose Edit. Paste in the following policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3ReadAccess",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<INSERT-ACCOUNT-NUMBER>:role/S3DynamoDBFullAccessRole"
            },
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::<INSERT-BUCKET-NAME>",
                "arn:aws:s3:::<INSERT-BUCKET-NAME>/*"
            ]
        }
    ]
}

```

```
Replace the <INSERT-BUCKET-NAME> value with your bucket name.

Replace the <INSERT-ACCOUNT-NUMBER> value with your account number. This can be found by choosing your username at the top right and copying down the value next to My Account.

```

- Choose Save changes.

> Now we have attached the policies to the s3 in a way that they going to have full access to the EC2,Dynamo-Db.

> You can attach you s3 bucket to the EC2 using the user data we will do it later in the stage-6

Now we have created your S3 object we need to delete them after they usage is done else that going to cost in the future.

## Delete your object

- Search for S3 in the search bar at the top. Choose S3. 
- Select your employee-photo-bucket-. Select your object. 
- Choose Delete. Confirm deletion by typing in the words permanently delete. 
- Choose Delete objects. Choose Exit. 

### Stage-5 Creating a Dynamo Db Database

- [Database in AWS](#database-in-aws)
  - [Different types of Databases in aws](#different-types-of-databases-in-aws)
  - [Entities of Amazon Relational Database Service] (#entities-of-amazon-relational-database-service) 
- [About Dynamodb Database](#about-dynamodb-database)
  - [Creating a Dynamodb Database](#creating-a-dynamodb-database)
   
## Database in AWS

> What Is a Relational Database?
 A relational database organizes data into tables. Data in one table can be linked to data in other tables to create relationships—hence, the relational part of the name.

> What Is a Relational Database Management System?
A relational database management system (RDBMS) lets you create, update, and administer a relational database. Here are some common examples of relational database management systems:

- MySQL

- PostgresQL

- Oracle

- SQL server

- Amazon Aurora
   
## Different types of Databases in aws

> What Is Amazon RDS?
- Amazon RDS enables you to create and manage relational databases in the cloud without the operational burden of traditional database management.
- For example, if you sell healthcare equipment and your goal is to be the number-one seller in the Pacific Northwest, building out a database doesn’t directly help you achieve that goal though having a database is necessary to achieve the goal.
 
- Amazon RDS helps you offload some of this unrelated work of creating and managing a database.
-  You can focus on the tasks that differentiate your application, instead of infrastructure-related tasks such as provisioning, patching, scaling, and restoring.

Amazon RDS supports most of the popular relational database management systems, ranging from commercial options, open source options, and even an AWS-specific option. Here are the supported Amazon RDS engines. 

- Commercial: Oracle, SQL Server

- Open Source: MySQL, PostgreSQL, MariaDB

- Cloud Native: Amazon Aurora


## Entities of Amazon Relational Database Service

> Engine

Their are huge pie of varities of databases to choose from.

![ZUAlGVg1QFiAJRlYNSBYwg_1bba2621dec849d98f072cc7670511bf_ccc0cfcb144d44cf2c539d7dddc55223_3297-e-7-a-1-f-2-c-9-4408-aa-66-1-fd-4161-f-1566](https://user-images.githubusercontent.com/72511276/179655252-baf05b2f-760c-4f54-8b0a-2bec13813389.jpeg)

>  DB Instances

- Just like the databases that you would build and manage yourself, 
- Amazon RDS is built off of compute and storage. The compute portion is called the DB (database) instance, which runs the database engine. 
- Depending on the engine of the DB instance you choose, the engine will have different supported features and configurations. 
- A DB instance can contain multiple databases with the same engine, and each database can contain multiple tables.

- Underneath the DB instance is an EC2 instance. However, this instance is managed through the Amazon RDS console instead of the Amazon EC2 console. 
- When you create your DB instance, you choose the instance type and size. 
- Amazon RDS supports three instance families.

- Standard 
  - which include general-purpose instances

- Memory Optimized
  -  which are optimized for memory-intensive applications

- Burstable Performance
  -  which provides a baseline performance level, with the ability to burst to full CPU usage.

> DB instance size

The DB instance you choose affects how much processing power and memory it has. Not all of the options are available to you, depending on the engine that you choose. You can find more information about the DB instance types in the resources section of this unit.

![iKO5HX5ER0OjuR1-RPdDEA_322b870726334c8194dbe834d2e1aef7_4e6ebe928958442d0bc696a3ee1695e0_573-e-0-d-85-9-c-82-4586-aad-8-574832307724](https://user-images.githubusercontent.com/72511276/179655553-7155caae-54f7-493f-a080-8043488a5dbf.jpeg)

## About Dynamodb Database

> What Is Amazon DynamoDB?
Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. 

DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling. 

With DynamoDB, you can create database tables that can store and retrieve any amount of data and serve any level of request traffic. 

You can scale up or scale down your tables' throughput capacity without downtime or performance degradation. 

You can use the AWS Management Console to monitor resource utilization and performance metrics.

DynamoDB automatically spreads the data and traffic for your tables over a sufficient number of servers to handle your throughput and storage requirements,while maintaining consistent and fast performance. 

All of your data is stored on solid-state disks (SSDs) and is automatically replicated across multiple Availability Zones in an AWS Region, providing built-in high availability and data durability.

- Core Components of Amazon DynamoDB
  - In DynamoDB, tables, items, and attributes are the core components that you work with. 
  - A table is a collection of items, and each item is a collection of attributes. 
  - DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility. 

The following are the basic DynamoDB components:

- Tables 
- Items 
- Attributes


## Creating a Dynamodb Database

To connect the app to a database, you first need to create one! To do this, you'll use DynamoDB. 

- Search for DynamoDB in the search bar at the top. Choose DynamoDB.

- At the left choose Tables. Choose Create table. 

- For the Table name paste in Employees. For the Primary key paste in id. 

- Choose Create. 

> As we have created our Dynamodb table we will alter this and add items to it through the `Employee directory application which will be running in the EC2 instance`
after we have added any item we will again come back to the DynamoDB Dashboard to view the final table.




### Stage-6 Creating a Auto scaling Group

### Stage-7 Cleaning Up the Resources
