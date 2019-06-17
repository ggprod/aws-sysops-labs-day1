# Lab 1: Using AWS Systems Manager
In this lab you'll explore and practice using the AWS Systems Manager to manage your AWS resources

## Task 1: Create an EC2 instance for use with Systems Manager
An IAM role called **SystemsManagerRole** has been pre-created that provides the necessary policy to manage an instance with Systems Manager.
Here you will create an instance which specifies this role in the Instance Profile so that you can manage it with the Systems Manager

1. In the **Services** menu (in the **Compute** section) click the **EC2**

2. 

## Task 2: Generating Inventory lists for Managed Instances
You can use the Systems Manager Inventory to collect operating system, application, and instance metadata from your Amazon EC2 instances and your on-premises servers or virtual machines in your hybrid environment. You can query the metadata to quickly understand which instances are running the software and configurations required by your software policy, and which instances need to be updated.

1. In the **Services** menu (in the **Management and Governance** section) click the **Systems Manager**

2. In the left-hand pane click **Managed Instances**