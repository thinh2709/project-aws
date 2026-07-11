---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Các bước chuẩn bị (Prerequisites)

Để hoàn thành bài thực hành workshop xây dựng ứng dụng đặt vé sự kiện **Ticket-App**, bạn cần chuẩn bị môi trường phát triển cục bộ và phân quyền tài khoản AWS tương ứng.

---

#### 1. Quyền hạn IAM (IAM Permissions)

Bạn cần sử dụng một tài khoản IAM User hoặc IAM Role có các quyền hạn để dọn dẹp và vận hành các tài nguyên trong workshop. 

{{% notice note %}}
Để thực hành lab cá nhân một cách thuận tiện nhất và tránh lỗi phân quyền (Access Denied), chúng tôi khuyên bạn nên sử dụng quyền **AdministratorAccess** cho tài khoản thực hành của mình.
{{% /notice %}}

Nếu bạn muốn cấu hình chính sách phân quyền tối thiểu (Least Privilege), hãy làm theo các bước sau để tạo IAM Policy trên AWS Console:

1. Đăng nhập vào **AWS Management Console** và tìm kiếm dịch vụ **IAM**.
2. Chọn **Policies** ở thanh điều hướng bên trái và nhấn **Create policy**.
3. Tại giao diện tạo policy, chuyển sang trình chỉnh sửa **JSON** (JSON editor), xóa hết mã mẫu cũ và dán đoạn chính sách JSON dưới đây vào:

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

5. Nhấn **Next**.
6. Nhập tên cho chính sách (ví dụ: `TicketAppCleanupPolicy`), thêm mô tả tùy chọn, và nhấn **Create policy**.
7. Tiến hành gán chính sách này vào **IAM User** của bạn (truy cập menu **Users** -> chọn User của bạn -> **Add permissions** -> **Attach policies directly** và chọn chính sách `TicketAppCleanupPolicy` vừa tạo).



---

#### 2. Cài đặt môi trường phát triển cục bộ (Local Workspace Setup)

Hãy đảm bảo máy trạm của bạn đã được cài đặt sẵn các công cụ sau:

##### A. Node.js & npm
*   Ứng dụng Ticket-App được viết bằng Node.js. Bạn cần cài đặt Node.js phiên bản **18.x** hoặc **20.x** (LTS).
*   Kiểm tra cài đặt:
    ```bash
    node -v
    npm -v
    ```

##### B. Git
*   Sử dụng Git để tải mã nguồn dự án.
*   Kiểm tra cài đặt:
    ```bash
    git --version
    ```

---

#### 3. Tải mã nguồn dự án (Project Source Code)

