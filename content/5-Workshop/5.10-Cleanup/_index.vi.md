---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.10. </b> "
---

### Dọn dẹp tài nguyên (Resource Cleanup)

{{% notice warning %}}
Để tránh phát sinh các chi phí ngoài ý muốn trên tài khoản AWS cá nhân của bạn, hãy thực hiện dọn dẹp toàn bộ tài nguyên ngay sau khi hoàn thành bài lab thực hành.
{{% /notice %}}

Vui lòng chọn phương án dọn dẹp phù hợp với phương thức triển khai bạn đã sử dụng ở chương **5.2**:

*   **Trường hợp A: Nếu bạn triển khai bằng CloudFormation (Lựa chọn A ở chương 5.2)**
    *   Bạn **KHÔNG ĐƯỢC** xóa thủ công từng tài nguyên riêng lẻ trước. Việc xóa thủ công sẽ phá vỡ trạng thái liên kết của CloudFormation Stack, dẫn đến lỗi Stack bị treo hoặc báo lỗi `DELETE_FAILED` khi xóa.
    *   Hãy cuộn xuống và thực hiện trực tiếp **Mục 2. Hướng dẫn Dọn dẹp bằng CloudFormation**.
*   **Trường hợp B: Nếu bạn tự tạo tài nguyên thủ công (Lựa chọn B ở chương 5.2)**
    *   Hãy làm theo hướng dẫn tại **Mục 1. Dọn dẹp thủ công từng tài nguyên**.

---

#### 1. Dọn dẹp thủ công từng tài nguyên (Dành cho Lựa chọn B)

Thực hiện xóa các tài nguyên theo trình tự từ trên xuống dưới:

