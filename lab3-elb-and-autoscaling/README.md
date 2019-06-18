# Lab 3: Using Auto Scaling in AWS
In this lab you will create an AMI from an EC2 instance (that you have configured with the software you want) and then use that AMI
in an Auto Scaling Group that will scale up and down with load.  You will test the group by connecting to it through an Elastic Load
Balancer

## Task 1: Install Apache Bench on your host EC2 instance and create AMI from your web-server EC2 instance
In the previous lab you created 2 EC2 instances and you will be using those in this lab as well. You will use the first instance to provide a source of load on 
your Auto Scaling Group.  For that purpose you will install Apache Bench which is a simple service to send web traffic into an endpoint.

1. Execute the following command to connect to your EC2 instance: `ssh -i keypair.pem ec2-user@public-ip-address` (replace keypair.pem with your Key Pair filename and public-ip-address with the IPv4 public IP address of your host instance).  When prompted type `yes` and hit return

2. Execute the following commands to install Apache Bench:

```bash
sudo yum install apr-util
sudo yumdownloader httpd-tools
rpm2cpio httpd-tools-2.4.39-1.amzn2.0.1.x86_64.rpm | cpio -idmv
sudo mv usr/bin/ab /usr/bin/
```

3. Find and copy the **Instance ID** for your firstname-lastname-web-server instance, then execute the following command to create an AMI from that server: `aws ec2 create-image --name firstname-lastname-web-server --instance-id your-instance-id` (replace firstname-lastname with your first name and last name and your-instance-id with the instance id of your web-server instance from the first lab)

This command creates an AMI from an instance.  It restarts the instance before creating the AMI to ensure integrity of the image

## Task 2: Create an Auto Scaling environment
In this task you create a Load Balancer to pass requests to instances of your Auto Scaling group.  Requests will be distributed to instances and based on the load more instances can 
be created or destroyed.  CloudWatch collects information about the load on each instance and can be configured with thresholds on when to create or remove instances due to the load.
In this case we'll use the CPU load on each instance as the scaling metric.

1. On the **EC2->Load Balancers** page click **Create Load Balancer** and under the **Application Load Balancer** click **Create**

2. Enter firstname-lastname-elb for **Name** and select the same subnet that your host EC2 instance is in and any other one. and click **Next**

3. You will see a warning about improving security by using HTTPS but ignore that and click **Next**.  Select your Security Group and click **Next**

4. Enter firstname-lastname-webapp for New Target group **Name**, then click **Next**, then click **Next: Review** (skip Registering targets without changing anything).  Then
click **Create**, then click **Close**

5. On the **EC2->Launch Configurations** page click **Create launch configuration**.  Select **My AMIs** in the left-hand pane and select the AMI you created earlier.

6. Leave the instance type set to t2.micro and click **Next**.  Enter firstname-lastname-lc in **Name** and click **Next**.  Leave Storage set as is and click **Next**

7. Select **Select an existing security group** and pick your Security group, click **Review**.  Ignore the warning and click **Create launch configuration**.  Select
**Proceed without a key pair** and click the checkbox and click **Create launch configuration** and then **Create an Auto Scaling Group from this launch configuration**

8. Enter firstname-lastname-asg as the **Name**, select the same subnet that your host instance is in and click on **Advanced Details**

9. Click on the **Receive traffic from one or more load balancers** checkbox and enter firstname-lastname-webapp in the **Target Groups**.  Click on the **Enable CloudWatch detailed Monitoring** checkbox then click **Next**

10. Select **Use scaling policies to adjust the capacity of this group**, set the between scaling between 1 to 2, and the Target value at 5 (this is the average CPU load that the ASG will scale out and in to try to stay around).  Click **Next** and **Next** and set a Tag with key=Name and value=firstname-lastname-webapp

11. Click **Review** and then **Create Auto Scaling group** and then **Close**

## Task 3: Test ELB and Auto Scaling
In this task you will verify the ELB by visiting its DNS name and verifying you see the web-app and then placing a load on it to trigger Auto Scaling

1. On the **EC2->Load Balancers** page click on the selection box to the left of your load balancer and copy the **DNS Name** from the **Description** panel at the bottom

2. Enter the name in a browser tab and verify you see the web-app (if you get a 503 error wait 1-2 minute(s) and try again)

3. In your SSH Terminal connection to your host instance enter the command: `ab -n 1000000 -c 1000 http://lb-dns/` (where lb-dns is the DNS name of your load balancer)

4. On the **EC2->Auto Scaling groups** page click on the selection box to the left of your Auto Scaling group and click on the **Activity History** 

5. Wait and watch for 10-15 minutes, periodically clicking the refresh button to see when your Auto Scaling group adds another instance based on the load (you may have to repeat step 3 a few times if it completes before you see a scale up)

6. Once you see the scale up event, abort Apache Bench (if it is running, via Ctrl-C), terminate the SSH session, and delete your Load Balancer, Auto Scaling Group, Launch Configuration, Target Group, Key Pair, AMI, and Security Group, and terminate your EC2 instances


