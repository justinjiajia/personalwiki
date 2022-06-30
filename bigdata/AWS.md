





[version_1.0]



# Lab 2 Exercise: Following IAM Best Practices

https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/lab-2-iam.html


The exercises are designed to be completed in your AWS account, and will have an associated cost. For this reason, in addition to the written instructions, this course includes video recordings of the exercises. If you intend to attempt the exercises, familiarize yourself with AWS pricing, specifically Amazon EC2 pricing, Amazon S3 pricing, and Amazon DynamoDB pricing and the AWS Free Tier.

In this scenario, you will follow best practices while continuing to set up your new AWS account. In this exercise, you will log into the root account, delete the root user access keys, and set up multi-factor authentication (MFA).

Instead of using the root user, you will create an IAM admin user. Then, you will will log in as the IAM admin user and create an IAM role that you will later assign to an EC2 instance hosting the employee directory application.

Lab Steps

## Stage 1 - Login to the Console

1. Visit https://aws.amazon.com/console/
2. Choose Sign In to the Console.
3. Choose Root user. Enter the Root user email address.
4. Choose Next.
5. Enter the Password for the root user. Choose Sign in.


## Stage 2 - Enable MFA (optional)

1. At the top right, choose your account name. Then choose My Security Credentials from the drop down menu.

2. Expand Multi-factor authentication (MFA). Choose Activate MFA.

3. On the Manage MFA device pop-up window. Choose Virtual MFA device and choose Continue.

> Note: You will need a virtual MFA application installed on your device or computer. You can see a list of applications on step 1 on the Set up virtual MFA device pop-up window. There is a hyperlink which shows a list of compatible applications. Before continuing to the next step make sure you have one of these applications installed on your mobile device or computer.

4. Choose Show QR code and scan the code using your device.

> Note: If you are using a computer you can choose Show secret key and type the secret key into your MFA application.  

5. Type the first MFA code into the MFA code 1 field. Then type the second generated number into  the MFA code 2 field. Choose Assign MFA.

6. You should see a pop-up indicating that you have successfully assigned a virtual MFA device. Choose Close.

7. Expand Access keys (access key ID and secret access key).

> Note: There should be no access keys listed. If an access key exists (for your new account) choose Delete under Actions. Choose Deactivate. Enter in the access key ID in the confirmation field. Choose Delete.

## Stage 3 - Create an IAM user

1. In the service search bar, type in Identity and Access Management (IAM) dashboard. On the left side panel, choose Users.

2. Choose Add user. Paste in Admin for the User name. Next to Access type, choose Programmatic access and AWS Management Console access.

3. Next to Console password, choose Custom password and type in a password of your choosing.

4. Uncheck Require password reset.

5. Choose Next: Permissions.

6. Choose Attach existing policies directly. Next to Filter policies, search for administrator. Under Policy name, choose AdministratorAccess. Choose Next: Tags.

7. Choose Next: Review. Choose Create user.

8. You can sign in with the new IAM user by clicking the hyperlink at the bottom of the Success window.

> Note: It should look similar to the following: https://000000000000.signin.aws.amazon.com/console. Your account number will be different :)

9. Log in using the Admin user and password that you created.

## Stage 4 - Set up an IAM role for EC2 instance

1. Now that you are logged in as the Admin user, search for IAM again in the service search bar. On the left side panel, choose Roles. Then, choose Create role.

2. Choose AWS service. Choose EC2. Choose Next: Permissions.

3. Next to Filter policies, search for amazons3full and choose AmazonS3FullAccess.

4. Next to Filter policies search for amazondynamodb and choose AmazonDynamoDBFullAccess.

5. Choose Next: Tags. Choose Next: Review.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

6. For Role name paste in S3DynamoDBFullAccessRole. Choose Create role.

> Note: Using full access policies are not something reccommended you should do in a production environment. We are using these policies as a proof of concept to get your demo up and running quickly. Once your Amazon S3 bucket and Amazon DynamoDB table are created, you can come back and modify this IAM Role to have more specific and restrictive permissions. More on this later.

Lab Complete
	 Congratulations! You have completed the lab.




# Lab3: Exercise: Compute

https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/lab-3-compute.html

