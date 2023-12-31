# Sending Bulk Emails with Amazon Simple Email Service(SES) using AWS Lambda and Amazon CodeWhisperer

Bulk Emailing with Amazon Web Services- AWS Lambda, SES, CloudWatch, IAMRoles and CodeWhisperer

The world of communication has evolved significantly in recent times, and businesses, organizations, and individuals often find themselves needing to send a large number of emails. However, manually managing this task can be tedious, time-consuming, and prone to errors. This is where Amazon Web Services (AWS) comes to the rescue, offering a seamless solution to automate mass emailing. In this blog, we will explore how to leverage AWS services, including AWS Lambda, CloudWatch, IAM Roles, and Simple Email Service (SES),and CodeWhisperer to create a robust and efficient mass emailing system with code companion.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/cff89ee7-57df-4d7b-8fec-4240d9e2569c" width="500">

#### Step 1: Creating an IAM Role

To kickstart our journey into bulk emailing automation with AWS, the first step is setting up an Identity and Access Management (IAM) role. This role ensures secure access to AWS resources while executing your bulk emailing tasks.<br>

* Open the AWS console, and in the search bar, type 'IAM' and select the IAM service.<br>
* In the IAM dashboard, navigate to 'Roles' and click on 'Create role.'<br>
* Select the use case as 'Lambda' and proceed to the next step.<br>
* In the 'Permissions' section, choose 'CloudWatch full access','AWSLambdaExecute' and 'SES full access' policies to grant the role the necessary permissions.<br>
* Provide a meaningful name for the role and click 'Create role.'<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/541263c4-48bb-4c44-a301-068008c90cb8" width="700">


#### Step 2: Verify email addresses or domain to send emails through Amazon SES <br>

You need to verify your email addresses or domain to send emails through Amazon SES.<br>

To verify your identity and send emails through Amazon Simple Email Service (SES), follow these steps:<br>
* Going to the Amazon SES console.<br>
* Going to Verified identities > create identity> select an individual email address.<br>

<img src="https://github.com/sreedevi-langoju/Mass_Emailing_Using_AWSServices/assets/135724041/46225b24-9db6-47c5-85a1-aad2c92b5222" width="600">

* Enter your email address to verify and then click on Create identity.<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/73b2fe8c-1616-4976-9bb1-3d45eb45457e" width="600">

* Amazon SES will send a confirmation email to the address you entered. Open your email client, find the email from AWS Notifications, and click on the link to confirm the email address. This link will take you to a confirmation page on the AWS website.<br>
* Once you've clicked the confirmation link, you'll receive a confirmation success message, and the email address will be marked as verified in the SES console.
You can now use the verified email address or domain to send emails through Amazon SES. <br>


<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3289c9f6-8432-4a75-bc9a-93bd9b3c9f36"  width="600">

<br>

* If you want to verify an entire domain (e.g., yourcompany.com) instead of individual email addresses, you can do this from the SES console. Navigate to "Domains" and follow the steps to verify the domain. This involves adding DNS records to your domain's DNS settings.<br>

* Before sending production emails, consider sending a test email to verify that your setup works as expected. Use the "Send a Test Email" option in the SES console.<br>

Remember that SES has both a sandbox and a production environment. In the sandbox, you can only send emails to verified email addresses or domains. To move to the production environment and send emails to any recipient, you must submit a request to Amazon SES for production access. Once approved, you can send emails to anyone, but you'll be responsible for following anti-spam regulations and best practices.<br>

#### Step 3: Creating a Lambda Function

AWS Lambda is a serverless computing service that allows you to execute code without the hassle of managing servers. This makes it an excellent choice for automating email sending.<br>

* In the AWS console, search for 'Lambda' and select the Lambda service.<br>
* Choose Author from scratch option and provide a name for your Lambda function.<br>
* Choose the desired runtime for your function; for instance, Python 3.11.<br>
* Select the existing role you created in the previous step.<br>
* You can leave the rest of the settings as optional.<br>
* Click 'Create function.'<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/d45c1c94-3b84-47a7-9853-005573484105" width="600" height=600>


* Now that you have your Lambda function in place, you'll need to write code to send emails using it. We are going to use Amazon CodeWhisperer to code faster.
  
* Amazon CodeWhisperer is a general purpose, machine learning-powered code generator that provides you with code recommendations in real time. As you write code,   
   CodeWhisperer automatically generates suggestions based on your existing code and comments.
  