1. **Xóa môi trường Elastic Beanstalk**:
   * Mở [Elastic Beanstalk console](https://us-east-1.console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/environments).
   * Chọn môi trường ```ticket-app-Backend-env``` -> click **Actions** -> chọn **Terminate environment** và xác nhận.
   ![Terminate Backend Env](/images/5-Workshop/5.10-Cleanup/eb_1.jpg)
   ![Confirm Backend Env](/images/5-Workshop/5.10-Cleanup/eb_2.jpg)
   * Thực hiện tương tự để Terminate môi trường ```ticket-app-Worker-env``` và xác nhận.
   ![Terminate Worker Env](/images/5-Workshop/5.10-Cleanup/eb_3.jpg)
   ![Confirm Worker Env](/images/5-Workshop/5.10-Cleanup/eb_4.jpg)
   * Chờ cho đến khi quá trình hủy môi trường hoàn tất, sau đó xóa Application: Quay lại trang **Applications** -> chọn ```ticket-app-App``` -> click **Actions** -> **Delete application**.

2. **Xóa API Gateway**:
   * Mở [API Gateway console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
   * Chọn HTTP API của bạn (ví dụ: `TicketAppAPI`) -> click **Delete** -> Xác nhận xóa.
   ![Delete API Gateway](/images/5-Workshop/5.10-Cleanup/api_1.jpg)
   ![Confirm Delete API Gateway](/images/5-Workshop/5.10-Cleanup/api_2.jpg)

3. **Xóa CloudFront Distribution**:
   * Mở [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
   * Chọn Distribution của bạn -> click **Disable**. Quá trình này sẽ mất vài phút.
   ![Disable CloudFront](/images/5-Workshop/5.10-Cleanup/cf_2.jpg)
   * Sau khi trạng thái chuyển sang Disabled, chọn lại Distribution -> click **Delete**.
   ![Delete CloudFront](/images/5-Workshop/5.10-Cleanup/cf_1.jpg)
   {{% notice note %}}
   **Lưu ý:** Nếu tài khoản của bạn đang đăng ký gói cước theo tháng của CloudFront, bạn sẽ không thể xóa hoàn toàn Distribution này ngay lập tức.
   {{% /notice %}}

4. **Xóa RDS Proxy & RDS PostgreSQL**:
   * Mở [Amazon RDS console](https://us-east-1.console.aws.amazon.com/rds/home?region=us-east-1#databases:).
   * Chọn **Proxies** -> Chọn ```rds-proxy-ticket-app``` -> click **Actions** -> **Delete**.
   ![Delete RDS Proxy](/images/5-Workshop/5.10-Cleanup/rds_1.jpg)
   * Chọn **Databases** -> Chọn Database instance ```database-ticket-app``` -> click **Actions** -> **Delete**:
     * Chọn **No** ở mục tạo final snapshot (để xóa nhanh không cần sao lưu).
     * Tích chọn xác nhận và nhập ```delete me``` để đồng ý xóa.
   ![Delete RDS Database](/images/5-Workshop/5.10-Cleanup/rds_2.jpg)

5. **Xóa ElastiCache Redis replication group**:
   * Mở [ElastiCache console](https://us-east-1.console.aws.amazon.com/elasticache/home?region=us-east-1#/clusters).
   * Chọn cụm Redis ```ticket-app-redis``` -> click **Actions** -> **Delete**.
   ![Delete Redis](/images/5-Workshop/5.10-Cleanup/redis_1.jpg)

6. **Xóa Cognito User Pool**:
   * Mở [Amazon Cognito console](https://us-east-1.console.aws.amazon.com/cognito/v2/home?region=us-east-1#).
   * Chọn User Pool ```ticket-app-user-pool``` -> click **Delete user pool**.
   ![Delete Cognito](/images/5-Workshop/5.10-Cleanup/cognito_1.jpg)

7. **Xóa AWS CodePipeline, CodeBuild & CodeCommit**:
   * Mở **CodePipeline console**:
     * Chọn Pipeline `ticket-app-backend-pipeline` -> click **Delete**.
     * Thực hiện tương tự cho `ticket-app-worker-pipeline`.
     ![Delete CodePipeline](/images/5-Workshop/5.10-Cleanup/ci_2.jpg)
   * Mở **CodeBuild console**:
     * Chọn Build project `ticket-app-backend-build` -> click **Delete build project**.
     * Thực hiện tương tự cho `ticket-app-worker-build`.
     ![Delete CodeBuild](/images/5-Workshop/5.10-Cleanup/ci_1.jpg)
   * Mở **CodeCommit console**:
     * Chọn Repository `ticket-app-backend` -> click **Delete repository**.
     * Thực hiện tương tự cho `ticket-app-worker`.
     ![Delete CodeCommit](/images/5-Workshop/5.10-Cleanup/ci_3.jpg)

8. **Xóa Amazon SQS, SNS & CloudWatch Alarm**:
   * Mở **CloudWatch console** -> **Alarms**: Chọn Alarm `ticket-app-checkout-dlq-alarm` -> click **Delete**.
   ![Delete Alarm](/images/5-Workshop/5.10-Cleanup/misc_1.jpg)
   * Mở **SQS console**:
     * Chọn `booking-queue.fifo` -> click **Delete**.
     * Chọn `checkout-dlq.fifo` -> click **Delete**.
   ![Delete SQS](/images/5-Workshop/5.10-Cleanup/misc_3.jpg)
   * Mở **SNS console**:
     * Chọn Topic `ticket-app-notifications` -> click **Delete**.
   ![Delete SNS](/images/5-Workshop/5.10-Cleanup/misc_2.jpg)

9. **Xóa SES Identity & SMTP Credentials**:
   * Mở **Amazon SES console**: Chọn **Identities** -> Chọn Email của bạn -> click **Delete**.
   ![Delete SES](/images/5-Workshop/5.10-Cleanup/ses_1.jpg)
   * Mở **IAM console**: Chọn **Users** -> Chọn User `ticket-app-smtp-user` -> click **Delete**.
   ![Delete IAM](/images/5-Workshop/5.10-Cleanup/iam_1.png)

10. **Xóa Secrets Manager**:
    * Mở [Secrets Manager console](https://us-east-1.console.aws.amazon.com/secretsmanager/home?region=us-east-1#!/listSecrets).
    * Chọn secret của bạn (chứa mật khẩu DB) -> click **Actions** -> **Delete secret**. (Nhập số ngày chờ là 7 ngày).
    ![Delete Secret](/images/5-Workshop/5.10-Cleanup/sm_1.jpg)

11. **Xóa S3 Buckets**:
    * Mở [Amazon S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#).
    * Bạn phải **Empty** (xóa sạch file) trong bucket trước khi có thể xóa bucket:
      * Chọn Frontend Bucket ```frontend-ticket-app-app-<your-account-id>``` -> click **Empty** -> nhập `permanently delete` để xác nhận.
      * Thực hiện tương tự với Assets Bucket ```ticket-app-assets-<your-account-id>```.
      ![Empty S3](/images/5-Workshop/5.10-Cleanup/s3_1.jpg)
      * Sau khi empty xong, chọn các Bucket -> click **Delete** -> nhập tên bucket để xác nhận xóa hẳn.
      ![Delete S3](/images/5-Workshop/5.10-Cleanup/s3_2.jpg)

12. **Xóa Hạ tầng Mạng (VPC, NAT, IGW, SGs)**:
    * Mở [VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1).
    * **NAT Gateways**: Xóa 2 NAT Gateways. (Quá trình xóa sẽ mất vài phút).
    ![Delete NAT](/images/5-Workshop/5.10-Cleanup/vpc_1.jpg)
    * **Elastic IPs**: Release 2 EIPs đã liên kết với NAT.
    ![Release EIP](/images/5-Workshop/5.10-Cleanup/vpc_2.jpg)
    * **VPC**: Chọn VPC `ticket-app-vpc` -> click **Actions** -> **Delete VPC**. Tính năng này sẽ tự động xóa các Subnets, Route Tables, Internet Gateway và Security Groups đi kèm (nếu không còn tài nguyên nào phụ thuộc). Hãy đảm bảo tất cả các tài nguyên ở trên (EC2, RDS, ElastiCache, Beanstalk, ELB) đã được xóa hoàn toàn trước khi thực hiện bước này.
    ![Delete VPC](/images/5-Workshop/5.10-Cleanup/vpc_3.jpg)

---

#### 2. Hướng dẫn Dọn dẹp bằng CloudFormation (Dành cho Lựa chọn A)

Nếu bạn deploy toàn bộ hạ tầng thông qua file template CloudFormation ở chương 5.2:

{{% notice warning %}}
**Lưu ý quan trọng trước khi xóa Stack:**
Trước khi bấm xóa Stack CloudFormation, bạn **bắt buộc** phải truy cập vào [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#) và thực hiện **Empty (Làm trống)** tất cả các S3 Buckets được tạo bởi hệ thống (bao gồm Frontend Bucket, Assets Bucket và CodePipeline Artifacts Bucket). 
Nếu các bucket này còn chứa dữ liệu (tệp tin code đã deploy, artifacts,...), CloudFormation sẽ không thể xóa bucket và quá trình xóa stack sẽ bị báo lỗi `DELETE_FAILED`.
{{% /notice %}}

1. Mở [AWS CloudFormation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks).
2. Chọn Stack của bạn (ví dụ: `ticket-app-stack`).
3. Click **Delete** ở thanh công cụ phía trên.
4. Xác nhận **Delete stack**.
![Delete CloudFormation](/images/5-Workshop/5.10-Cleanup/cfn_1.jpg)

5. Hệ thống sẽ tự động giải phóng toàn bộ tài nguyên từ A đến Z (VPC, Subnets, Beanstalk, RDS, Redis, S3, Cognito, API Gateway, CloudFront, CodePipeline...) một cách an toàn và sạch sẽ mà không cần phải thực hiện dọn dẹp thủ công từng bước.
