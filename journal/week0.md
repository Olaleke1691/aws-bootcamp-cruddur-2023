# Week 0 — Billing and Architecture

# CONCEPTUAL ARCHITECTURE

This was first on a napkin design 

![0051FF61-20E8-4436-880E-44948D2DEFB1](https://user-images.githubusercontent.com/48904934/219656176-426c325c-29b0-45f6-be89-0ea2052f9ae3.jpg)


And then was designed on LUCID https://lucid.app/lucidspark/d3dc7e8b-5e77-431e-8a6d-d8aa973ffa77/edit?beaconFlowId=848C969CF553466E&invitationId=inv_de2b00a3-2d20-4a99-bfa4-d9f5d0480734#

![image](https://user-images.githubusercontent.com/48904934/219655892-645a23c4-5553-4d1d-ba55-42d114b3e3df.png)

# Created an Admin user from my root account

To do this, I navigated to my root user account and navigated to the IAM entity from the global serach. 

<img width="872" alt="image" src="https://user-images.githubusercontent.com/48904934/219631757-c0d0a920-0d12-41fa-a502-56904351e7f7.png">

Created a user, added the user to a newly created group, configured MFA & access keys, granted the user Admin access and access to Billing.

<img width="909" alt="image" src="https://user-images.githubusercontent.com/48904934/219633785-fa031482-f304-4b7f-9563-da94535e1636.png">

<img width="889" alt="image" src="https://user-images.githubusercontent.com/48904934/219634123-0effd131-4d40-4c50-af07-b30848df4b8b.png">

After doing this, clicked on Security Credentials and Create access key. This process alows us to get the caller identity of a the active account in the AWS CLI.

<img width="740" alt="image" src="https://user-images.githubusercontent.com/48904934/219634851-ac83eb56-f3ad-4e50-8a61-0fe66e0f9c0c.png">


After doing this, I logged out as the root user and logged in as the IAM user.

# INSTALL AND VERIFY AWS CLI.
Starting this step, I followed the AWS CLI installation documentation  https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

Selected the operating system we are going to be using for this project, which is LINUX.

Copy and pasted the commands line by line as this is best recommended. You can decide to install all the commands at once, but there may be a glitch in the installation process. 

The installation commands are as follows 
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# SETUP ENVIRONMENTAL VARIABLES.

When running the $aws sts get-caller-identity command after installing the CLI, i encounted an error, which is as a result of not declaring the environmental variable credentials. I got a prompt feedback stating that I should use ***aws config***. But since I set up my environment with Gitpod, I can not do that because Gitpod uses environmental variabled to execute.

To resolve this, I followed the guide in the documentation https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html 

Copied and pasted the commands in my Gitpod terminal, line by line after inputing the values of the IAM credentials in the environemtal variables.
$ export AWS_ACCESS_KEY_ID="my access key"
$ export AWS_SECRET_ACCESS_KEY="my secret key"
$ export AWS_DEFAULT_REGION="data center location"

Checked to see if i set it right by typing **env | grep AWS_** and this printed my credentials been saved temporary in the environmental variables.

Used the command aws sts get-caller-identity, to see if my credentials would be printed and it outputed my User Id and other details.

For me not to always do this process of reminding GItpod of my credentials, I used the following commands to set Gitpod to always remember my credentials 
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=us-east-1

Added this to a script on my gitpod.yml file to make sure everytime i start up my workspace it loads these settings.

<img width="707" alt="image" src="https://user-images.githubusercontent.com/48904934/219643953-86e207b6-aabb-4b18-94aa-897373d46bee.png">

After this, I commited my changes.

# BILLING

Navigated to my AWS console and turned on billing alerts to receive notifications with my IAM user, since I have granted permissions to Billing.
Scrolled downl to Billing Preferences and checked the Received Billing Alerts.
Saved Preferences.

<img width="650" alt="image" src="https://user-images.githubusercontent.com/48904934/219646033-2c3bd383-f3a9-4c22-941a-2c312d20086f.png">


# CREATING A BILLING ALARM

To start with, we need to create a and SNS topic before we can create an alarm. The SNS is the tool which would  send us an alert when we get overbilled.

More details here https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html

Using the documentation for this setup from the AWS CLI https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html#examples , I navigated to the examples section and found the command aws sns create-topic --name Billing-Alarm.

<img width="605" alt="image" src="https://user-images.githubusercontent.com/48904934/219647862-aead3df0-c23a-4473-8dbf-7e3cd584e0de.png">

Using the command and modifying the values aws sns subscribe \
--topic-arn="arn:aws:sns:us-east-1:666970591496:Billing-Alarm"
--protocol=email
--notification-endpoint=olaopaolaleke9116@outlook.com

I had to do run this command one after the other, else it would throw an error. 

After running the commands line by line, i got a pending confirmation as seen below

<img width="709" alt="image" src="https://user-images.githubusercontent.com/48904934/219650667-f3516ae4-e08e-48f7-b3f8-18cbfc4b7f8d.png">

Received an email which says I should confirm to subscription.

Navigated to my sns service in the aws consile to see if my sns topic was created and I found it.

<img width="892" alt="image" src="https://user-images.githubusercontent.com/48904934/219651931-f2a460f2-8022-4c48-b8d8-b525eba169d2.png">




