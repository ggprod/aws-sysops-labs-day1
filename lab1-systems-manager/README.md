# Lab 1: Using AWS Systems Manager
In this lab you'll explore and practice using the AWS Systems Manager to manage your AWS resources

## Task 1: Create an EC2 instance for use with Systems Manager
An IAM role called **SystemsManagerRole** has been pre-created that provides the necessary policy to manage an instance with Systems Manager.
Here you will create an instance which specifies this role in the Instance Profile so that you can manage it with the Systems Manager

1. In the **Services** menu (in the **Compute** section) click the **EC2**

2. Click the **Launch Instance** button, and click the **Select** button for the AMI image at the top of the list

3. Leave the default setting for the machine type and click the **Next: Configure Instance Details** button in the bottom right

4. Leave all settings as is except the **IAM Role** which should be changed to **SystemsManagerRole**.  This will allow the Systems Manager to manage the instance

5. Now skip ahead to configuation step 5 by clicking on the **5. Add Tags** menu item at the top of the screen and add a key-value pair with key=Name and
value set to your firstname-lastname-instance

6. Click the **Review and Launch**  and then the **Launch** and select **Proceed without a keypair** and click the checkbox and then **Launch Instance**

## Task 2: Generating Inventory lists for Managed Instances
You can use the Systems Manager Inventory to collect operating system, application, and instance metadata from your Amazon EC2 instances and your on-premises servers or virtual machines in your hybrid environment. You can query the metadata to quickly understand which instances are running the software and configurations required by your software policy, and which instances need to be updated.

1. In the **Services** menu (in the **Management and Governance** section) click the **Systems Manager**

2. In the left-hand pane click **Managed Instances**