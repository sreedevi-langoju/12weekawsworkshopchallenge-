# Real-time data streaming with Amazon Kinesis Data streams and Amazon Kinesis Data Firehose

This  Whizlabs lab guides you through the process of deploying a sample website on an EC2 Linux instance, utilizing the Apache web server, and capturing real-time website logs. These logs are then streamed to AWS S3 for storage and analysis using a combination of Kinesis Data Streams, Kinesis Agent, Kinesis Firehose, and S3.

By following this, you will have the opportunity to practice working with these AWS services and create a comprehensive data pipeline for processing website logs.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/2e104918-f972-4cda-9240-b9b40ebe8b88">

## Case Study

1.Suppose an application is running on the EC2 Instance and it is generating continuous logs.

2.Those logs will be pushed into the Kinesis Data Streams.

3.From the Kinesis Data Streams, it gets consumed through the Kinesis Firehose.

4.The data from Kinesis Firehose is then saved into the S3 Bucket.



## Task 1: Sign in to AWS Management Console

Once Signed In to the AWS Management Console, Make the default AWS Region as US East (N. Virginia) us-east-1.

## Task 2: Launching an EC2 Instance

Make sure you are in US East (N. Virginia) us-east-1 Region. 

Navigate to EC2 by clicking on the Services menu at the top, then click on EC2 in the Compute section.

Navigate to Instances on the left panel and click on Launch Instance button.

 Enter Name as Demo_Instance

 Select Amazon Linux from the Quick Start.

Choose an Amazon Machine Image (AMI): Choose Amazon Linux 2 AMI(HVM) from the drop-down.

Choose an Instance Type: Select t2.micro 

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/746b51f6-ae3a-48a4-9ce9-f504d6d10714" width=400 height=400>

For Key pair : Choose Create a new key pair

Key Pair name :  Enter  Whizkey
Key pair type : Choose RSA
Private key file format: Choose .pem
Click on Create key pair button.
In Network Settings Click on Edit Button:

Auto-assign public IP: Select Enable
Select Create new Security group
Security group name : Enter kinesis_demo_SG
Description : Enter Security Group to allow traffic to EC2
SSH rule will already be present for you. To add HTTP:
 
Select Add Security rule Button
Choose Type: HTTP 
Source:  Select Anywhere
Under Advanced Details,
IAM Instance profile : Select EC2_Role_<RANDOM_NUMBER> 

Keep Rest thing Default and Click on Launch Instance Button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3b95489e-065a-4104-b2db-31880ac2db50" width=400 height=400>

Select View all Instances to View Instance you Created

 Launch Status: Your instances are now launching, Navigate to Instances page from left menu and wait the status of the EC2 Instance changes to running and health check status changes to 2/2 checks passed

Select the Instance and copy the Public IPv4 address from the Details section. Note down the Public IPv4 Address of your EC2 instance. A sample is shown in the screenshot below.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/381a257a-7ec9-4a4e-a72b-cdbeaf3ffafc" width=400 height=400>

## Task 3: SSH into EC2 Instance
Please follow the steps in SSH into EC2 Instance.
 

## Task 4: Host a sample website
In this task, we will host a sample website by navigating to the HTML folder present in the var directory and then we will fetch the sample site using the wget command. 

Switch to the root user :

    sudo -s
    
Run all the updates using the yum command:

    yum update -y
    
<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/717fc4de-5059-4943-8c50-65f686161801" width=400 height=400>
    
Install the LAMP server:

    sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
    
Install the HTTPD :

    sudo yum install -y httpd mariadb-server

Start the HTTPD server:

    sudo systemctl start httpd

Enable the HTTPD Server:

    sudo systemctl enable httpd

Navigate to the HTML folder path.

    cd /var/www/html

Let us download a sample website template. Here I am downloading a zip file from the site using wget.

    sudo wget https://www.free-css.com/assets/files/free-css-templates/download/page270/marvel.zip

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/932a03d3-4fb2-46bb-8217-fff187371fe4" width=400 height=400>  


