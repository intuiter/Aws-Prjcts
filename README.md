                                                  Aws Three-Tier Architecture
<p>&nbsp;</p>

### Architectural Diagram of Three-tier work flow:
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/2ad78cbe-9fa6-48e2-bdf0-41fd388d3e5f)
<p>&nbsp;</p>

**A Scenario to explain about three-tier architecture functionality,**

# Presentation Tier (Front-end):
The presentation tier is responsible for interacting with the user and presenting the information. In this scenario, it could be a web application or a mobile app where the user enters their hall ticket number to check their board exam results. The front-end interface displays a form where the user can input their hall ticket number and submit the request.

# Application Tier (Middle-tier):
The application tier contains the business logic and handles the processing of user requests. It receives the hall ticket number from the front-end and validates it. It then sends the hall ticket number to the data tier to retrieve the corresponding exam results. The application tier ensures that the request is properly authorized and handles any necessary data transformations or calculations before sending the response back to the front-end.

# Data Tier (Back-end):
The data tier consists of the database where the exam results are stored. In this scenario, the database could be an Amazon RDS instance configured with Multi-AZ for high availability. It contains the exam results data, including the hall ticket numbers and corresponding scores or grades. The data tier receives the hall ticket number from the application tier and retrieves the exam results associated with that hall ticket number. It then sends the results back to the application tier, which eventually returns the data to the front-end for display to the user. 

# Benefits of Three-tier architecture model:
There are many benefits to using three-tier architectures in AWS including scalability, speed, performance, and availability. As mentioned earlier, modernizing different tiers of an application gives development teams the ability to develop and enhance a product with more incredible speed than developing a singular codebase. A specific layer can be upgraded with minimal impact on the other layers. It can also help improve development efficiency by allowing teams to focus on their core competencies. Many development teams have separate developers who specialize in front- end, server back-end, and data back-end development. By modularizing these parts of an application, you no longer have to rely on full-stack developers and can better utilize the specialties of each team.
<p>&nbsp;</p>

## Objectives:

a) Create a VPC

b) Create a Web Tier using Aws services as Load balancer, Ec2 template, Autoscaling groups and associate it with N/w configuration and security/firewall rules which faced to Public internet.

c) Create an Application Tier using Aws services as Load balancer, Ec2 template, Autoscaling groups and associate it with N/w configuration and security/firewall rules which will have access from webtier only(middle tier)

d) Create a Database Tier is designed such a way that it only allows traffic from Application layer.
<p>&nbsp;</p>

## Implementation:
* ___Create VPC___ - In this step, will create 6 subnets total for our VPC. Two public subnets for Web-facing servers and four private subnets. Two each for application and database tiers. This will be configured across two availability zones to ensure high availability.
Included in VPC will be a NAT Gateway for hosts in the private subnets to ensure they are able to reach out to the internet for updates etc. Public and private route tables will be configured. Internet gateway associated with VPC to allow internet assoaciated to public subnets.
Navigate to the VPC dashboard in the console and click Create VPC
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/4dbe1969-b782-4fe1-9156-2d290fa60a8d)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/ff361d34-a51a-43fc-8fbe-62d1f7ccebab)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/3dd5c42b-c319-4997-83a0-00528161b88d)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/a143c735-7263-4bf4-bcce-be3411acac26)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/b03d8d34-b9bd-47a2-a83f-1e28d138afc1)
<p>&nbsp;</p>

* ___Create a Web Tier___ - Let’s create an auto-scaling group for web tier with an application load balancer. 
Navigate to Auto scaling groups and click create.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/803fb038-aa12-4fbc-9ae2-8d94b54a3061)
<p>&nbsp;</p>
In the new window configured the name of the template and selected a free tier Amazon AMI with a t2.micro instance type then associated a key pair.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/a23e33b1-c4e9-4950-8435-a5096050bd79)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/6499e5be-f371-4ee9-9557-58dc3631a7e6)
<p>&nbsp;</p>

Created a new Security group, given a name and allowed HTTP traffic from any source. Add a rule to allow SSH ideally from your IP only. I’m going to allow it from anywhere for demonstration purposes. Select the VPC we created.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/7d8f0880-36fc-4170-b6af-4187c03dcec7)
<p>&nbsp;</p>
Under Advanced network configuration add network interface and enable Auto-assign public IP.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/91ba6852-6b27-4d10-9957-7b3998d90f71)
<p>&nbsp;</p>

Now will add bootstrap the image to install and run apache, PhpMyadmin. Expand advanced details and input the following code in the User data field. Click Create launch template to finish.


![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/c4091624-6f8c-4b19-911c-d81391464015)
<p>&nbsp;</p>

