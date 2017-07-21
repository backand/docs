# The Backand Lambda Launcher

The Backand Lambda Launcher is a tool we developed to ease the process of executing your AWS Lambda functions. Through use of the Lambda Launcher, you can easily execute any AWS Lambda function in your AWS account. This provides a number of benefits over traditional Lambda execution:

* You can provide users with a simple interface to your Lambda function library, allowing for execution of a function with a single click
* You can use non-AWS security systems to control user access to your AWS Lambda functions, allowing you to expose these functions without also needing to grant a user access to the AWS Console
* You can dynamically show and hide functions in the interface based upon user authentication and authorization, giving you full control over access to your Lambda functions
* You can integrate this functionality within a Backand application to give you ready-made devops, allowing you to perform reporting and maintenance tags from anywhere you have an internet connection

## Configuration

In order to work with your Lambda functions in Backand, you'll first need to connect Backand to your AWS account. Below, we walk through the process of creating a security policy to govern your integration of AWS Lambda with Backand, creating a user with the security access necessary to connect to your Lambda functions from Backand, and finally configuring Backand to communicate with your AWS Lambda functions.

These next sections focus on configuring the Lambda Launcher tool from a number of different locations. In all cases, the process will have three primary steps:

1. Create an IAM Policy to control access to your account's Lambda functions
2. Create an IAM User that you can use to connect Backand to your AWS account
3. Connect AWS to Backand using the IAM User's access keys

In all cases, step 3 - pasting the credentials into Backand - is the same, and as such we present the instructions for that step in a section after the AWS-specific configuration instructions. To create the AWS IAM Policy and User, choose the section most appropriate to your organizational practices from the options below.

##Setting up IAM Access for the Lambda Launcher
The Backand Lambda Launcher requires the following IAM policies in order to work with your lambda functions:

* lambda:GetFunction - this is used to retrieve a specific function's details from your account
* lambda:InvokeAsync - this is used to asynchronously run your lambda functions
* lambda:InvokeFunction - this is used to run your lambda functions synchronously
* lambda:ListFunctions - this is used to populate the list of Lambda functions available in your AWS account.