You can check if it is downloaded in the HTML path using ls command.

Unzip the downloaded html template. Use the zip file name to unzip.

    sudo unzip marvel.zip

  <img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9f09ce0e-ca56-446f-b480-cb79e555a91a" width=300 height=400>
  
Use the command ls to list all the files and folders present in the present working directory. 

You will be able to see a zip file and a folder. When we unzipped the marvel.zip, we got the folder, 2115_marvel. 

Copy the folder name to a text editor.

To verify if the sample website is hosted, paste http://IP_Address/folder_name in the browser and press [Enter]

   http://34.233.120.188/2115_marvel/


You can see that the website is hosted successfully.

   16. The website logs will be in the path “/var/log/httpd/access_log”. For each click and use of the website, the related logs will be collected and stored here.

   17. You can check the logs using the following commands

    sudo su
    cd /var/log/httpd/
    tail -10 access_log

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/faa9d0ba-3e76-46c0-959c-5515af3d5d86" width=400 height=400>

## Task 5: Set file permissions to httpd

Now let's see how to store these continuous logs. Before proceeding, change the permission of the httpd folder, so that the file will be in readable, writable, and executable mode by ec2-user.

Add the httpd group to your EC2 instance with the command

    groupadd httpd
    
Add ec2-user user to the httpd group with the command

    usermod -a -G httpd ec2-user
    
To refresh your permissions and include the new httpd group, log out completely with the command

    exit
    
exit (if you are in the sudo, you have to exit twice)

Follow the steps to SSH into EC2 Instance.

Verify that the httpd group exists with the groups with the command

    groups


Change the group ownership of the /var/log/httpd directory and its contents to the httpd group,

    sudo chown -R root:httpd /var/log/httpd
    
Change the directory permissions of /var/log/httpd and its subdirectories to add group write permissions and set the group ID on subdirectories created in the future,

    sudo chmod 2775 /var/log/httpd
    
find /var/log/httpd -type d -exec sudo chmod 2775 {} +

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/58f506a3-111f-469e-ba35-c24ab6804884" width=400 height=400>

## Task 6: Creating Kinesis Data Stream

Let us create a Kinesis data stream. The logs created in the EC2 sample website will be pushed to Kinesis data stream.

Make sure you are in the US East (N. Virginia) us-east-1 Region.

Navigate to Kinesis by clicking on the Services menu, under the Analytics section.

Under Get Started, select Kinesis Data Streams and click on Create data stream.

Under Data stream name, enter the Data stream name as whiz-data-stream

Click on Create data stream button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/f8c1bb5f-ddfb-433a-9d6e-740b74425b93" width=400 height=400>

Once the data stream is created, click to open it.

Click on the Configuration tab.

Scroll down to Encryption and click on Edit button.

Check Enable server-side encryption and use the default encryption key type, i.e Use AWS managed CMK.

Click on Save changes button.

You have used AWS KMS to encrypt your data.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/55c8bab6-0d08-4841-8f8e-14f3fd96f508" width=400 height=400>




## Task 7: Creating a S3 Bucket

In this task, we will create an S3 bucket where we will store the data from the firehose.

Make sure you are in the US East (N. Virginia) us-east-1 Region.

Navigate to S3 by clicking on the Services menu, under the Storage section.

Click on Create bucket button.

In the General Configuration,

Bucket name: Enter whiz-demo-logs

  Note: S3 Bucket names are globally unique, choose a name that is available.

Region: Select US East (N. Virginia) us-east-1 (i.e same region as the Kinesis data stream).

In the Default encryption,

Encryption key type: Leave the key type as Amazon S3 key (SSE-S3).

Bucket key: Select Enable

Click on Create bucket button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/e5cfe3af-d30b-4a3a-ac0e-8add2fc703c5" width=400 height=400>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/68f9591b-9b11-4562-8c4c-4a622c01b552"  width=400 height=400 >


## Task 8: Creating Kinesis Data Firehose