* Check this link https://docs.aws.amazon.com/codewhisperer/latest/userguide/lambda-setup.html for the instructions to enable CodeWhisperer in Lambda console.You can see in Tools tab -> Amazon CodeWhisperer Suggestions option, enable it.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/1da91de2-8385-4fb7-a5c9-d5dc87109756" width=600 >

* When you start typing out single lines of code, CodeWhisperer makes suggestions based on your current and previous inputs.You can provide single line comment or block of statements to get the code suggestions.Use below keys combinations to get or accept Code Whisperer suggestions.Please check below link for more details : https://docs.aws.amazon.com/codewhisperer/latest/userguide/whisper-code-examples.html
<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/9102aefe-b47f-4e99-974c-ba1bff6f0e43" width=600>

* I used the multi-line comment : '''
Write lambda_handler function send to multiple emails at a time using Amazon Simple Email Service 
Use environmental variables for SES_REGION, SES_SOURCE_EMAIL, SES_DESTINATION_EMAILS without accesskeys
And use SES_DESTINATION_EMAILS as list of emails in try exception block
''' to get the code suggestions.

 <img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3bc69cab-9d9e-4e9d-8895-6424d7d6a41d" width=600>

* To provide environmental variables to lambda function, select your Lambda function.

* select the "Configuration" tab. Scroll down to the "Environment variables" section.
* Click on the "Edit" button to add or modify environmental variables.
* Add Key-Value pairs: Here, you can add key-value pairs for our environmental variables SES_REGION, SES_SOURCE_EMAIL, SES_DESTINATION_EMAILS.click on "Save" to apply the changes.

    For example: SES_REGION : us-east-1 <br>
                SES_SOURCE_EMAIL : test1@gmail.com <br>
                SES_DESTINATION_EMAILS: test2@gmail.com,test3@gmail.com,test4@gmail.com <br>
  
    Note: The email ids you are providing should be verified identities of in Amazon SES .

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/fc7d5001-b217-4019-9599-6b7979a5d16d" width=600>
<br>

#### Step 4: Creating CloudWatch Events

AWS CloudWatch is a monitoring service that provides valuable insights into your AWS resources and allows you to schedule tasks, making it an ideal choice for automating email dispatch at specific times.<br>
In the Lambda console, select the Lambda function to which you want to add a CloudWatch Event Rule.<br>

* On the Lambda function configuration page,  click the "Add trigger" button.<br>
* In the "Add triggers" section, choose "CloudWatch Events" as the trigger source.<br>
* In the "Configure triggers" section, select "Create a new rule."<br>
* Provide a Rule name and description.<br>
* Choose the Rule type as "scheduled expression."<br>
* In the Schedule expression field, specify the cron or rate expression, e.g., rate(1 minute) for a 1-minute interval.<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/52c611f1-7e9d-4a69-aca8-736e994bf21c" width="600">

* Click the "Add" button to add the CloudWatch Event Rule as a trigger for your Lambda function.<br>
* After adding the rule, you'll see it listed as a trigger in the Lambda function's configuration page.<br>
* Click "Save" to save the changes to your Lambda function.<br>

Your Lambda function is now associated with the CloudWatch Event Rule as a trigger, and it will be invoked based on the schedule you defined in the rule. Test and monitor your Lambda function to ensure it works as expected when triggered by the CloudWatch Event Rule.<br>


<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/568d64e4-4fda-4c45-a819-fb70eccfddb8"  width="400">

As a result of these steps, your Lambda function will be triggered, and it will send out emails at the scheduled times, making bulk emailing a breeze. Check the email inboxes , you should be receiving the emails from SES.<br>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/7b948d1c-a1ee-465d-b3fe-5f746688ef6b"  width="400">

#### Cleanup & Conclusion :

In conclusion, by integrating AWS Lambda, IAM roles, CloudWatch, and SES, you can establish an efficient and reliable solution for sending mass emails.

Cleaning up AWS resources after you're done using them is crucial to avoid unnecessary billing. Delete the Lambda function, IAM Role, and SES verified identities after completion of this implementation.
Remember, before deleting any resources, ensure they are not in use by any critical applications or services. Always take precautions to prevent accidental deletion of important data or resources. Keeping a clean AWS environment not only helps in cost management but also reduces security risks and complexity in your infrastructure.

