# Deploying-Multi-Tier-Website-On-EC2

## Problem Statement:
Company ABC wants to move their product to AWS. They have the following things setup right now.
1.	MySQL DB
2.	Website PHP
The company wants high availability on this product, therefore wants auto-scaling to be enabled on this product. 

## First let's start by understanding a few key terms :
## What is EC2?

EC2 stands for Elastic Compute Cloud, which is a web service provided by Amazon Web Services (AWS). EC2 is a central part of AWS and offers scalable computing capacity in the cloud. It allows users to rent virtual servers, known as instances, on a pay-as-you-go basis. These instances can be easily configured and scaled based on the user's computing requirements. EC2 instances come with a variety of instance types optimized for different use cases, such as compute-optimized, memory-optimized, storage-optimized, and more. Users have full control over their instances, including the ability to start, stop, terminate, and configure them. EC2 is widely used for hosting applications, websites, and for running various types of workloads in the cloud. It provides flexibility, scalability, and reliability for computing resources in the AWS cloud environment.

## What is a Multi-Tier Website?
Multi-tier website refers to a web architecture that is organized into different layers or tiers, each serving a specific function in the overall operation of the site. The three main tiers are the presentation layer (or user interface), the business logic layer, and the data layer. The presentation layer is what users see and interact with on the website. The business logic layer contains the application's logic and rules, handling tasks like processing user inputs and making decisions. The data layer manages the storage and retrieval of information from databases. This architectural approach helps in creating a scalable, modular, and maintainable web system, where changes or updates in one tier do not necessarily affect the others.

## What is RDS?
Amazon RDS (Relational Database Service) is a fully managed database service offered by Amazon Web Services (AWS), enabling users to easily set up, operate, and scale relational databases in the cloud. Supporting various database engines such as MySQL, PostgreSQL, Oracle, and Microsoft SQL Server, RDS automates routine administrative tasks like backups and patching, allowing users to focus on application development. With features like scalability, high availability through Multi-AZ deployments, security enhancements, and the option for read replicas, RDS provides a reliable and efficient solution for organizations seeking a cloud-based, hassle-free relational database management system.

## What is AutoScaling?
Auto Scaling in AWS is a service that automatically adjusts the number of compute resources (such as EC2 instances) in a group to match the desired capacity specified by the user. The primary goal of Auto Scaling is to ensure that you have the right amount of resources available to handle varying workloads, helping to maintain application availability and manage costs effectively.

## What is an Auto-Scaling Group?
Auto Scaling Groups (ASG) in AWS allow for the automatic adjustment of the number of EC2 instances based on application demand. By defining policies tied to metrics like CPU utilization or network traffic, ASGs dynamically scale instances up or down, ensuring optimal performance and resource utilization. These groups distribute instances across multiple Availability Zones for increased fault tolerance, and they seamlessly integrate with features like Elastic Load Balancing and AWS CloudWatch for efficient load distribution and monitoring. ASGs simplify the management of instances, offering a scalable and resilient solution that adapts to changing workloads in a cost-effective manner.

# Now let's start implementing our solution :-
 
![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/4466ab29-744f-4bb8-b8de-986212c889ee)

### Step 1: 
Firstly, let's set up a new EC2 instance. We will opt for the Ubuntu AMI and choose the "default" security group (which will allow all traffic to our server) by clicking on “Select existing security group”. Now we can launch the instance.

 
![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/13771f84-663c-46db-905a-f5c8b9dd18f5)

### Step 2: 
Now we will open CloudShell and connect to our instance. To achieve this, we will upload our Private Key file to the CloudShell.

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/44d1530c-4477-4d52-be17-fc60ad8afa55)

### Step 3: 
Now we will run this command, to ensure your key is not publicly viewable.
 `chmod 400 satyam-project.pem`
Next we will run the following command to SSH in our instance.
`ssh -i "satyam-project.pem" ubuntu@ec2-107-20-62-201.compute-1.amazonaws.com`


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/3ea1e71c-f185-49a4-83b7-9dc04393067a)


### Step 4: 
Now we need to install apache2 server using this command. Also, we will make sure to check for updates before installing server by using the sudo apt-get update command.
`sudo apt-get install apache2  -y`

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/16da9c63-aeb6-4f1f-a9f9-e672d0a295bc)

### Step 5: 
Now we can check if our website has been deployed on our server. 

![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/15b3222f-bc4a-4e8c-8acc-a9e35d0963ae)

 
### Step 6: 
Now we need to transfer our PHP code to the ubuntu machine. For that purpose, we will use WinSCP. Now by the help of the WinSCP. We can make a connection to the remote system. We need to provide our PPK file to login. Next, we just need to drag and drop our PHP code folder from our local machine to the Ubuntu machine.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/f13eb3bc-ca61-454c-8f4e-bce8c2fdcef8)