Once the streaming service gets the data from the logs, then we need to push the data somewhere. It is not possible to post the data from the Kinesis Data Streams. So we will use Kinesis Data Firehose.

Make sure you are in the US East (N. Virginia) us-east-1 Region.

Navigate to Kinesis by clicking on the Services menu, under the Analytics section.

Under Get Started, select Kinesis Data Firehose and click on Create delivery stream.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/4c9f921e-c08b-4c0c-a60a-d681228529dd" width=400 height=400>

Under Choose Source and Destination,

Source: Choose Amazon Kinesis Data Streams

Destination: Choose Amazon S3

Under the Source settings,

Click on Browse button.

From the pop-up, select the Data Stream we have created earlier

Click on Choose button.
 

 Under Delivery stream name, Enter Delivery stream name as whiz-delivery-stream

 Leave the Transform and convert records as default.

 Under the Destination settings,

Click on Browse button.

From the pop-up, select the S3 Bucket we have created earlier, in my case, whiz-demo-logs

Click on Choose button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3dd61dfd-4f1d-40f2-a1aa-2755a82920ec" width=400 height=400>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/05a9aeae-b446-40e3-b6e7-1b28a325c739" width=400 height=400>
 Expand the Buffer hints, compression and encryption section. Under the Buffer interval, make it to 60 seconds.

 Expand the Advanced settings,

Under Permissions, let it be as default, i.e Create or update IAM role. New IAM role with required permission would be created and will be assigned to this Kinesis delivery stream.

Click on Create delivery stream button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/52411a62-52b4-4476-8cc9-02b475dd6f8f">



## Task 9: Creating and configuring Kinesis Agent

Let us configure a Kinesis agent which will collect data and send it to Kinesis Data Streams.

 SSH into the EC2 instance.

Let us install the latest version of Kinesis agent on the instance.

    sudo yum install –y https://s3.amazonaws.com/streaming-data-agent/aws-kinesis-agent-latest.amzn2.noarch.rpm
    
Type “y” if asked in the installation process.

After installing the Kinesis agent, let us update the json file available in the path /etc/aws-kinesis/agent.json.

Edit the agent.json,

    sudo nano /etc/aws-kinesis/agent.json

Remove all the content and paste the below JSON content.

Note 1: Make sure you change the “awsAccessKeyId” , "awsSecretAccessKey"   in the JSON code wherever required.

Note 2: Make sure the “filePattern” consists of the log file path which is default in this case and “kinesisStream” consists of the created Kinesis Data Stream name.

Press “ctrl + x” to save. Press “y” to save the modified changes and press Enter (Follow the commands to save the file carefully).

```
{
    "cloudwatch.emitMetrics": true,
    "kinesis.endpoint": "",
    "firehose.endpoint": "",
    "awsAccessKeyId": "A*****************",
    "awsSecretAccessKey": "B******************",
    "flows": [
      {
        "filePattern": "/var/log/httpd/access_log",
        "kinesisStream": "whiz-data-stream",
        "partitionKeyOption": "RANDOM"
      }
    ]
}
```

The name of the kinesis stream ( “kinesisStream” ) to which the agent sends data. Change the kinesisStream name according to the name you created.

Whenever you change the configuration file (agent.json), you must stop and start the agent, using the commands.

    sudo service aws-kinesis-agent stop
    
    sudo service aws-kinesis-agent start
    
Once the agent is started for the first time, a log file will be created. Check the log file using the commands,

    cd /var/log/aws-kinesis-agent/
    
    ls -ltr

You can check if the service is started properly by going through the log.

    head -10 aws-kinesis-agent.log

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/a2143126-9859-4780-9f24-8237e4e94607" width=400 height=400>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/d8ca432b-b528-44d5-8c2f-fa7351928e51"  width=400 height=400 >

We can see that the agent is successfully started.


## Task 10: Testing the real-time streaming of data

Let us test by hosting the above sample website on multiple browsers or do some click activity on the website. The related logs will be collected on the listed S3 bucket.

To test the data streaming, paste your IP_Address/folder_name in the multiple browsers and press enter.

