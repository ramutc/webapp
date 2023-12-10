Overview

In this tutorial, you will create a simple serverless web application that enables users to request unicorn rides from the Wild Rydes fleet. The application will present users with an HTML-based user interface for indicating the location where they would like to be picked up and will interact with a RESTful web service on the backend to submit the request and dispatch a nearby unicorn. The application will also provide facilities for users to register with the service and log in before requesting rides.

Prerequisites

To complete this tutorial, you will need an AWS account, AWS CLI installed, an account with ArcGIS to add mapping to your app, a text editor, and a web browser. If you don't already have an AWS account, you can follow the Setting Up Your AWS Environment getting started guide for a quick overview.

Application architecture

The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.


![image](https://github.com/ramutc/webapp/assets/151390614/0f47e142-7551-4426-8313-eab85d56005e)





















 








 
Static Web Hosting
AWS Amplify hosts static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser.
 
User Management
Amazon Cognito provides user management and authentication functions to secure the backend API.
 
Serverless Backend
Amazon DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.
 
RESTful API
JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway.

Module 1: Static Web Hosting with Continuous Deployment
To get started, you will configure AWS Amplify to host the static resources for your web application with continuous deployment built in

Overview:

In this module, you will configure AWS Amplify to host the static resources for your web application with continuous deployment built in. The Amplify Console provides a git-based workflow for continuous deployment and hosting of full-stack web apps. In subsequent modules, you will add dynamic functionality to these pages using JavaScript to call remote RESTful APIs built with AWS Lambda and Amazon API Gateway.



Architecture overview:

 ![image](https://github.com/ramutc/webapp/assets/151390614/f71325bf-f17d-4131-b55e-7ec40e47f3ac)



The architecture for this module is straightforward. All your static web content including HTML, CSS, JavaScript, images, and other files will be managed by AWS Amplify Console. Your end users will then access your site using the public website URL exposed by AWS Amplify Console. You don't need to run any web servers or use other services to make your site available.

Select a Region:
This web application can be deployed in any AWS Region that supports all the services used in this application, which include AWS Amplify, AWS Code Commit, Amazon Cognito, AWS Lambda, Amazon API Gateway, and Amazon DynamoDB. Select the regions as per your respective region.

 ![image](https://github.com/ramutc/webapp/assets/151390614/fc71eec0-15ae-4912-b1fb-96c8ae06baea)


AWS CLI install and update instructions:

Download and run the AWS CLI MSI installer for Windows (64-bit):
https://awscli.amazonaws.com/AWSCLIV2.msi






Create a New Repository in AWS CodeCommit using the name, “wildrydes-site”.

 ![image](https://github.com/ramutc/webapp/assets/151390614/6133614c-8cb8-4878-8a3c-73fc7378981a)


Once repository is created, set up an IAM user with Git credentials in the IAM console.

 ![image](https://github.com/ramutc/webapp/assets/151390614/62963248-e831-40b4-9d1b-c33cca827a4c)



In the terminal window you used to install AWS CLI, enter the command: aws configure

Enter the AWS Access Key ID and Secret Access Key you created. 

For Default region name enter the Region you initially selected to create your CodeCommit repository in.

Leave Default output format blank, and press enter. 

The following code block is an example of what you will see in your terminal window.

% aws configure
AWS Access Key ID [****************]: 
AWS Secret Access Key [****************]: 
Default region name [us-east-1]: ap-south-1
Default output format [None]:

Set up the git config credential helper in the terminal window. Using below commands.

git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true

Select Clone HTTPS from the Clone URL dropdown to copy the HTTPS URL.

 ![image](https://github.com/ramutc/webapp/assets/151390614/107eca44-b6ec-4bfd-80d1-1664d826ccd0)


From your terminal window run git clone and paste the HTTPS URL of the repository.

The following code block is an example of what you will see in your terminal window:

$ git clone https://git-codecommit. ap-south-1.amazonaws.com/v1/repos/wildrydes-site
Cloning into ‘wildrydes-site’...

Username for ‘https://git-codecommit. ap-south-1.amazonaws.com/v1/repos/wildrydes-site’: 

Enter the HTTPS Git credentials for AWS CodeCommit username you generated.

Password for ‘https://username@git-codecommit. ap-south-1.amazonaws.com/v1/repos/wildrydes-site’: Enter the HTTPS Git credentials for AWS CodeCommit password you generated.
warning: You appear to have cloned an empty repository.
Populate the Git Repository:


	Once you've used either AWS CodeCommit or GitHub.com to create your git repository and clone it locally, you need to copy the website content from an existing, publicly accessible S3 bucket associated with this tutorial and add the content to your repository.

Change directory into your repository and copy the static files from S3 using the following commands (make sure you change the Region in the following command to copy the files from the S3 bucket to the Region you selected at the beginning of this tutorial):

cd wildrydes-site

aws s3 cp s3://wildrydes-ap-south-1/WebApplication/1_StaticWebHosting/website ./ --recursive

Add, commit, and push the git files. 

The following code block is an example of what you will see in your terminal window:

Run a command, git add .

Run a command, git commit -m "new files"

 ![image](https://github.com/ramutc/webapp/assets/151390614/f368610e-46ff-4086-bc7b-351bb951df9f)



Run a command, git push

 ![image](https://github.com/ramutc/webapp/assets/151390614/e4fd74e4-8fe0-45cd-99d7-701fe0695740)


Enable Web Hosting with AWS Amplify Console: -

1.	Launch the AWS Amplify console. 
2.	Choose Get Started. 
3.	Under the Amplify Hosting Host your web app header, choose Get Started. 
4.	On the Get started with Amplify Hosting page, select AWS CodeCommit and choose Continue.
5.	On the Add repository branch step, select wildrydes-site from the Select a repository dropdown.
6.	In the Branch dropdown select master and choose Next. 


 ![image](https://github.com/ramutc/webapp/assets/151390614/ac34836d-ee99-4a5e-a5b3-42931961d9f3)


7.	On the Build settings page, leave all the defaults, select Allow AWS Amplify to automatically deploy all files hosted in your project root directory and choose Next.

      8.  On the Review page select Save and deploy.

The process takes a couple of minutes for Amplify Console to create the necessary resources and to deploy your code.





Once completed, select the site image, or the link underneath the thumbnail to launch your 
Wild Rydes site. If you select the link for master, you'll see the build and deployment details related to your branch.


 


![image](https://github.com/ramutc/webapp/assets/151390614/3e9f4fd1-ad7a-4721-b72f-c04a80bf4edf)

 


![image](https://github.com/ramutc/webapp/assets/151390614/b4eb1fae-cbe2-4e89-92af-82d58b7c13ed)









Module 2: Manage Users.

Overview:

In this module you'll create an Amazon Cognito user pool to manage your users' accounts. You'll deploy pages that enable customers to register as a new user, verify their email address, and sign into the site.

Architecture overview:

![image](https://github.com/ramutc/webapp/assets/151390614/d7ee3f31-03ff-4add-a575-39064ca2df9b)

 
When users visit your website, they will first register a new user account. For the purposes of this workshop, we'll only require them to provide an email address and password to register. However, you can configure Amazon Cognito to require additional attributes in your own applications.

After users submit their registration, Amazon Cognito will send a confirmation email with a verification code to the address they provided. To confirm their account, users will return to your site and enter their email address and the verification code they received. You can also confirm user accounts using the Amazon Cognito console with a fake email address for testing.

After users have a confirmed account (either using the email verification process or a manual confirmation through the console), they will be able to sign in. When users sign in, they enter their username (or email) and password. A JavaScript function then communicates with Amazon Cognito, authenticates using the Secure Remote Password protocol (SRP), and receives back a set of JSON Web Tokens (JWT). The JWTs contain claims about the identity of the user and will be used in the next module to authenticate against the RESTful API you build with Amazon API Gateway.


Implementation:

Use AWS CloudFormation Template, from File, Webapp.yaml

 




![image](https://github.com/ramutc/webapp/assets/151390614/7e5aa040-19fd-4507-8ab3-beaea9bbd398)














Module 3: Serverless Service Backend

You will use AWS Lambda and Amazon DynamoDB to build a backend process for handling requests for your web application.

Overview: -

In this module, you will use AWS Lambda and Amazon DynamoDB to build a backend process for handling requests for your web application. The browser application that you deployed in the first module allows users to request that a unicorn be sent to a location of their choice. To fulfil those requests, the JavaScript running in the browser will need to invoke a service running in the cloud.

Architecture overview: -

![image](https://github.com/ramutc/webapp/assets/151390614/94187cb7-a61c-48ee-8e80-11bdd0348658)

 

You will implement a Lambda function that will be invoked each time a user requests a unicorn. The function will select a unicorn from the fleet, record the request in a DynamoDB table, and then respond to the frontend application with details about the unicorn being dispatched.

The function is invoked from the browser using Amazon API Gateway. You'll implement that connection in the next module. For this module, you will just test your function in isolation.







Create a DynamoDB Table:

Use the Amazon DynamoDB console to create a new DynamoDB table. 

	In the Amazon DynamoDB console, choose Create table.
	For the Table name, enter Rides. This field is case sensitive.
	For the Partition key, enter RideId and select String for the key type. This field is case sensitive.
	In the Table settings section, ensure Default settings is selected, and choose Create table. 
	On the Tables page, wait for your table creation to complete. Once it is completed, the status will say Active. Select your table name.
	In the Overview tab > General Information section of your new table and choose Additional info. Copy the ARN. You will use this in the next section.

 ![image](https://github.com/ramutc/webapp/assets/151390614/89e338e6-360c-4b7d-9b51-529ef51df300)



Create a IAM Role for Lambda Function:

Every Lambda function has an IAM role associated with it. This role defines what other AWS services the function is allowed to interact with. For the purposes of this tutorial, you'll need to create an IAM role that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.

In the IAM console, select Roles in the left navigation pane and then choose Create Role.
In the Trusted Entity Type section, select AWS service. For Use case, select Lambda, then choose Next. 
Note: Selecting a role type automatically creates a trust policy for your role that allows AWS services to assume this role on your behalf. If you are creating this role using the CLI, AWS CloudFormation, or another mechanism, specify a trust policy directly.
	Enter AWSLambdaBasicExecutionRole in the filter text box and press Enter. 
	Select the checkbox next to the AWSLambdaBasicExecutionRole policy name and choose Next.
	Enter WildRydesLambda for the Role Name. Keep the default settings for the other parameters.
	Choose Create Role.
	In the filter box on the Roles page type WildRydesLambda and select the name of the role you just created.
	On the Permissions tab, under Add permissions, choose Create Inline Policy.
	In the Select a service section, type DynamoDB into the search bar, and select DynamoDB when it appears.
	Choose Select actions.
	In the Actions allowed section, type PutItem into the search bar and select the checkbox next to PutItem when it appears.
	In the Resources section, with the Specific option selected, choose the Add ARN link.
	Select the Text tab. Paste the ARN of the table you created in DynamoDB (Step 6 in the previous section), and choose Add ARNs.
	Choose Next.
	Enter DynamoDBWriteAccess for the policy name and choose Create policy.


 



![image](https://github.com/ramutc/webapp/assets/151390614/86674040-2a55-41a8-9a01-a7cc1d29ee4f)






Create a Lambda Function:

Use the AWS Lambda console to create a new Lambda function called RequestUnicorn that will process the API requests. Use the following requestUnicorn.js example implementation for your function code. Just copy and paste from that file into the AWS Lambda console's editor.

Make sure to configure your function to use the WildRydesLambda IAM role you created in the previous section.
	From the AWS Lambda console, choose Create a function.
	Keep the default Author from scratch card selected.
	Enter RequestUnicorn in the Function name field.
	Select Node.js 16.x for the Runtime (newer versions of Node.js will not work in this tutorial).
	Select Use an existing role from the Change default execution role dropdown.
	Select WildRydesLambda from the Existing Role dropdown.
	Click on Create function.

 
![image](https://github.com/ramutc/webapp/assets/151390614/c1984f08-6ee4-47c0-a178-b069d4f77880)



Validation:


	In the RequestUnicorn function you built in the previous section, choose Test in the Code source section, and select Configure test event from the dropdown.
	Keep the Create new event default selection.
	Enter TestRequestEvent in the Event name field.
	Copy and paste the following test event into the Event JSON section:



{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}


	. Choose Save.

	In the Code source section of your function, choose Test and select TestRequestEvent from the dropdown.

	On the Test tab, choose Test.

	In the Executing function:succeeded message that appears, expand the Details dropdown.

	Verify that the function result looks like the following:

{
    "statusCode": 201,
    "body": "{\"RideId\":\"SvLnijIAtg6inAFUBRT+Fg==\",\"Unicorn\":{\"Name\":\"Rocinante\",\"Color\":\"Yellow\",\"Gender\":\"Female\"},\"Eta\":\"30 seconds\"}",
    "headers": {
        "Access-Control-Allow-Origin": "*"
    }
}
 





![image](https://github.com/ramutc/webapp/assets/151390614/da36a7ef-fd5a-418f-b609-2106c1f91e54)
















Module 4: Deploy a RESTful API

You will use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API

Overview: -

In this module, you will use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API. This API will be accessible on the public Internet. It will be secured using the Amazon Cognito user pool you created in the previous module. Using this configuration, you will then turn your statically hosted website into a dynamic web application by adding client-side JavaScript that makes AJAX calls to the exposed APIs.

Architecture overview:

 ![image](https://github.com/ramutc/webapp/assets/151390614/2a02f18c-b758-4cb0-88ca-4ef9e4c56629)



The static website you deployed in the first module already has a page configured to interact with the API you will build in this module. The page at /ride.html has a simple map-based interface for requesting a unicorn ride. After authenticating using the /signin.html page, your users will be able to select their pickup location by clicking a point on the map and then requesting a ride by choosing the "Request Unicorn" button in the upper right corner.

This module will focus on the steps required to build the cloud components of the API, but if you're interested in how the browser code works that calls this API, you can inspect the ride.js file of the website. In this case, the application uses jQuery's ajax() method to make the remote request.


Create a new Rest API using API Gateway,

	In the Amazon API Gateway console, select APIs in the left navigation pane. 
	Choose Build under REST API. 
	In the Choose the protocol section, select REST. 
	In the Create new API section, select New API.
	In the Settings section, enter WildRydes for the API Name and select Edge optimized in the Endpoint Type dropdown.
	Note: Use edge-optimized endpoint types for public services being accessed from the Internet. Regional endpoints are typically used for APIs that are accessed primarily from within the same AWS Region.
	Choose Create API.


 





![image](https://github.com/ramutc/webapp/assets/151390614/9a863dbb-6385-4827-8c27-ff7d12f30993)









Create an Authorizer:

You must create an Amazon Cognito User Pools Authorizer. Amazon API Gateway uses JSON web tokens (JWT), which are returned by the Amazon Cognito User Pool (created in Module 2) to authenticate the API calls. In this section, we will be creating an Authorizer for the API, so we can make use of the user pool.

Use the following steps to configure the Authorizer in the Amazon API Gateway console:

	In the left navigation pane of the WildRydes API you just created, select Authorizers.
	Choose Create New Authorizer. 
	Enter WildRydes into the Authorizer Name field.
	Select Cognito as the Type. 
	Under Cognito User Pool, in the Region drop-down, select the same Region you have been using for the rest of the tutorial. Enter WildRydes in the Cognito User Pool name field. 
	Enter Authorization for the Token Source. 
	Choose Create.
	To verify the authorizer configuration, select Test. 
	Paste the Authorization Token copied from the ride.html webpage in the Validate your implementation section of Module 2 into the Authorization (header) field and verify that the HTTP status Response code is 200.
  

![image](https://github.com/ramutc/webapp/assets/151390614/e5d60ebf-d117-4335-a5ac-7a5af70d47b5)
 



Create a new resource method.

	In the left navigation pane of your WildRydes API, select Resources.
	From the Actions dropdown, select Create Resource.
	Enter ride as the Resource Name, which will automatically create the Resource Path /ride.
	Select the checkbox for Enable API Gateway CORS.
	Choose Create Resource.
	With the newly created /ride resource selected, from the Actions dropdown select Create Method.
	Select POST from the new dropdown that appears under OPTIONS, then select the checkmark icon.
	Select Lambda Function for the Integration type.
	Select the checkbox for Use Lambda Proxy integration.
	Select the same Region you have been using throughout the tutorial for Lambda Region.
	Enter RequestUnicorn for Lambda Function.
	Choose Save.
	Note: If you get an error that your function does not exist, check that the region you selected matches the one you used in the previous modules.
	When prompted to give Amazon API Gateway permission to invoke your function, choose OK.
	Select the Method Request card.
	Choose the pencil icon next to Authorization.
	Select the WildRydes Cognito user pool authorizer from the drop-down list and select the checkmark icon.


![image](https://github.com/ramutc/webapp/assets/151390614/77da7ff2-82e0-4668-a6d9-f9512e51007f)


![image](https://github.com/ramutc/webapp/assets/151390614/3e42c67c-9789-46a3-9455-7b8c1401463c)






In this step you will update the /js/config.js file in your website deployment to include the Invoke URL of the stage you just created. You will copy the Invoke URL directly from the top of the stage editor page on the Amazon API Gateway console and paste it into the invokeUrl key of your site's config.js file. Your config file will still contain the updates you made in the previous module for your Amazon Cognito userPoolID, userPoolClientID, and region.

	On your local machine, navigate to the js folder, and open the config.js file in a text editor of your choice
	Paste the Invoke URL you copied from the Amazon API Gateway console in the previous section into the invokeUrl value of the config.js file. 
	Save the file.


See the following example of a completed config.js file. Note, the actual values in your file will be different.

window._config = {

    cognito: {

        userPoolId: ‘ap-south-1_vNDYqlV4X', 

        userPoolClientId: ' ramu-at-535053841485', 

        region: ‘ap-south-1’ 

    }, 

    api: { 

        invokeUrl: https://6ctv4t1016.execute-api.eu-north-1.amazonaws.com/prod, 

    } 

};
