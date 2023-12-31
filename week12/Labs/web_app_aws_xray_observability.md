# Create a sample Web Application and monitor it in AWS X-Ray:

Let me walks you through the steps to deploy the sample web application using CloudFormation and monitor it using AWS X-Ray.

## Architecture Diagram:

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/b1237be4-e2c2-4a78-b822-d18a21fd8b67" width=500 height=400>


<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/cfd9f0ca-8730-4c88-9902-854dbd41ef7e" width=500 height=400>


## Task 1: Sign in to AWS Management Console
Click on the Open Console button, and you will get redirected to AWS Console in a new browser tab.

Once Signed In to the AWS Management Console, Make the default AWS Region as US East (N. Virginia) us-east-1.

## Task 2: Create an S3 bucket

In this task, we are going to create an S3 bucket by providing the required configurations and uploading the zip file as an object. 

Make sure you are in the US East (N. Virginia) us-east-1 Region.

Navigate to the Services menu at the top. Click on S3 in the Storage section.

On the S3 Page, click on Create bucket button and fill in the bucket details.

Bucket name: Enter mys3bucket<Random_Numbers>

Note: S3 bucket name is globally unique, choose a name which is available.

Region: Select US East (N. Virginia) us-east-1

    4. Leave other settings as default. 

    5. Click on Create bucket button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/47bd0ab0-2e95-4df6-9604-4a91782059b3">

    6. Click on the bucket name you created.

    7. Download this aws-xray-node-sample-app.zip( in this repository)  file and save it on your local machine.

    8. To upload this zip file to our S3 bucket,

Click on Upload button.

Click on Add files button.

Select the zip file. 

Click on the Upload button.

You can watch the progress of the upload from within the transfer panel at the top of the screen.

Once your file has been uploaded, it will be displayed in the bucket.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/269c9e82-ff96-4835-b99e-d77263dcbf7b">

Copy the Bucket name and Object name for later use.
         

## Task 3: Deploy a sample X-Ray application using CloudFormation

Navigate to CloudFormation by clicking on Services, and click on CloudFormation under the Management and Governance section.

Before creating CloudFormation Stack, download this template and save it on your local machine.

The CloudFormation template will create below resources:

* Elastic Beanstalk application: host sample X-Ray application.

* IAM Role: used by the EC2 in Elastic Beanstalk.

* DynamoDB: store signup details.

* S3 bucket: store the template.

* SQS: decouple the application.


On the CloudFormation dashboard, click on Create stack button.

Prepare Template: Select Template is ready

Specify template: Select Upload a template file

Click on Choose file and choose the xray-cf.yml from your local machine

Click on the Next button.
       

On the Specify stack details page, provide the following details:

Stack Name: Enter a name of your choice.

NodejsPlatformVersion: Enter latest version of Amazon Linux 2 Node.js 18 (Use this link to get the latest version) 

S3BucketName : Enter S3 Bucket Name

S3ObjectKey : Enter Object Name

Subnet: Select any subnet from the dropdown (any default subnet)

VPC: Select the default VPC from the dropdown

Click on the Next button.

 <img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/c5a4c64c-b887-4f85-ba76-c9518f1749bf">

On the Configure Stack options page, leave it as default and click on Next button in the bottom right corner.

Review the Stack, Check the acknowledgment at the bottom of the page, and then click on Submit button.

Once you click on Create Stack, your stack (xray-cf.yml) will start deploying the sample application. 

Initially, the stack status will be CREATE_IN_PROGRESS and it will create all the resources.

Wait for 10 minutes till the stack status changes to CREATE_COMPLETE.

Note: It will create an additional nested stack.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/331444f0-a3fa-43ca-aea7-c4f1741478be">

## Task 4: Test the Application

Go to EC2 Console by searching EC2 in the Services, and click on the Instances from the left side.

Select the mystack instance and Copy the Elastic IP Address  ( it will be the application Public IPv4 that you will use to access the application).

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/4d5fa846-28f1-4103-8a26-9ab3864eba45">

Open the IP in the new tab. It will open the sample X-ray application.

Let’s generate some traffic by signing up for the application.

For this step, you can manually signup into the application. But you will automate the process by clicking on the Start button.

Wait for a few minutes and then click on the Stop button.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/1152cc6b-28c5-4ee9-8e91-340903420737">

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/063b20a9-566b-4d4f-bb1f-e49f7c296c7e">

     7. Hence, you have successfully created a few signup accounts in the sample application.

     8. To view the signup details, navigate to DynamoDB by clicking on the Services menu at the top and search for DynamoDB in the search bar.

     9. Click on the Explore table items button from the left navigation panel and Select the table created. 

    10. You can view the signup details.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3f2a2e2f-0c5e-41ab-9d19-1c0cfb2cc763">

## Task 5: Monitor the traffic in AWS X-Ray

Go to the CloudWatch by clicking the Services menu at the top and search for CloudWatch in the search bar.

Now click on Service Map from left navigation menu under the X-Ray traces.

In the service map, you will get insight into the traffic that flows through our application.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/e5e89319-b1f0-4330-a233-fc54c3aade88">

Note: If you got a warning as Data not found, then click on 5 minutes at the top right corner and select 1 hour.


Now click on any of the components to view the response distribution.

It will show the traffic response status along with the graph.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/1943acbc-f475-4369-90cc-79310451c6df">

Click on the Analyze Traces to view detailed information.

It will show detailed information about HTTP Method, URL, User-Agent, HTTP Method, Trace List, etc.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/bebb6802-8e85-42e3-8d3b-9a6e28928c3e">

You can click on any of the Trace List to view the traces.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9bb4bcb2-240a-4e07-94a4-843022edc65b">

You can also filter the traces by clicking on Question mark(?) at the top right corner.


## Task 6: Clean up AWS Resources

Delete CloudFormation Stack

Make sure you are in the N.Virginia Region.

Navigate to CloudFormation by clicking on the Services menu at the top and search for CloudFormation in the search bar.

Select the mystack stack and click on the Delete button.

Now click on the Delete stack button in the prompt.

Now, wait till your stack deletion is completed. It will also delete the nested stack.

## Completion and Conclusion

You have successfully navigated to and deployed a sample X-ray application.

You have successfully tested the application and monitored the traffic in X-Ray.