Now back at the Auto Scaling group wizard selected the template just created from the drop-down then click Next.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/6be7206d-abb9-4e62-aabc-7c5e0418c1ba)
<p>&nbsp;</p>
Select VPC and select created two private subnets from the drop-down then click Next.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/aedeb4cb-7c59-4af1-a9aa-8c0c620c31b3)
<p>&nbsp;</p>
Select Attach to a new load balancer and Internet-facing. Select Create a target group(WebTier-TG) then leave all the defaults. Click Next.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/8d997f18-4f36-41ce-9132-f9eae325216a)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/784962ac-1440-4f40-a048-075468284d8a)
<p>&nbsp;</p>
 
We want a minimum of two machines up at all times. Change the settings to the following. Then click Skip to review and click create Auto Scaling group.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/4f7b4923-f11a-4a3e-b790-5453793687c3)
<p>&nbsp;</p>

Now that we have our ASG created we need to change the security group for our application load balancer to allow internet traffic through. Navigate to EC2 > Load balancers and select our WebTier-ALB load balancer.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/f87dd1da-74f2-4b36-8982-d0d41f9053d2)
<p>&nbsp;</p>
 
Go to Actions > Edit security groups Then click create new security group.
Give it a name and select our VPC. Allow HTTP traffic from any source.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/cd637a40-d96b-43d0-baa8-32610661b8d1)
<p>&nbsp;</p>
 
Now associate the security group with the load balancer created,
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/4f8f6de9-0ecf-4f2d-9ddc-9fce3c30f37f)
<p>&nbsp;</p>
 
Now let’s test by copying the DNS name of our load balancer into a web browser.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/fc6e11f4-3ee8-4b60-a154-7e2b08f4dcbd)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/1eb21481-9c20-4eae-85f1-c209ba4ba801)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/773f6f8a-c6b2-4e99-86c7-586ead702be6)

Successfully Web Tier has been created and we can view our frontend server as shown the screenshots.
<p>&nbsp;</p>

* ___Create Application Tier___ - Now we will create the application tier. This will be very similar to our web tier. We will place our Auto Scaling Group in two private subnets to host our EC2 instances.
Created another ASG as before this time called it Apptier-ASG and select Created launch template. Select the same AMI and instance type as we did for the web tier.
Created a new security group. This time configure the SG to only allow traffic from our Web-SG.
 
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/b5d057dd-ac55-4b6e-9431-7d67933cf737)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/22316895-0e9d-41bf-9ae2-f6310a2b45b9)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/152537cc-07e2-4140-94ff-67575783f8e8)
<p>&nbsp;</p>
No need to enable public IP assignment as these instances will remain in our private subnets. We will not add any code to the user data field.
Now back in our ASG wizard select the launch template we just created and click Next. Select the appropriate VPC and two private subnets.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/9cc2655c-74af-4cbc-9022-9674d6296853)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/76f23761-f340-4e5b-babd-9462ef500a0b)
<p>&nbsp;</p>
Add a new ALB and this time select the Internal for load balancer scheme.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/afaf941f-9299-47e8-b2a4-97fdd50d2e88)
<p>&nbsp;</p>
Select Private subnets for application tier and create Target group by name(Apptier-TG)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/58139dab-87da-4ad8-864a-3b22e61d20a2)

Complete the wizard using the configuration as we did for the web tier. We now have two ASG and two more EC2 instances. Now let’s verify that we can reach the tier 2 EC2 instances located in our private subnets from the Web tier. Connect to one of the web tier instances and ping one of the EC2 instances in our tier 2 subnets via private IP.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/7e8a15ea-5b72-492c-a411-e3eee5f6fde5)

Our ping was successfull and we have configured our NSGs correctly. Tier 2 is complete.
<p>&nbsp;</p>
<p>&nbsp;</p>


* ___Add Database Tier___ - Now we will add our database. We will be utilizing the free tier option and creating one MySQL DB in one of our AZs.
Create a new security group(DB-SG). Select our VPC and create an inbound rule to only allow traffic for TCP port 3306 from our App-SG.
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/d1979e40-5a1f-4b3c-8753-bf3536b40ef7)
<p>&nbsp;</p>

Within the console navigate to RDS > Databases and select Create database and choose Standard create and MySQL. Choose the Free tier and leave defaults. Fill in the password field.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/3f7c537d-6fb3-4425-ad31-1c420b293f60)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/c29bcaca-ae7a-428e-a1fa-cf8bb3b12de9)
![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/8cef452b-dc00-4eb8-b7fc-761df2a4eee3)
<p>&nbsp;</p>

Under the Connectivity section select our VPC and our SG for the VPC security group. Select us-west-2a for Availability Zone. Click Create database.

![image](https://github.com/intuiter/Aws-Three-Tier-Architecture/assets/135228471/468b032a-3a93-4f1e-b6ce-77dcd3a1c9db)

The database has been created and we are finished creating Three tier architecture model.