http://34.233.120.188/2115_marvel/

Note: The folder_name is the unzipped folder of the sample website which we have noted earlier in the Task 5.

Once you have followed the above step, click on the website links present to create more logs.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/c4384ac6-ecc8-4017-b791-ec4603e44069">

  Note: We are clicking the links in the sample website website to generate logs which will be streamed to the created S3 Bucket.
  Navigate to S3 by clicking on the Services menu, under the Storage section.

  Note: Wait for 3-5 minutes, if you are not able to see the logs. 
  
You will see a hierarchy of folders with year > month > date > hour.

Click on the date or hour to see the logs created.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/57db9e83-490e-45b7-9889-7ae3e4504c20" width=400 height=400>

Click on the log and select Open and save the file.

Open the log file in any text editor in the local and check the logs.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/100b2e96-239b-4cd1-ae32-441d20d8f024">

Note: The more the clicks in the page, the more the logs are generated. In this demo webpage, we have only 1 page. So try to open the webpage in many browsers and click on the links to generate the logs.

 
## Task 11: Checking the CloudWatch metrics of Kinesis Data Stream and Firehose


Let us the check the CloudWatch metrics of Kinesis Data Stream which records the data and Kinesis Delivery stream which reads the data from Data Stream.

Make sure you are in the US East (N. Virginia) us-east-1 Region.

Navigate to Kinesis by clicking on the Services menu, under the Analytics section.

Click on the created data stream and navigate to the Monitoring tab. You will be able to see the graph according to the logs generated.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/aea58bd8-baf7-47b1-bfcb-d10ef66662c8" width=400 height=400>

On the left navigation panel, click on the Data Firehose.

Click on the created delivery stream and navigate to the Monitoring tab. You will be able to see the graph.



<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9b9f6fc3-f11f-40fb-864b-cbe3dc60f54e" width=400 height=400>

Amazon Kinesis Data Streams supports the concept of shards, which are the fundamental units of data capacity within a stream. Each shard in a Kinesis Data Stream can handle up to 1 MB/sec of data input and 2 MB/sec of data output. However, with the use of Kinesis Data Firehose, you can easily ingest data into Kinesis Data Streams at a much higher rate, exceeding the individual shard limits.


## Task 12: Delete AWS Resources

### Terminating EC2 Instance

Make sure you are in the US East (N.Virginia) us-east-1 Region.

Navigate to the Services menu at the top left corner and click on EC2 present under the Compute section.

Click on Instances from the left navigation menu.

Select the created instance and click on Instance state.

From the drop-down menu select Terminate instance.

Confirm the terminate by clicking on Terminate button.



### Deleting Kinesis Data Streams

Make sure you are in the US East (N.Virginia) us-east-1 Region.

Navigate to Kinesis by clicking on the Services menu, under the Analytics section.

On the left panel, click on the Data streams.

Select the created data stream and click on the Actions button.

Select Delete from the drop-down.

Confirm by typing Delete and click on Delete button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/025e8c29-0d67-47be-8d2f-fb1531456ab0" width=400 height=400>

### Deleting Kinesis Delivery Streams

Make sure you are in the US East (N.Virginia) us-east-1 Region.

Navigate to Kinesis by clicking on the Services menu, under the Analytics section.

On the left panel, click on the Data Firehose.

Select the created delivery stream.

Select Delete.

Confirm by typing the delivery stream name in the field and click on Delete button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/2df1a6a8-9df4-47dd-99e0-858b4f02a318" width=400 height=400>

## Completion and Conclusion
You have successfully launched an EC2 Instance.

You have successfully hosted a sample website.

You have successfully set file permissions to httpd.

You have successfully created Kinesis data stream.

You have successfully created a S3 Bucket.

You have successfully created Kinesis Data Firehose.

You have successfully created and configured Kinesis Agent.

You have successfully tested the real-time streaming of data.

You have successfully checked the CloudWatch metrics of Kinesis Data Streams and Data Firehose.
