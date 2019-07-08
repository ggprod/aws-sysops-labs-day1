# Lab 1: Using AWS Systems Manager
In this lab you'll explore and practice using the AWS Systems Manager to manage your AWS resources

## Task 1: Create an EC2 instance for use with Systems Manager
An IAM role called **SystemsManagerRole** has been pre-created that provides the necessary policy to manage an instance with Systems Manager.
Here you will create an instance which specifies this role in the Instance Profile so that you can manage it with the Systems Manager

1. Select the appropriate region you've been assigned to, then in the **Services** menu (in the **Compute** section) click the **EC2**

2. Click the **Launch Instance** button, and click the **Select** button for the AMI image at the top of the list

3. Leave the default setting for the machine type and click the **Next: Configure Instance Details** button in the bottom right

4. Leave all settings as is except the **IAM Role** which should be changed to **SystemsManagerRole**.  This will allow the Systems Manager to manage the instance

5. Now skip ahead to configuation step 5 by clicking on the **5. Add Tags** menu item at the top of the screen and add a key-value pair with key=Name and
value set to your firstname-lastname-instance

6. Click the **Review and Launch**  and then the **Launch** and select **Proceed without a keypair** and click the checkbox and then **Launch Instance**

## Task 2: Generating Inventory lists for Managed Instances
You can use the Systems Manager Inventory to collect operating system, application, and instance metadata from your Amazon EC2 instances and your on-premises servers or virtual machines in your hybrid environment. You can query the metadata to quickly understand which instances are running the software and configurations required by your software policy, and which instances need to be updated.

1. In the **Services** menu (in the **Management and Governance** section) click the **Systems Manager**

2. In the left-hand pane click **Managed Instances**, then click the radio button to the left of your instance (which you can identify by the **Name** coloumn), then click
**Setup Inventory**

3. Take a look at the configurable options on the page, change **Name** to match your instance name, change the **Targets** to **Manually selecting instances**, click the checkbox left of your instance, then click **Setup Inventory** at the bottom-right.  Now the Systems Manager will be checking inventory on your instance every 30 minutes.

4. Click the link in the **Instance ID** coloumn for your instance and click the inventory tab to see the software installed on your instance.  You can select different **Inventory Type** options to browse.

## Task 3: Install a custom application using the Run command
Here you will install some software on your instance using the **Run command** option

1. In the left-hand pane click **Run command**, then click the orange **Run command** button on the right of the page.

2. Click on the magnifying glass in the search bar and select **Platform Types** then select **Linux**, then click the radio button to the left of **AWS-RunShellScript**

3. In the **Command Parameters** section paste `sudo yum -y install httpd` to install the apache web server.  In the **Targets** section select your instance.
Then click **Run** in the bottom right.  When the command completes successfully the software was installed (You'll have to wait a while to see it in the inventory though
because it only updates every 30 minutes)

## Task 4: Create a parameter in the parameter store and verify it in the session manager
AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, and license codes as parameter values. You can store values as plain text or encrypted data. You can then reference values by using the unique name that you specified when you created the parameter.

Here you will create a test parameter in the parameter store and verify the setting of that parameter in the Session Manager.

1. In the left-hand pane click **Parameter Store** then click the orange **Create Paramter** button.

2. Set the **Name** to be your firstname-lastname-test.  Set the value to anything you'd like and leave everything else set as is.

AWS Systems Manager Session Manager lets you manage your Amazon EC2 instances through an interactive one-click browser-based shell or through the AWS CLI. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys. Session Manager also makes it easy to comply with corporate policies that require controlled access to instances, strict security practices, and fully auditable logs with instance access details, while still providing end users with simple one-click cross-platform access to your Amazon EC2 instances.

When used with Microsoft Windows, the AWS Systems Manager Session Manager provides access to a PowerShell console on the instance.

3. In the left-hand pane click **Session Manager** then click the orange **Start Session** button.  Select your instance then click **Start Session**

4. You should see a new browser tab open with a terminal session connected to your instance.

5. In the browser tab with the terminal window type the following command: `aws ssm get-parameters --names --region ` followed by your region (e.g. us-east-1 or us-east-2 or us-west-1 or us-west-2) and then the name of the parameter you entered into the parameter store.  You should see a response that shows you the value.

6. In the browser tab with the AWS management console return to the **Parameter Store**, click on your parameter and **Edit** and update the value and then **Save Changes**

7. In the browser tab with the terminal window type, re-execute the previous command again and verify the updated parameter value.

8. Return to the **EC2** page and select the instance you created, click on **Actions** then select **Instance State->Terminate**