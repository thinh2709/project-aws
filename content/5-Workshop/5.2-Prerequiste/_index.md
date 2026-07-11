---
title : "Prerequisites"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Prerequisites

To successfully complete this hands-on workshop to build the **Ticket-App** online event ticketing platform, you need to prepare your local development environment and configure the appropriate AWS account permissions.

---

#### 1. IAM Permissions

You need to use an IAM User or IAM Role with appropriate permissions to clean up and operate the resources in this workshop.

{{% notice note %}}
For the most convenient personal lab practice and to avoid permission errors (Access Denied), we recommend using **AdministratorAccess** permissions for your lab account.
{{% /notice %}}

If you want to configure a least privilege policy, follow these steps to create an IAM Policy on the AWS Console:

1. Sign in to the **AWS Management Console** and navigate to the **IAM** service.
2. Select **Policies** on the left navigation bar, then click **Create policy**.
3. In the policy creation interface, switch to the **JSON** editor, clear any default template code, and paste the following JSON policy block:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ElasticBeanstalkCleanup",
      "Effect": "Allow",
      "Action": [
        "elasticbeanstalk:TerminateEnvironment",
        "elasticbeanstalk:DeleteApplication",
        "elasticbeanstalk:DeleteApplicationVersion",
        "elasticbeanstalk:DeleteConfigurationTemplate"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EC2AndAutoScalingCleanup",
      "Effect": "Allow",
      "Action": [
        "ec2:TerminateInstances",
        "ec2:DeleteSecurityGroup",
        "ec2:DeleteSubnet",
        "ec2:DeleteVpc",
        "ec2:DeleteInternetGateway",
        "ec2:DetachInternetGateway",
        "ec2:DeleteNatGateway",
        "ec2:DeleteRouteTable",
        "ec2:DeleteRoute",
        "ec2:DisassociateRouteTable",
        "ec2:ReleaseAddress",
        "ec2:DeleteVpcEndpoints",
        "ec2:DescribeInstances",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeNatGateways",
        "ec2:DescribeRouteTables",
        "ec2:DescribeAddresses",
        "ec2:DescribeVpcEndpoints",
        "ec2:DescribeNetworkInterfaces",
        "autoscaling:DeleteAutoScalingGroup",
        "autoscaling:UpdateAutoScalingGroup",
        "autoscaling:DeleteLaunchConfiguration",
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeLaunchConfigurations"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
      "Sid": "RDSCleanup",
      "Effect": "Allow",
      "Action": [
        "rds:DeleteDBInstance",
        "rds:DeleteDBProxy",
        "rds:DeregisterDBProxyTargets",
        "rds:DeleteDBSubnetGroup",
        "rds:DescribeDBInstances",
        "rds:DescribeDBProxies",
        "rds:DescribeDBSubnetGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ElastiCacheCleanup",
      "Effect": "Allow",
      "Action": [
        "elasticache:DeleteReplicationGroup",
        "elasticache:DeleteCacheSubnetGroup",
        "elasticache:DescribeReplicationGroups",
        "elasticache:DescribeCacheSubnetGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "SQSCleanup",
      "Effect": "Allow",
      "Action": [
        "sqs:DeleteQueue",
        "sqs:GetQueueUrl",
        "sqs:ListQueues"
      ],
      "Resource": "*"
    },
    {
      "Sid": "SNSCleanup",
      "Effect": "Allow",
      "Action": [
        "sns:DeleteTopic",
        "sns:ListTopics"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CognitoCleanup",
      "Effect": "Allow",
      "Action": [
        "cognito-idp:DeleteUserPool",
        "cognito-idp:DescribeUserPool"
      ],
      "Resource": "*"
    },
    {
      "Sid": "S3Cleanup",
      "Effect": "Allow",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteObject",
        "s3:DeleteObjectVersion",
        "s3:ListBucket",
        "s3:ListBucketVersions",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Sid": "APIGatewayCleanup",
      "Effect": "Allow",
      "Action": [
        "apigateway:DELETE",
        "apigateway:GET"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudFrontCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudfront:DeleteDistribution",
        "cloudfront:GetDistribution",
        "cloudfront:UpdateDistribution"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodePipelineCleanup",
      "Effect": "Allow",
      "Action": [
        "codepipeline:DeletePipeline",
        "codepipeline:GetPipeline"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeBuildCleanup",
      "Effect": "Allow",
      "Action": [
        "codebuild:DeleteProject",
        "codebuild:BatchGetProjects"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeCommitCleanup",
      "Effect": "Allow",
      "Action": [
        "codecommit:DeleteRepository",
        "codecommit:GetRepository"
      ],
      "Resource": "*"
    },
    {
      "Sid": "SecretsManagerCleanup",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:DeleteSecret",
        "secretsmanager:ListSecrets"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DescribeAlarms",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMCleanup",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:DeleteInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile",
        "iam:DetachRolePolicy",
        "iam:ListRolePolicies",
        "iam:ListAttachedRolePolicies",
        "iam:GetRole",
        "iam:GetInstanceProfile"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ELBCleanup",
      "Effect": "Allow",
      "Action": [
        "elasticloadbalancing:DeleteLoadBalancer",
        "elasticloadbalancing:DeleteTargetGroup",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticloadbalancing:DescribeTargetGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "KMSDescribe",
      "Effect": "Allow",
      "Action": [
        "kms:DescribeKey",
        "kms:ListKeys"
      ],
      "Resource": "arn:aws:kms:us-east-1:<your-account-id>:key/*"
    }
  ]
}
```

5. Click **Next**.
6. Enter a name for the policy (e.g., `TicketAppCleanupPolicy`), add an optional description, and click **Create policy**.
7. Attach this policy to your **IAM User** (navigate to **Users** -> select your User -> **Add permissions** -> **Attach policies directly** and select the `TicketAppCleanupPolicy` you just created).



---

#### 2. Local Workspace Setup

Ensure your workstation has the following tools installed and configured:

##### A. Node.js & npm
*   The Ticket-App application is written in Node.js. You need Node.js version **18.x** or **20.x** (LTS).
*   Verify your installation:
    ```bash
    node -v
    npm -v
    ```

##### B. Git
*   Used to fetch the project source code.
*   Verify your installation:
    ```bash
    git --version
    ```

---

#### 3. Download Project Source Code

The Ticket-App source code is structured into three main directories:
1.  **ticket-booking-frontend/**: Static React UI showing event list and handling ticket purchases.
2.  **ticket-booking-backend/** (Backend API): Node.js API (Express) validating requests, authenticating users via Cognito, and publishing events to SQS.
3.  **ticket-booking-worker/** (Worker): Background processor consuming messages from SQS FIFO and writing transactions to the PostgreSQL DB.

Clone the project from your workshop repository:
```bash
git clone https://github.com/thinh2709/Project-FCAJ.git
cd Project-FCAJ
```



---

#### 4. Choose Infrastructure Deployment Method (VPC, RDS, Beanstalk, SQS,...)

In this workshop, you can choose one of the following two methods to build your network and server infrastructure:

*   **Option A (Recommended - Fast): Automate deployment using AWS CloudFormation**
    *   The entire system infrastructure will be automatically initialized from A to Z.
    *   All chapters from **5.3 to 5.8** will serve as guides for you to **Verify and Validate** the automatically created resources rather than manually configuring them.
*   **Option B: Manually configure step-by-step (Manual)**
    *   You will **skip** Step 4 (running CloudFormation) below.
    *   Starting from Chapter **5.3**, you will manually follow the instructions on the AWS Console to build the system from scratch.

---

#### Instructions for Option A: Automatic Deployment using CloudFormation

To prepare the environment for the workshop, we deploy the following CloudFormation template:

<div style="background-color: #f8f9fa; border: 1px solid #e9ecef; border-left: 5px solid #ff9900; padding: 20px; border-radius: 8px; margin: 20px 0; display: flex; flex-direction: column; gap: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05);">
  <div style="font-weight: 700; color: #232f3e; font-size: 1.1rem; line-height: 24px;">
    <img src="https://img.icons8.com/color/48/amazon-web-services.png" style="width: 24px !important; height: 24px !important; margin: 0 8px 0 0 !important; display: inline-block !important; vertical-align: middle !important;" alt="AWS"/>
    <span style="vertical-align: middle !important;">Quick Deploy with AWS CloudFormation</span>
    <span style="font-size: 0.8rem; background-color: #e6f4ea; color: #137333; padding: 4px 8px; border-radius: 4px; font-weight: 600; display: inline-flex; align-items: center; gap: 4px; vertical-align: middle !important; margin-left: 10px; border: none;">
      <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
      Verified & Tested in Real Environment
    </span>
  </div>
  <p style="margin: 0; font-size: 0.95rem; color: #545b64; line-height: 1.5;">
    Click the button below to open the <strong>Quick create stack</strong> wizard on your AWS Console (us-east-1). Keep all default options.
  </p>
  <div>
    <a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=ticket-app-stack&templateURL=https://fcaj-ticketing-templates.s3.amazonaws.com/template-cloudformation.yaml" target="_blank" style="display: inline-flex; align-items: center; gap: 8px; background-color: #ff9900; color: white !important; padding: 12px 24px; border-radius: 4px; font-weight: 700; text-decoration: none; transition: background-color 0.2s ease; font-size: 0.95rem; box-shadow: 0 2px 4px rgba(0,0,0,0.15);">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" style="margin-right: 4px;"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path><polyline points="15 3 21 3 21 9"></polyline><line x1="10" y1="14" x2="21" y2="3"></line></svg>
      Deploy TicketAppStack
    </a>
  </div>
</div>

{{% notice info %}}
**Note on Deployment Scope**: 
If you choose automatic deployment via CloudFormation, the entire system—ranging from network infrastructure, security, databases, to CI/CD—will be automatically initialized from A to Z. You can **skip all manual creation steps** throughout the workshop and simply read the instructions from **Chapter 5.3 to Chapter 5.8** to **Verify/Check** configurations of the pre-created resources.
{{% /notice %}}

1. The browser will open the **Quick create stack** page on the CloudFormation Console with the Template URL pre-filled.
2. Verify that the **Stack name** is set to `ticket-app-stack`.
3. Scroll down to the **Parameters** section and configure the parameters (you must change the email fields to your own information):
   * **EnvironmentName**: Keep the default `ticketing`.
   * **DBMasterUsername** & **DBName**: Keep the default `postgres` & `ticketing_db`.
   * **OpsNotificationEmail**: Change to your personal email address (used to receive system ops alerts).
   * **UserNotificationEmail**: Change to your personal email address (used to test receiving ticket booking confirmation emails).
   * **MailFrom**: Change to your personal email address (the sender address, which will be verified and configured with Amazon SES in later steps).
   * Other parameters (such as `SmtpUsername`, `SmtpPassword`, `MailFromName`, `ReservationTimeoutMinutes`...): Can be kept as default.
4. Once configured, check the box **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** in the Capabilities section at the bottom.
5. Click the **Create stack** button to start the deployment.
6. Wait for the initialization process to complete (the Stack status will change to `CREATE_COMPLETE`).

After the CloudFormation Stack deploys successfully, your entire system infrastructure is fully initialized from A to Z! In the following chapters (from **5.3** to **5.8**), if you choose **Option A**, you only need to read the instructions to **verify configurations** for automatically created resources without performing any manual configuration steps. You can proceed directly to **Chapter 5.9 (Test)** to start testing and validating the system.