The exercises are designed to be completed in your AWS account, and will have an associated cost. For this reason, in addition to the written instructions, this course includes video recordings of the exercises. If you intend to attempt the exercises, familiarize yourself with AWS pricing, specifically Amazon EC2 pricing, Amazon S3 pricing, and Amazon DynamoDB pricing and the AWS Free Tier.

For this scenario, you will be creating the employee directory application using user data configured during the EC2 instance set up. Since this is a dry run, you will terminate the instance afterwards to prevent additional costs from occuring.

In this exercise, you will log into the console as the IAM admin user. You will then launch an EC2 instance using the IAM role you created in the previous lab. Finally, once you've created the employee directory application, you will stop and then terminate the instance.

Lab Steps
Stage 1 - Launch EC2 instance using role

1. Search for **EC2** in the search bar at the top. Choose **EC2**.

2. Select **Instances** on the left side panel, and then choose the **Launch instance** button.

3. Choose **Select** next to the first AMI, which should be **Amazon Linux 2 AMI (HVM), SSD Volume Type**.

4. Choose the **t2.micro** (Free tier eligible) as the **Type**. Choose **Next: Configure Instance Details**.

5. Leave the Network as the (default). Next to Subnet, choose the first subnet in the drop down list.

6. Next to Auto-assign Public IP choose Enable.

7. Next to IAM role choose the S3DynamoDBFullAccessRole.

8. Scroll down to Advanced Details. Paste in the following into the **User data** box:


```bash
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install mysql
pip3 install -r requirements.txt
amazon-linux-extras install epel
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=<INSERT REGION HERE>
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```

Change the following line to match your region:


Note: You can find this at the top right next to your user name.  

```bash
export AWS_DEFAULT_REGION=<INSERT REGION HERE>
```
Example:


Note: US West (Oregon)


```bash
export AWS_DEFAULT_REGION=us-west-2
```

Note: You will modify this User Data script again to use your Amazon S3 bucket in a later lab. For now, just leave the `${SUB_PHOTOS_BUCKET}` in the script.

9. Choose Next: Add Storage. Choose Next: Add Tags.

10. Choose Add Tag. Under Key paste in Name. Under Value paste in employee-directory-app.

11. Choose Next: Configure Security Group. For Security group name: paste in app-sg.

12. Choose Add Rule. For Type choose HTTP. For Source change to Anywhere. Then, next to the SSH rule, choose the X at the right to remove it as you will not need SSH access to the instance.

> Note: You may get a warning that you will no longer be able to SSH into your instance. This is fine - as you won't need that functionality for this course.

13. Choose Review and Launch. Choose Launch. Choose Create a new key pair. Under Key pair name paste in app-key-pair. Choose Download Key Pair. Finally choose Launch instances.

14. Scroll down, and choose View Instances. The instance should now show up under Instances. Wait for the Instance state to change to Running and the Status check to change to 2/2 checks passed.

> Note: Often, the status checks update and the UI does not. Feel free to refresh the page after a few minutes to minimize waiting.  

15. Next to Name, choose the checkbox to select the instance.  Under the Details tab, copy down the Public IPv4 address.

> Note: do not click the link to open the IPv4 address. Simply just copy the address and paste it into a new tab.

16. Paste it into a new browser tab/window. You should see a Employee Directory placeholder. Right now you will not be able to interact with it as it's not currently connected to our DynamoDB database.

Congrats! You've successfully created an EC2 instance hosting the employee directory application. After you've finished looking around, it's time to stop and terminate your instance, so that you don't incur future costs.

17. Back in the AWS Management Console, the employee-directory-app should still be selected. Now, choose Instance state at the top and choose Stop instance. Choose Stop. The Instance state will eventually go into the Stopped state.

18. Next, you will terminate the instance. Again, select the checkbox next to the instance Name. Choose Instance state and choose Terminate instance. Choose Terminate.

Lab Complete
	 Congratulations! You have completed the lab.


ssh into the instance with username: ec2-user
and run the command manually

```bash
sudo su root
cd /
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install mysql
pip3 install -r requirements.txt
amazon-linux-extras install epel
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=<INSERT REGION HERE>
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```


make sure the protocol prefix is http:// rather than the auto-used https://
