---
title : "Resource Cleanup"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.10. </b> "
---

### Resource Cleanup

{{% notice warning %}}
To avoid incurring unexpected costs on your personal AWS account, please clean up all resources immediately after completing the hands-on lab.
{{% /notice %}}

Please choose the appropriate cleanup method based on the deployment method you used in Chapter **5.2**:

*   **Case A: If you deployed using CloudFormation (Option A in Chapter 5.2)**
    *   Do **NOT** manually delete individual resources first. Doing so will break the CloudFormation Stack's state, leading to errors where the Stack gets stuck or reports `DELETE_FAILED` during deletion.
    *   Scroll down and follow **Section 2. CloudFormation Cleanup Instructions**.
*   **Case B: If you manually created resources (Option B in Chapter 5.2)**
    *   Follow **Section 1. Clean Up Manually Deployed Resources**.

---

#### 1. Clean Up Manually Deployed Resources (For Option B)

Delete resources in the following order from top to bottom:

1. **Delete Elastic Beanstalk environments**:
   * Open the [Elastic Beanstalk console](https://us-east-1.console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/environments).
   * Select the ```ticket-app-Backend-env``` environment -> click **Actions** -> select **Terminate environment**.
   ![Terminate Backend Env](/images/5-Workshop/5.10-Cleanup/eb_1.jpg)
   ![Confirm Backend Env](/images/5-Workshop/5.10-Cleanup/eb_2.jpg)
   * Perform the same steps to terminate the ```ticket-app-Worker-env``` environment.
   ![Terminate Worker Env](/images/5-Workshop/5.10-Cleanup/eb_3.jpg)
   ![Confirm Worker Env](/images/5-Workshop/5.10-Cleanup/eb_4.jpg)
   * Wait until the environment termination is complete, then delete the Application: Navigate back to the **Applications** page -> select ```ticket-app-App``` -> click **Actions** -> **Delete application**.

2. **Delete API Gateway**:
   * Open the [API Gateway console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
   * Select your HTTP API (e.g., `TicketAppAPI`) -> click **Delete** -> Confirm deletion.
   ![Delete API Gateway](/images/5-Workshop/5.10-Cleanup/api_1.jpg)
   ![Confirm Delete API Gateway](/images/5-Workshop/5.10-Cleanup/api_2.jpg)

3. **Delete CloudFront Distribution**:
   * Open the [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
   * Select your Distribution -> click **Disable**. This process will take a few minutes.
   ![Disable CloudFront](/images/5-Workshop/5.10-Cleanup/cf_2.jpg)
   * Once the status changes to Disabled, select the Distribution again -> click **Delete**.
   ![Delete CloudFront](/images/5-Workshop/5.10-Cleanup/cf_1.jpg)
   {{% notice note %}}
   **Note:** If your account is subscribed to a CloudFront monthly package, you may not be able to delete this Distribution immediately.
   {{% /notice %}}

4. **Delete RDS Proxy & RDS PostgreSQL**:
   * Open the [Amazon RDS console](https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#databases:).
   * Select **Proxies** -> Select ```rds-proxy-ticket-app``` -> click **Actions** -> **Delete**.
   ![Delete RDS Proxy](/images/5-Workshop/5.10-Cleanup/rds_1.jpg)
   * Select **Databases** -> Select Database instance ```database-ticket-app``` -> click **Actions** -> **Delete**:
     * Select **No** for final snapshot (for fast deletion).
     * Check the acknowledgment box and type ```delete me``` to confirm.
   ![Delete RDS Database](/images/5-Workshop/5.10-Cleanup/rds_2.jpg)

5. **Delete ElastiCache Redis Replication Group**:
   * Open the [ElastiCache console](https://us-east-1.console.aws.amazon.com/elasticache/home?region=us-east-1#/clusters).
   * Select the Redis cluster ```ticket-app-redis``` -> click **Actions** -> **Delete**.
   ![Delete Redis](/images/5-Workshop/5.10-Cleanup/redis_1.jpg)

6. **Delete Cognito User Pool**:
   * Open the [Amazon Cognito console](https://us-east-1.console.aws.amazon.com/cognito/v2/home?region=us-east-1#).
   * Select User Pool ```ticket-app-user-pool``` -> click **Delete user pool**.
   ![Delete Cognito](/images/5-Workshop/5.10-Cleanup/cognito_1.jpg)

7. **Delete AWS CodePipeline, CodeBuild & CodeCommit**:
   * Open the **CodePipeline console**:
     * Select the `ticket-app-backend-pipeline` pipeline -> click **Delete**.
     * Repeat for `ticket-app-worker-pipeline`.
     ![Delete CodePipeline](/images/5-Workshop/5.10-Cleanup/ci_2.jpg)
   * Open the **CodeBuild console**:
     * Select the `ticket-app-backend-build` build project -> click **Delete build project**.
     * Repeat for `ticket-app-worker-build`.
     ![Delete CodeBuild](/images/5-Workshop/5.10-Cleanup/ci_1.jpg)
   * Open the **CodeCommit console**:
     * Select the `ticket-app-backend` repository -> click **Delete repository**.
     * Repeat for `ticket-app-worker`.
     ![Delete CodeCommit](/images/5-Workshop/5.10-Cleanup/ci_3.jpg)

8. **Delete Amazon SQS, SNS & CloudWatch Alarm**:
   * Open the **CloudWatch console** -> **Alarms**: Select the `ticket-app-checkout-dlq-alarm` alarm -> click **Delete**.
   ![Delete Alarm](/images/5-Workshop/5.10-Cleanup/misc_1.jpg)
   * Open the **SQS console**:
     * Select `booking-queue.fifo` -> click **Delete**.
     * Select `checkout-dlq.fifo` -> click **Delete**.
   ![Delete SQS](/images/5-Workshop/5.10-Cleanup/misc_3.jpg)
   * Open the **SNS console**:
     * Select the `ticket-app-notifications` topic -> click **Delete**.
   ![Delete SNS](/images/5-Workshop/5.10-Cleanup/misc_2.jpg)

9. **Delete SES Identity & SMTP Credentials**:
   * Open the **Amazon SES console**: Select **Identities** -> Select your Email -> click **Delete**.
   ![Delete SES](/images/5-Workshop/5.10-Cleanup/ses_1.jpg)
   * Open the **IAM console**: Select **Users** -> Select User `ticket-app-smtp-user` -> click **Delete**.
   ![Delete IAM](/images/5-Workshop/5.10-Cleanup/iam_1.png)

10. **Delete Secrets Manager**:
    * Open the [Secrets Manager console](https://us-east-1.console.aws.amazon.com/secretsmanager/home?region=us-east-1#!/listSecrets).
    * Select your secret (containing the DB password) -> click **Actions** -> **Delete secret**. (Enter a waiting period of 7 days).
    ![Delete Secret](/images/5-Workshop/5.10-Cleanup/sm_1.jpg)

11. **Delete S3 Buckets**:
    * Open the [Amazon S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#).
    * You must **Empty** (delete all files) the bucket before you can delete it:
      * Select the Frontend Bucket ```frontend-ticket-app-app-<your-account-id>``` -> click **Empty** -> enter `permanently delete` to confirm.
      * Perform the same for the Assets Bucket ```ticket-app-assets-<your-account-id>```.
      ![Empty S3](/images/5-Workshop/5.10-Cleanup/s3_1.jpg)
      * After emptying, select the Buckets -> click **Delete** -> enter the bucket name to confirm permanent deletion.
      ![Delete S3](/images/5-Workshop/5.10-Cleanup/s3_2.jpg)

12. **Delete Network Infrastructure (VPC, NAT, IGW, SGs)**:
    * Open the [VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1).
    * **NAT Gateways**: Delete the 2 NAT Gateways. (This process will take a few minutes).
    ![Delete NAT](/images/5-Workshop/5.10-Cleanup/vpc_1.jpg)
    * **Elastic IPs**: Release the 2 EIPs associated with the NATs.
    ![Release EIP](/images/5-Workshop/5.10-Cleanup/vpc_2.jpg)
    * **VPC**: Select VPC `ticket-app-vpc` -> click **Actions** -> **Delete VPC**. This action will automatically delete the associated Subnets, Route Tables, Internet Gateway, and Security Groups (as long as there are no dependent resources). Ensure all resources above (EC2, RDS, ElastiCache, Beanstalk, ELB) are completely deleted before performing this step.
    ![Delete VPC](/images/5-Workshop/5.10-Cleanup/vpc_3.jpg)

---

#### 2. CloudFormation Cleanup Instructions (For Option A)

If you deployed the core infrastructure using the CloudFormation template file in Chapter 5.2:

{{% notice warning %}}
**Important Note Before Deleting the Stack:**
Before clicking delete on the CloudFormation Stack, you **must** access the [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#) and **Empty** all S3 Buckets created by the stack (including the Frontend Bucket, Assets Bucket, and CodePipeline Artifacts Bucket).
If these buckets still contain data (deployed code files, artifacts, etc.), CloudFormation will not be able to delete the buckets, causing the stack deletion process to fail with a `DELETE_FAILED` error.
{{% /notice %}}

1. Open the [AWS CloudFormation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks).
2. Select your Stack (e.g., `ticket-app-stack`).
3. Click **Delete** on the toolbar at the top.
4. Confirm **Delete stack**.
![Delete CloudFormation](/images/5-Workshop/5.10-Cleanup/cfn_1.jpg)

5. The system will automatically release all resources from A to Z (VPC, Subnets, Beanstalk, RDS, Redis, S3, Cognito, API Gateway, CloudFront, CodePipeline...) safely and cleanly without requiring manual cleanup steps.
