### Deploy a Highly Available Application with Load Balancer and Auto Scaling

### Step 1: Launch an EC2 Instance
<< Create an Instance

<< Click on Launch Instance.

<< Enter a name for your instance (e.g., AWS Project Web Server).

<< Select Amazon Linux 2023 (Free Tier eligible).

<< Choose t2.micro instance type.

<< Create or Select a Key Pair

<< Click on Create new key pair. Name it (e.g., AWSProjectKey).

<< Select .pem format and download it. Keep this key safe.

<< Choose Create Security Group.

<< Allow the following rules:
HTTP → Port 80 → Anywhere (0.0.0.0/0) for website access.
SSH → Port 22 → My IP for secure access.

<< Launch the Instance
Click Launch and wait for it to start.

### Step 2: Connect to Your EC2 Instance Using SSH

## Next Step:

<< Once you're connected to the instance using SSH, let me know, and we’ll proceed to install the necessaStep 3: Install and Configure a Web Server
We’ll install Apache HTTP Server (httpd) to host your static website.

# Current Scenario:
## Instance 1: You're connected using SSH, and we are setting up Apache and the website.

## Instance 2: It's created, but we haven't connected to it yet

### Step 3: Create an Amazon Machine Image (AMI)
 
 << We’ll create an AMI from your configured instance to replicate it for the second instance.
 
 ## Steps to Create an AMI

 << Go to AWS Management Console

<< Navigate to EC2 → Instances.

<< Select the Instance

<< Choose Instance 1 (the one you've set up).

<< Click on Actions → Image and templates → Create Image.
Provide Details

<< Image Name: Give it a meaningful name (e.g., AWS-Project-Web-Server)
Description: Add a short description (e.g., Web server with Apache website)and stat.
Leave other options as default.
Create the Image
Click Create Image.
The AMI creation will take a few minutes.

### Step 4: Launch the Second Instance Using the AMI
 ##  Now that your AMI is ready, follow these steps to launch a second instance using the image:
 << Go to AWS Management Console
   
 <<  Navigate to EC2 → AMIs (under the Images section).
 
 <<  Select your created AMI (e.g., AWS-Project-Web-Server).
 
 <<  Launch an Instance
 
 <<  Click on Launch instance from image.
 
 <<  Choose t2.micro (Free Tier eligible).
 
 <<  Ensure the correct key pair is selected (the same one you used for Instance 1).
 
 <<  Select the same Security Group that allows HTTP and SSH access.
 
 <<  Configure Details
 
 <<  Keep other settings as default.
 
 << Click Launch Instance.
 
 <<  Check Status


Go to EC2 → Instances.
Wait for the instance to move to the Running state.
### Next Step:
Once the second instance is running, we’ll proceed to set up the Elastic Load Balancer (ELB).

## Step 5: Create an Elastic Load Balancer (ELB)
The ELB will distribute incoming traffic between your two instances, ensuring reliability and scalability.
<< Steps to Create an ELB

## Go to AWS Management Console

## Navigate to EC2 → Load Balancers.

## Click on Create Load Balancer.

## Choose Load Balancer Type

<< Select Application Load Balancer (ALB) since it’s ideal for HTTP/HTTPS traffic.
Click Create.

<< Configure Basic Settings

<< Name: Provide a name (e.g., AWS-Project-ELB).
Scheme: Choose Internet-facing.
IP Address Type: Choose IPv4.
Configure Listeners

<< Select HTTP on port 80.

<< Choose Availability Zones: Select the same region where your instances are deployed.Choose at least two availability zones for high availability.

<< Configure Security Group:Select the same security group used for your instances.

<<Create Target Group:Click Create Target Group → Instances.

<< Provide a name (e.g., AWS-Project-TG).
  Protocol: HTTP
   Port: 80
   
<< Click Next.

<< Register Instances

<< Select your two running instances.

<< Click Include as pending below.

<< Click Create Target Group.

<< Finish ELB Creation

<< Go back to the ELB setup and select the target group you created.

<< Click Create Load Balancer.

### Step 6: Configure Auto Scaling Group (ASG)

## An Auto Scaling Group (ASG) will automatically add or remove instances based on traffic load, ensuring high availability and cost-efficiency.

# Steps to Create an Auto Scaling Group
<< Go to AWS Management Console

<< Navigate to EC2 → Auto Scaling Groups.

<< Click on Create Auto Scaling Group.

<< Basic Configuration
Name: Provide a name (e.g., AWS-Project-ASG).
Launch Template or Configuration: Select Launch Template.
Choose the template you used to create your instances.
Select Network

<< Choose the VPC used for your instances.

<< Select at least two Availability Zones for high availability.

<< Configure Load Balancing
Select Attach to an existing load balancer.

<< Choose the Target Group you created earlier for the ELB.

<< Configure Scaling Policies

<< Select Target tracking scaling policy.

<< Choose a metric like Average CPU Utilization.

<< Set a target value (e.g., 50% CPU).
AWS will automatically add instances when the CPU crosses 50% and remove them when it drops.

<< Set Desired Capacity
Desired Capacity: 2 (to match your current setup)
Minimum Capacity: 2
Maximum Capacity: 4 (you can adjust this later based on traffic)

<< Create Auto Scaling Group

<< Click Create and wait for the ASG to be ready

