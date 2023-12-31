# AWS CloudFormation: Create a VPC with EC2 Instance using Nested Stacks

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/7fc97788-e6b7-40be-88fb-38d85661446c" width=900 height=600>

Here's a step-by-step guide to create a VPC with one public subnet, one private subnet, Internet gateway, route tables for each subnet and launch webserver - EC2 Linux instance in the public subnet of the created VPC using Nested stacks AWS Cloud formation service:

## Step 1: Create an S3 Bucket:

  * Log in to the AWS Management Console.
  * Navigate to the S3 service.
  * Create a new bucket: mycfcuket1116( bucket name should be unique)  to store your CloudFormation templates.
    
<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/0ee0bd57-cea7-4700-a40b-69b4f0734baf" width=400 height=400>

  
## Step2 :Upload Template Files to S3:

  * Upload three files ( parentstack.yaml, vpctsack.yaml and ec2instancestack.yaml) required for your nested stacks (VPC and EC2       
    templates) into the created S3 bucket. Please find the template files in this directory.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/c0fb8e6c-36eb-4675-b22e-4af34dccbb8d" width=400 height=400>
 

  * Retrieve the Object URLs of each uploaded file.
    
   Ex:  https://mycfbucket1116.s3.amazonaws.com/parentstack.yaml

        https://mycfbucket1116.s3.amazonaws.com/vpcstack.yaml

        https://mycfbucket1116.s3.amazonaws.com/Ec2Instancestack.yaml
 
  * Download the parentstack.yaml CloudFormation template to your local environment.
    

## Step 3: Edit the Parent Stack Template:

  * Open parentstack.yaml using a favorite text editor.
  * Locate the Resources section in the template.
  * Add the Object URLs of the VPC and EC2 files in the appropriate sections of the Resources using AWS::CloudFormation::Stack.
  * Save the edited parentstack.yaml.
  
<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/ad33f709-8041-404d-822c-ac24c1cc3670" width=400 height=400>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/a544c942-0761-4f50-a3f4-8a6902baa004" width=400 height=400>

  * Upload the updated parentstack.yaml to the same S3 bucket, replacing the old file.

   
## Step 4: Create CloudFormation Stack:

  * Go to the CloudFormation service in the AWS Management Console.
  * Click on "Create Stack".

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/0a7ebed0-e8a6-4cc2-b01a-698481cd32ad">

    
  * Choose the updated parentstack.yaml in the S3 location as the template source.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/13a867fa-06fe-47f6-bf7d-9461b600b872">
    
  * Provide a name for the stack. Ex: mycfparentstack
  * Fill in the required parameters:
  * Provide Ec2 Image(AMI)Id, EC2 Instance type, CIDR blocks for VPC, public and private subnets.
  * Specify the EC2 key pair name.
  * Enter your IP address for SSH port access.
  * Click "Next" and then "Create Stack".

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/decd6961-c289-46d6-8f10-1339bbe42dcd"> 
    
## Step 5: Monitor Stack Creation:

  * Wait for the CloudFormation stack to be created. Monitor the progress in the CloudFormation console.
  * Once successfully created

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/3a970d9e-360e-4c8b-970d-fb00458a7c84">

  * Check the outputs tab for VPC stack , you will find the created VPC Id and Public Subnet Id

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/e85fc901-10f3-49f5-8154-56813b54451b">

  * Check the outputs tab for EC2Instance stack , you will find the Public IP address of the EC2 Instance

 <img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/1425bc09-a085-4443-b03f-3665812a2086">

  * Goto VPC console andcheck the resources created:
    
    * check VPC with public and private subnets.
    * Internet Gateway attached to the VPC.
    * Route tables configured for subnets.   

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/60d01a86-bfc4-491b-bfb1-764596c93063">


    * EC2 instance launched in the public subnet with a security group configured.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/075533df-c8f7-4315-b22a-084f2f0a4946">

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/93e76cd8-0e35-4e53-bf16-3befbce4765c">

   * Copy the public IP address of the EC2 isnatnce and paste it on a web browser ,you will see the Helloworld message as below:
     
<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/415c1acb-056e-4ba5-bebb-a2bc48cf0b24">

    Test connectivity to the EC2 instance using SSH based on the security group settings.


## Step 6: Cleanup:

Ensure that the VPC, subnets, and EC2 instance are functioning as expected.Don't forget to delete the configured resources when they are no longer needed to avoid unnecessary billing.

If you delete the parent stack from the cloud formation, all the resources created with it will get deleted.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/d0a1d0e5-b663-4d40-b5de-9ffcfa20b252">

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/adbcc31c-dee1-4489-849e-e9fc46559b08">

Now delete the S3 bucket and objects int it.

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/7c1392e1-fcc3-470b-9d6b-01516d657fc9" width=400 height=400>

<img src="https://github.com/sreedevi-langoju/12weekawsworkshopchallenge-/assets/135724041/e8cab37c-df9f-4b80-9df0-a86a1e49971f" width=400 height=400>

##### Congratuations, you successfully created VPC and its compoennets and launch EC2 instance with in that VPC using AWS Cloud Formation service - Nested Stacks!!!