### Step 7 :
Now we need to transfer our files to /var/www/html folder. We will use the following command
`sudo cp -r /home/ubuntu/1243/* /var/www/html`


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/3e2a4d24-0ee1-4a9d-ab8d-791c7ef33528)


### Step 8: 
Now we see that our PHP code has been successfully hosted on our apache2 server.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/bf009f0a-443d-497b-ad2b-ffc66e0ee056)


### Step 9: 
Now we need to create the database, for which we will jump to RDS tab and choose MySQL as database. And we will go with the default settings/options for free tier. We will choose “default” (which will allow all traffic to our server) as our security group. We also have to select No for public access because we don’t want our DB to be publicly accessible. Next, we will use db.t2.micro as in our instance configuration. 

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/5bb5daa2-6745-4733-aa48-b862f7ee0762)

![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/090e9a0c-8f38-4b87-bb5a-8d18ebbf0fa7)

 
### Step 10 : 
We will also provide “intel” as the name for our DB. And then hit on “Create Database”. We can see our newly created database.

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/ffb208cc-ca4b-41b8-8e18-8586aa2d328f)

### Step 11: 
To connect RDS to our CloudShell we will need to install MySQL client compatible with PHP. For that we’ll use the following command
`sudo add-apt-repository -y ppa:ondrej/php`
`sudo apt install php5.6 mysql-client php5.6-mysqli`
This command will import the packages from the remote repository and then install the php5.6, mysql-client and mysqli compatible with php5.6

![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/f08e6660-8693-4e51-8cc0-f51e0b9b63ac)

 
### Step 12: 
Now we need to connect to our RDS Instance. We will need to provide 3 things, the endpoint, username and password. The command to connect is as follows:
`mysql -h aws-project.c82fjit7n16w.us-east-1.rds.amazonaws.com -u admin -p`
We will be prompted to enter our password, upon successful input of password we will be able to connect to our database.

![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/cd6cfcfd-6951-4219-a2f6-185ee6cded92)

 
### Step 13 : 
Now we will go inside our database using “use intel” sql command. As we can see we currently don’t have any tables, we can use “show tables” command to check this.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/4b74329e-5a37-4b15-bf97-a6aa5dda52bf)

### Step 14: 
Now we need to create a new table in our database to store the name and email. For that we will use the following SQL command:
`create table data(firstname varchar(20), email varchar(40));`


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/cda54209-d9fb-4c8f-96f6-d1d27ed93b4e)

### Step 15 : 
In our PHP code, we need to change the servername, username and password and substitute with our database credentials.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/0e520dfd-b60b-460a-843e-7c16f4595cd8)

### Step 16:
Now we will try saving some data via the website. We can just provide the name and email and hit on submit. It will add a new entry in our data table. We can check in our database that a new record has been added. We can use the following command to see all the records in our table:
Select * from data;

 
	![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/936b5cdb-5394-41a0-b73b-0827db331140)

### Step 17 :-
Now we need to enable Auto-Scaling. We’ll first start by creating an Image.	


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/3bf0e921-2d2e-4030-afca-5f63ee1ead9c)

### Step 18: 
Now we need to Auto-Scaling Groups. We need to create “Launch Configuration”. We will start by providing a name and description and then choosing t2.micro as our instance type.

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/b516d28e-b8b5-4efe-9f3b-80ce29d405a3)

### Step 19 : 
Now we’ll create a new security group and allow all traffic.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/671b040d-ac76-431e-bb0c-45fde29ef4f3)

### Step 20 : 
Now we’ll select the existing key pair for this, and hit on “Create Launch Configuration”.

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/ab082ab9-bb42-4b93-bc02-ff18c21648ab)

### Step 21 : 
Now we’ll create a new Auto-Scaling Group and select our launch configuration from the dropdown.

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/970bbc1a-7456-4929-bbb6-cacb8e21f0e4)

### Step 22 :
Now we’ll select the default VPC and select different availability zones to make our application highly available. 

 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/5db26c8a-1e90-4d0b-9f0b-49a25560e9b1)

### Step 23 :- 
Now we’ll adjust the Desired Capacity to 2 and Min desired capacity to 1 and Max desired capacity to 3 for demo purposes.


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/b780f221-41a8-4159-8a8d-8ff097117f71)

### Step 24:
AutoScaling Group has been successfully created.	


 ![image](https://github.com/satyamaatmdeep10/Deploying-Multi-Tier-Website-On-EC2/assets/137147966/1724d599-da6a-4c47-b43d-5cec9fe204ca)

### Step 25 : 
We can verify that 2 new instances have been created by Auto-Scaling.

	




	




	