Backand takes security very seriously - all credentials entered are encrypted, and never used for any purpose other than executing your Lambda functions. If you would like further assistance in setting up the security policy, please [contact us](https://www.backand.com/contact).

### IAM Policy JSON
> The following JSON is to be used when creating the IAM security policy, to manage Backand's access to your AWS Lambda functions

```json--persistent
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1495964947000",
            "Effect": "Allow",
            "Action": [
                "lambda:GetFunction",
                "lambda:InvokeAsync",
                "lambda:InvokeFunction",
                "lambda:ListFunctions"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

The Backand Lambda integration relies on the policy JSON provided to the right. This grants Backand the capability to fetch a list of functions, fetch an individual functions, and invoke a function both synchronously and asynchronously.

### From AWS CloudFormation

If you have admin access to your organization's AWS account, you can use **AWS CloudFormation** to easily create the appropriate IAM policies and users for Backand's external function links.

<aside class="success">Please note: if you do not have admin access to your AWS account, we can walk you through a modified version of this process. Contact us at <a href="mailto:support@backand.com">support@backand.com</a> for further assistance.</aside>

Run this command using the AWS CLI, and we create an IAM user that only has the 'GetFunction', 'InvokeFunction' and 'ListFunctions' policies:

`aws cloudformation create-stack --template-url https://s3.amazonaws.com/cdn.backand.net/cf/1/backand-rc --stack-name createbackanduser --capabilities CAPABILITY_IAM`

Once the IAM user has been created, use this command to obtain the user's access and secret keys, then copy these values into the **Connection Details** dialog in the Backand dashboard, under **Functions & Integrations -> Functions -> External Functions**:

`aws cloudformation describe-stacks --stack-name createbackanduser`

<aside class="notice">IAM user creation can take up to a minute. Check the StackStatus parameter for the new stack - it will report "CREATE_IN_PROGRESS" while the creation is still underway. Once creation is completed, you will receive the IAM credentials for the user after calling describe-stacks</aside>

### From the AWS Console
If you're already logged into the AWS Console for your region, use the following steps to connect your AWS Lambda functions to Backand:

#### Create the IAM Security Policy
1. Open the IAM service panel
2. In the navigation menu on the left, select `Policies`
3. Click the `Create Policy` button
![image](aws_console/aws-IAM-Policy-add.png)
4. For "Step 1 : Create Policy," select `Create Your Own Policy`
5. Set the policy name to `BackandLambdaReadExecuteAccess`
6. Paste the [policy JSON](#iam-policy-json) into the area provided for the Policy Document
![image](aws_console/aws-IAM-Policy-add-3.png)
7. Once the JSON is ready, click `Create Policy`

#### Create the IAM User
1. On the navigation menu on the left, select `Users`
2. Click `Add User`
![image](aws_console/aws-IAM-users-add.png)
3. Enter `backand_lambda_user` as the user name
4. Check the `Programmatic access` option
![image](aws_console/aws-IAM-users-add-1.png)
5. Click on `Next: Permissions`
6. Click on `Attach existing policies directly`
7. Search for `BackandLambdaReadExecuteAccess`, and select it from the search results using the provided checkbox
8. Click `Next : Review`
9. click `Create User`
10. On the `Complete` page of the wizard, click the `Show` link in the table column `Secret access key`
11. Copy the "Access key ID" and "Secret access key" values to use with Backand.

### From the AWS Lambda Service panel
Using AWS Lambda, you can leverage a python script that we created which goes through the process of creating the IAM Policy and User on your behalf.

#### Lambda Python Script
```python
from __future__ import print_function

import json
import boto3

print('Loading function')

s3 = boto3.client('s3')


def lambda_handler(event, context):

    settings = {
        "name" :"backand_lambda_user",
        "policyName":"BackandLambdaReadExecuteAccess",
        "policy": ""
    }
    settings["policy"] = json.dumps({
          'Version': '2012-10-17',
          'Statement': [
            {
              'Sid': 'Stmt1494926726830',
              'Action': [
                'lambda:GetFunction',
                'lambda:InvokeAsync',
                'lambda:InvokeFunction',
                'lambda:ListFunctions'
              ],
              'Effect': 'Allow',
              'Resource': '*'
            }
          ]
        })
    client = boto3.client('iam')
    response = client.create_user(
        UserName=settings["name"]
    )
    print(response);
    accessReponse = client.create_access_key(
        UserName=settings["name"]
    )
    print(accessReponse)
    policy = client.create_policy(
        PolicyName= settings["policyName"],
        PolicyDocument=settings["policy"]
    )
    print(policy)
    user_policy = client.attach_user_policy(
        UserName=settings["name"],
        PolicyArn=policy["Policy"]["Arn"]
    )
    return {
        'AccessKey': accessReponse["AccessKey"]["AccessKeyId"],
        "SecretKey":accessReponse["AccessKey"]["SecretAccessKey"]

    }
```

This approach uses a python script to create the appropriate IAM resources. This script is provided in the right-hand panel, and should be used as the body of the Lambda action that will perform the required resource allocation in IAM.

#### Create the Lambda Function
1. go to Lambda service panel
2. Select the "Blank Function and click "Next"
3. name your function "createBackandLambdaUser"
4. Select the "Python 3.6" runtime
5. paste the [lambda python code](#lambda-python-script) into the provided area.
![image](aws_lambda/aws-lambda-config-1.png)
6. in the Role dropdown select "Create Custome Role"
7. in the "AWS Lambda requires access to your resources" window, select "Create new Role Policy"
8. click "View Policy Document" and click the Edit button on the right
![image](aws_lambda/aws-lambda-role.png)
9. approve the "Edit Policy" pop-up
![image](aws_lambda/aws-lambda-role-1.png)
10. Paste the [policy JSON](#iam-policy-json) into the area provided for the Policy Document and click "Allow"
![image](aws_lambda/aws-lambda-role-2.png)
11. click "Next"
12. click "Create  Function"
13. after the function was created , click "Test",
![image](aws_lambda/aws-lambda-config-2.png)
11. Copy the "Access key ID" and "Secret access key" values to use with Backand.

### From the AWS CLI
If you primarily use the AWS CLI to interact with AWS, you can easily connect Backand to your AWS account using the following steps:

#### Create the IAM Policy and User

First, ensure that all files involved are saved in an ASCII-encoded format. Before starting, you'll need to create a new file in the current directroy - `createBackandLambdaUser.json` - and populate it with the [policy JSON](#iam-policy-json) before starting. Then, run the following series of commands to create the IAM Policy and User in your account:

<aside class="notice">Be sure to replace &lt;&lt;ACCOUNT_NUMBER&gt;&gt; with your AWS Account Number.</aside>

1. aws iam create-policy --policy-name BackandLambdaReadExecuteAccess --policy-document file://createBackandLambdaUser.json ( and copy the policy arn for step 4)arn:aws:iam::<<ACCOUNT_NUMBER>>:policy/BackandLambdaReadExecuteAccess
2. aws iam create-user --user-name backand_lambda_user
3. aws iam attach-user-policy --user-name backand_lambda_user --policy-arn arn:aws:iam::<<ACCOUNT_NUMBER>>:policy/BackandLambdaReadExecuteAccess
4. aws iam create-access-key --user-name backand_lambda_user
5. Copy the "Access key ID" and "Secret access key" values to use with Backand.

### Creating Cross-Account Access for AWS Lambda

You can create an IAM role and user that span multiple AWS accounts, allowing you to bring your lambda functions from multiple AWS accounts into a single interface. This method creates a restricted linkage between your AWS account and Backand, allowing you to import your personal Lambda functions into your Backand application.

#### Obtaining the AWS Account ID

This approach relies upon obtaining your AWS Account ID. To do this:

- Navigate to the AWS Console for your region.
- In the top navigation bar, select **Support**, then **Support Center**.
- Your AWS Account ID is located in the upper-right-hand corner, immediately below the **Support** menu. It is a 12-digit number.

Save this ID for use in later configuration steps.

#### AWS CLI

Using CloudFormation, we'll create a stack that we'll use to drive cross-account integration with Backand. Use the following AWS CLI command to get started:

<aside class="notice">You'll need to replace the value <code>{{appMasterToken}}</code> in this command with your application's <a href="#social-amp-keys">master token</a>.</aside>

`aws cloudformation create-stack --template-url https://s3.amazonaws.com/cdn.backand.net/cf/1/backand-cc --parameters  ParameterKey=ExternalId,ParameterValue=bknd_{{appMasterToken}} --stack-name createBkndCrossAccess --capabilities CAPABILITY_NAMED_IAM`

#### AWS Management Console

Once you've created the stack, open the AWS Management Console as an account administrator, and pull up the IAM console. Before you create the role, you'll need to create a managed policy that defines the permissions required by the IAM role to be created. You will attach this policy to the new role in a later step.

In the navigation pane, on the left side of the screen, select **Policies**, and then select **Create Policy**

##### Step 1 - Create Policy
```json--persistent
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1500562449000",
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "lambda:GetFunction",
                "lambda:InvokeFunction",
                "lambda:InvokeAsync",
                "lambda:ListFunctions",
                "lambda:ListVersionsByFunction"
            ],
            "Resource": "*"
        }
    ]
}
```

For **Step 1: Create Policy**, select **Create Your own Policy**. Name the policy `BackandCrossAccountPolicy`.

Next, set the Policy Document to the JSON on the right. Once this is done, the policy should appear in your list of managed policies.

When you've completed your changes, the screen should look like the following:

![image](images/AWS-CAA-CreatePolicy.png)

##### Step 2 - Create a Role

The next step is to create an IAM Role.

1. Open the IAM Console for this account
2. Click **Roles**
3. For **Step 1: Select Role Type**, select and expand the "Role for cross-account access" option.
4. Select **Provide access between your AWS account and a third party AWS account**
​![image](images/AWS-CAA-roleType.png)
5. On the next page, set Backand's account ID and external ID to the following values:
  - **Account ID**: `328923390206`
  - **External ID**: `bknd_{{appsMasterToken}}`
  ![image](images/AWS-CAA-AccountID.png)
<aside class="success"> Note that for now, you do not need to require users to have Multi-Factor Authentication enabled in order to assume the cross-account role</aside>
6. Click **Next Step**
7. Attach the policy created above by selecting it from the provided list
8. Click **Next Step**
9. Set the role name to `BackandCrossAccountRole`
![image](images/AWS-CAA-Createe.png)
10. Click **Create Role**

#### Reference documentation

- http://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html
- https://aws.amazon.com/blogs/security/enable-a-new-feature-in-the-aws-management-console-cross-account-access/
- http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html





### Connect the IAM User to Backand
If you are configuring your Lambda Launcher access for the first time, via the modal dialog, you can simply paste your Access Key ID and Secret Access Key into the provided fields. If you have an existing app, you can update those from the External Functions page:

1. Open the Backand App Dashboard
2. Select the `Functions & Integrations` tab, if it is not already selected
3. Select `External Functions`, under `Functions` in the left-hand navigation menu
4. Provide the Access Key, Secret Key, and Region in the provided fields
5. Press `Link`

At this point, your AWS Lambda functions should be listed in the Backand interface.
