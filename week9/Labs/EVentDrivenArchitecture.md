# EventDriven_EmailNotification_AWS
Event Driven Notification System using Amazon S3, Lambda, SNS 

In this project, I built an event-driven architecture by harnessing the capabilities of AWS services. The process was initiated with S3 triggers, which were captured through a Lambda function. The captured data was then seamlessly passed to Amazon SNS, enabling the automated sending of customized email notifications to customers. This setup ensured timely and efficient communication with end-users and streamlined the event-based email notification process.

<img src="https://github.com/sreedevi-langoju/EventDriven_EmailNotifications_AWS/assets/135724041/f548b8ea-da08-4320-957d-a13ffd31f44a" width="600">

#### Step 1: Create an S3 Bucket

* Go to the AWS S3 console.
* Click "Create bucket" and follow the wizard to create a new bucket or select an existing one.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9ef6f87d-c6b8-4238-bade-2b5c82768f38">

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/0548476d-09d8-4d41-acb9-278842e58200">

  

#### Step 2: Create an SNS Topic

* Go to the AWS SNS console.
* Click "Create Topic" and follow the wizard to create a new SNS topic.
* Configure the topic with a name and display name. You will use this topic to send word count notifications.
* Need to subscribe to this SNS topic.
* Notedown the SNS Topic ARN value.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/5c300b9e-36f8-4816-8702-e6d51ba5b47a">

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/73cc472f-d934-43d6-b1f3-6a17d76bccd1">


<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9543a11b-be6a-4ce6-8180-77e65bb59801">

#### Step 3: Create an IAM Role for Lambda

* Go to the AWS IAM console.
* Create a new IAM role with permissions to execute Lambda functions, read from the S3 bucket, and publish to the SNS topic.
* Attach the following policies to the role:
* AWSLambdaBasicExecutionRole (for Lambda execution)
* AmazonS3ReadOnlyAccess (for reading from S3)
* AmazonSNSFullAccess (for publishing to SNS)

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/c87ceb05-6a01-4e34-a2e7-09dd5216f63e">


#### Step 4: Create the Lambda Function

* Go to the AWS Lambda console.
* Click "Create function" and select "Author from scratch."
* Configure your function:
* Name: Choose a name for your Lambda function.
* Runtime: Select the appropriate runtime for your code (e.g., Python, Node.js, etc.).Here I am choosing Python 3.9
* Execution role: Choose "Use an existing role" and select the IAM role created in Step 3.
* Click "Create function".

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/647a95a5-82bb-45ed-922a-136147723a86">

#### Step 5: Write Lambda Function Code
* Write the Lambda function code to count words in the uploaded text file. 
 (OR)
* Delete the existing code in the  Lambda Code area and copy the paste the python code from wordcount.py file in this repository.
* In the Function overview section, scroll down to the "Function code" panel.
* In the "Environment variables" section, click the "Edit" button to add environmental variables.
* Add a new environmental variable with a key like SNS_TOPIC_ARN and the value set to your SNS topic's ARN.
* Add SNS Topic ARN Variable
* Click "Save" to save the environmental variable.


<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/fed8dbb2-c818-42d6-a8c0-225576b1a915">

#### Step 6: Configure S3 Event Trigger for Lambda

* Go to your Lambda function in the AWS Lambda console.
* Click "Add trigger" and select "S3."
* Configure the trigger with the S3 bucket you created in Step 1 and set event type to "ObjectCreated (All)."
* Click "Add."

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/cb145bdc-1f8a-4d60-a9ab-ca1562c58960">

#### Step 7: Configure SNS Notification

* Go to your Lambda function in the AWS Lambda console.
* Click on the "Designer" tab.
* Click on the Lambda function box and then click on "Add destination."
* Choose "SNS" as the destination and select the SNS topic you created in Step 2.
* Click "Save" to configure the notification.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/b5d2346d-6bca-426c-958e-d1b33269cbcd">


#### Step 8: Upload a Text File to S3
Upload a text file to the S3 bucket. This will trigger the Lambda function, which will count the words and send a notification to the SNS topic.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/565b87d4-67d1-481b-bb8b-c55f405f6e38">


#### Step 9: Receive Word Count Report
The SNS topic will send a notification to the specified email address with the word count report whenever a text file is uploaded to the S3 bucket.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/6da07bc3-0e04-43a7-a3f1-0b2690b9f5fc">


You have now set up a Lambda function to count words in a text file, configured S3 to trigger the Lambda function, and created an SNS topic to report the word count via email notifications.

#### Step 10: Cleanup

Dont forget to delete the resources s3 bucket, SNS topic and Lambda function created earlier to avoid unnecessary billing.