Mã nguồn của ứng dụng Ticket-App được chia làm 3 thư mục chính:
1.  **ticket-booking-frontend/**: Giao diện React tĩnh hiển thị danh sách sự kiện và đặt vé.
2.  **ticket-booking-backend/** (Backend API): Mã nguồn Node.js API (Express) xử lý tiếp nhận lượt booking, xác thực Cognito và đẩy tin nhắn vào SQS.
3.  **ticket-booking-worker/** (Worker): Mã nguồn chạy tác vụ ngầm tiêu thụ SQS để ghi nhận thông tin đặt vé vào PostgreSQL DB.

Hãy clone dự án từ repository workshop của bạn:
```bash
git clone https://github.com/thinh2709/Project-FCAJ.git
cd Project-FCAJ
```



---

#### 4. Lựa chọn Phương thức Triển khai Hạ tầng (VPC, RDS, Beanstalk, SQS,...)

Trong workshop này, bạn có thể lựa chọn 1 trong 2 phương thức sau để xây dựng cơ sở hạ tầng mạng và máy chủ:

*   **Lựa chọn A (Khuyên dùng - Nhanh chóng): Triển khai tự động bằng AWS CloudFormation**
    *   Toàn bộ hạ tầng của hệ thống sẽ được khởi tạo hoàn toàn tự động từ A đến Z.
    *   Tất cả các chương từ **5.3 đến 5.8** sẽ đóng vai trò là hướng dẫn để bạn **Kiểm tra và Xác thực** cấu hình tài nguyên đã được tạo tự động thay vì phải tự cấu hình thủ công.
*   **Lựa chọn B: Tự tay cấu hình thủ công từng bước (Manual)**
    *   Bạn sẽ **bỏ qua** bước 4 (chạy CloudFormation) dưới đây.
    *   Bắt đầu từ chương **5.3**, bạn sẽ tự tay làm theo từng bước hướng dẫn trên AWS Console để tự xây dựng hệ thống từ con số 0.

---

#### Hướng dẫn cho Lựa chọn A: Triển khai tự động bằng CloudFormation

Để chuẩn bị cho môi trường làm workshop, chúng ta tiến hành deploy CloudFormation template:

<div style="background-color: #f8f9fa; border: 1px solid #e9ecef; border-left: 5px solid #ff9900; padding: 20px; border-radius: 8px; margin: 20px 0; display: flex; flex-direction: column; gap: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05);">
  <div style="font-weight: 700; color: #232f3e; font-size: 1.1rem; line-height: 24px;">
    <img src="https://img.icons8.com/color/48/amazon-web-services.png" style="width: 24px !important; height: 24px !important; margin: 0 8px 0 0 !important; display: inline-block !important; vertical-align: middle !important;" alt="AWS"/>
    <span style="vertical-align: middle !important;">Khởi tạo Nhanh với AWS CloudFormation</span>
    <span style="font-size: 0.8rem; background-color: #e6f4ea; color: #137333; padding: 4px 8px; border-radius: 4px; font-weight: 600; display: inline-flex; align-items: center; gap: 4px; vertical-align: middle !important; margin-left: 10px; border: none;">
      <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
      Đã kiểm thử & xác thực chạy thực tế
    </span>
  </div>
  <p style="margin: 0; font-size: 0.95rem; color: #545b64; line-height: 1.5;">
    Click vào nút bên dưới để mở giao diện <strong>Quick create stack</strong> trên AWS Console (us-east-1). Hãy giữ nguyên các lựa chọn mặc định.
  </p>
  <div>
    <a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=ticket-app-stack&templateURL=https://fcaj-ticketing-templates.s3.amazonaws.com/template-cloudformation.yaml" target="_blank" style="display: inline-flex; align-items: center; gap: 8px; background-color: #ff9900; color: white !important; padding: 12px 24px; border-radius: 4px; font-weight: 700; text-decoration: none; transition: background-color 0.2s ease; font-size: 0.95rem; box-shadow: 0 2px 4px rgba(0,0,0,0.15);">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" style="margin-right: 4px;"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path><polyline points="15 3 21 3 21 9"></polyline><line x1="10" y1="14" x2="21" y2="3"></line></svg>
      Deploy TicketAppStack
    </a>
  </div>
</div>

{{% notice info %}}
**Lưu ý về phạm vi triển khai**: 
Nếu chọn triển khai tự động bằng CloudFormation, toàn bộ hệ thống từ cơ sở hạ tầng mạng, bảo mật, cơ sở dữ liệu đến CI/CD sẽ được tự động khởi tạo từ A đến Z. Bạn có thể **bỏ qua tất cả các bước tạo thủ công** trong toàn bộ workshop này và chỉ cần đọc hướng dẫn từ **Chương 5.3 đến Chương 5.8** để **Kiểm tra/Xác minh** cấu hình của các tài nguyên đã được tạo sẵn.
{{% /notice %}}

1. Trình duyệt sẽ mở trang **Quick create stack** của bảng điều khiển CloudFormation Console với Template URL đã được điền sẵn.
2. Kiểm tra tên Stack là `ticket-app-stack` ở mục **Stack name**.
3. Cuộn xuống phần **Parameters** để cấu hình các tham số (bắt buộc thay đổi các trường email theo thông tin của bạn):
   * **EnvironmentName**: Để mặc định là `ticketing`.
   * **DBMasterUsername** & **DBName**: Để mặc định là `postgres` & `ticketing_db`.
   * **OpsNotificationEmail**: Thay đổi thành email cá nhân của bạn (dùng để nhận thông báo vận hành/cảnh báo hệ thống).
   * **UserNotificationEmail**: Thay đổi thành email cá nhân của bạn (dùng để kiểm thử nhận email thông báo đặt vé thành công).
   * **MailFrom**: Thay đổi thành email cá nhân của bạn (địa chỉ email gửi thông báo, sẽ được cấu hình và xác thực với Amazon SES ở bước sau).
   * Các thông số còn lại (như `SmtpUsername`, `SmtpPassword`, `MailFromName`, `ReservationTimeoutMinutes`...): Có thể giữ nguyên giá trị mặc định.
4. Sau khi cấu hình xong, tích chọn hộp thoại **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** ở phần Capabilities dưới cùng.
5. Click chọn nút **Create stack** để bắt đầu triển khai.
6. Đợi cho đến khi quá trình khởi tạo hoàn tất (Trạng thái của Stack chuyển sang `CREATE_COMPLETE`).

Sau khi CloudFormation Stack triển khai thành công, toàn bộ hạ tầng hệ thống của bạn đã được khởi tạo hoàn tất từ A đến Z! Ở các chương tiếp theo (từ **5.3** đến **5.8**), nếu đi theo **Lựa chọn A**, bạn chỉ cần đọc hướng dẫn để **kiểm tra và xác minh cấu hình** tài nguyên của mình đã được tạo tự động mà không cần thực hiện thêm bất kỳ thao tác cấu hình thủ công nào. Bạn có thể tiến thẳng đến **Chương 5.9 (Test)** để bắt đầu kiểm thử và xác thực hệ thống hoạt động.
