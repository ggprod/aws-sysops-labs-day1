# Lab 2: Creating Amazon EC2 instances
In this lab you'll launch EC2 instances via the management console as well as via the CLI

## Task 1: Create an EC2 instance via the management console
Create an EC2 instance in the management console

1. Select the appropriate region you've been assigned to, then in the **Services** menu (in the **Compute** section) click the **EC2**

2. Click **Launch Instance** Select the first AMI image at the top and leave other configuration as-is.  In the **5. Add Tags** section give the instance a tag with key=Name
and value set with your firstname-lastname and any suffix.

3. In the **6. Configure Security Groups** section. Add a Rule for HTTP.  Leave the Rule configuration as is which allows access from any IP address over port 80.

4. Click **Review and Launch** and then **Launch** and select **Create a new key pair**.  Give the new keypair a name firtname-lastname-key1 and click **Download Key Pair** and
Save it to your machine and make note of the location.  Click **Launch Instances**

5. View the Instances page and make note of the public IPv4 address of your instance.  

## Task 2: Connect to EC2 instance via SSH
Here you will connect to the instance from your local machine

1. Open a local terminal session on your machine and change to the folder you downloaded the Key Pair file to.

2. Execute the following command to change the permissions on the key-file (necessary to allow using it with SSH to connect to instance): `chmod 400 keypair.pem` (replace keypair.pem with the name of your Key Pair filename)

3. Execute the following command to connect to your EC2 instance: `ssh -i keypair.pem ec2-user@public-ip-address` (replace keypair.pem with your Key Pair filename and public-ip-address with the IPv4 public IP address of your instance).  When prompted type `yes` and hit return

4. You should not be connected via SSH to your instance.  Let's install and start an apache web server via the commands:

```bash
sudo yum -y install httpd
sudo systemctl enable httpd
sudo systemctl start httpd
sudo touch /var/www/html/index.html
sudo nano /var/www/html/index.html
```

5. Now copy and past the following into the nano editor for the file (open in the terminal): `<html><h1>Hello From Your Web Server!</h1></html>` and Ctrl-O, return, Ctrl-X

6.  Verify that the web-server is running by opening a browser tab and putting in the public IPv4 address of your instance (You should see: Hello From Your Web Server!)

## Task 3: Create an EC2 instance via the CLI
Here you will use the AWS CLI to create an instance

1. Under **Services** in the **Security, Identity, and Compliance** section select **IAM** then **Users** and click on your username and the **Security Credentials** tab

2. Click **Create Access Key** and copy the **Access key ID** and click **show** under **Secret access key** and copy it as well

3. In your SSH terminal session to your first instance, type the command: `aws configure` and when prompted enter your Access key data, and your assigned region.  Hit return to leave output format set to `None`

4. Execute the following commands in your SSH session:

```bash
# Set the Region
AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
export AWS_DEFAULT_REGION=${AZ::-1}

# Obtain latest Linux AMI
AMI=$(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --query 'Parameters[0].[Value]' --output text)

echo $AMI
```
