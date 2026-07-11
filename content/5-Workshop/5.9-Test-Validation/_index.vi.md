---
title : "Kiểm thử & Xác thực"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

### Kiểm thử & Xác thực (Test & Validation)

Sau khi hệ thống đã được triển khai hoàn tất thông qua CI/CD, bước tiếp theo là xác thực khả năng hoạt động, độ chịu tải và cơ chế chịu lỗi của kiến trúc 3-tier. 

Trong bài lab này, chúng ta sẽ thực hiện end-to-end các bước kiểm thử theo đúng luồng người dùng và luồng hệ thống.

<div style="background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%); padding: 25px; border-radius: 12px; margin: 25px 0; color: white; box-shadow: 0 10px 20px rgba(0,0,0,0.15); border-left: 5px solid #00c6ff; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 280px; margin-right: 20px;">
    <h4 style="color: #ffffff !important; margin-top: 0; margin-bottom: 10px; font-weight: 800; font-size: 1.3rem; display: flex; align-items: center; gap: 8px; border: none; padding: 0; letter-spacing: 1px;">
      <span>🎥</span> VIDEO DEMO HỆ THỐNG
    </h4>
    <p style="margin: 0; font-size: 0.95rem; opacity: 0.9; line-height: 1.5; color: white;">
      Xem video demo giao diện thực tế cùng luồng hoạt động chi tiết của Người dùng (User) và Quản trị viên (Admin) trên hệ thống.
    </p>
  </div>
  <div style="margin-top: 15px; margin-bottom: 15px;">
    <a href="https://drive.google.com/drive/folders/1h8qYPxYnWJFA6rm8JlsLANiK2xKfqR0s?usp=sharing" target="_blank" style="display: inline-block; background-color: #00c6ff; color: #1e3c72; padding: 12px 24px; border-radius: 8px; font-weight: 700; text-decoration: none; box-shadow: 0 4px 6px rgba(0,0,0,0.1); transition: all 0.3s ease; text-transform: uppercase; font-size: 0.9rem;">
      Xem Video Demo ➜
    </a>
  </div>
</div>

---

#### 1. Kiểm tra Metric & Message Queue (SQS)
**Kịch bản:** Xác thực tin nhắn đã được Backend API đẩy thành công vào hàng đợi SQS.

*   **Bước 1:** Đăng nhập vào AWS Console, truy cập dịch vụ **SQS**.
*   **Bước 2:** Chọn hàng đợi `booking-queue.fifo`.
*   **Bước 3:** Chuyển sang tab **Monitoring**.
*   **Kết quả mong đợi:** Bạn sẽ thấy biểu đồ **Number Of Messages Sent** tăng lên tương ứng với số lượng request đặt vé. Nếu Worker đang chạy tốt, biểu đồ **Number Of Messages Deleted** cũng sẽ tăng ngay sau đó (do Worker đã tiêu thụ tin nhắn).
![SQS Metrics 2](/images/5-Workshop/5.9-Test-Validation/sqs_2.png)
![SQS Metrics 1](/images/5-Workshop/5.9-Test-Validation/sqs_1.png)

---

#### 2. Xem Log trên CloudWatch & Elastic Beanstalk
**Kịch bản:** Kiểm tra quá trình Worker xử lý đơn hàng và ghi vào cơ sở dữ liệu PostgreSQL.

*   **Bước 1:** Truy cập **CloudWatch** -> **Log groups**.
*   **Bước 2:** Tìm Log group của môi trường **Beanstalk Worker** (thường có định dạng `/aws/elasticbeanstalk/ticket-app-Worker-env/var/log/web.stdout.log`).
*   **Bước 3:** Tìm kiếm các từ khóa như `Processing message` hoặc `Message processed successfully`.
*   **Kết quả mong đợi:** Log hiển thị chi tiết tiến trình worker nhận tin nhắn từ SQS, xử lý giao dịch thanh toán, khởi tạo vé điện tử (e-ticket), cập nhật trạng thái vào RDS PostgreSQL và gửi email thông báo qua SES.
![CloudWatch Logs 1](/images/5-Workshop/5.9-Test-Validation/cw_log_1.png)
![CloudWatch Logs 2](/images/5-Workshop/5.9-Test-Validation/cw_log_2.png)

---

#### 3. Xác thực kết quả đầu ra (Email SES / SNS)
**Kịch bản:** Khách hàng phải nhận được email chứa vé điện tử (e-ticket).

*   **Bước 1:** Kiểm tra hộp thư (inbox) của email bạn dùng để đăng ký tài khoản Cognito.
*   **Kết quả mong đợi:** Một email xác nhận từ **Amazon SES** chứa thông tin vé điện tử và mã QR code được gửi đến thành công. Nếu có độ trễ, thời gian không quá 1-2 phút sau khi bấm đặt vé.

{{% notice note %}}
**Lưu ý:** Vì tài khoản Amazon SES của bạn đang ở chế độ **Sandbox**, bạn chỉ có thể gửi email đến các địa chỉ đã được xác minh. Đảm bảo rằng địa chỉ email nhận mail (email dùng để đăng ký tài khoản Cognito) cũng đã được Verify Identities trên giao diện SES nhé.
{{% /notice %}}

![SES Email 1](/images/5-Workshop/5.9-Test-Validation/ses_1.png)
![SES Email 2](/images/5-Workshop/5.9-Test-Validation/ses_2.png)
![SES Email 3](/images/5-Workshop/5.9-Test-Validation/ses_3.png)

---

#### 4. Kiểm thử lỗi (Chaos / Fault Tolerance Testing)
**Kịch bản:** Mô phỏng sự cố ngắt kết nối cơ sở dữ liệu hoặc lỗi code xử lý để xem hệ thống có mất đơn hàng hay không.

*   **Bước 1:** Cố tình thay đổi mật khẩu RDS trong Secrets Manager thành sai, HOẶC truy cập vào Beanstalk Worker chỉnh Capacity xuống 0 để tắt toàn bộ Worker.
*   **Bước 2:** Tiếp tục lên giao diện Frontend thực hiện đặt vé liên tục.
*   **Bước 3:** Kiểm tra SQS.
*   **Bước 4:** Kiểm tra CloudWatch Alarms. Sau một thời gian, bạn sẽ thấy Alarm của DLQ (ví dụ: `ticket-app-checkout-dlq-alarm`) chuyển sang trạng thái **In alarm** do có tin nhắn bị lỗi lọt vào DLQ. Đồng thời, một email cảnh báo từ **Amazon SNS** sẽ được gửi đến hộp thư của quản trị viên (Ops).
*   **Kết quả mong đợi:** 
    * API Backend vẫn phản hồi *"Đang xử lý"* nhanh chóng, không bị sập. 
    * Do Worker bị tắt hoặc lỗi, tin nhắn sẽ bị giữ lại trong `booking-queue.fifo`. 
    * Sau số lần retries thất bại, tin nhắn đơn hàng sẽ tự động bị đẩy sang **Dead Letter Queue (checkout-dlq.fifo)** và kích hoạt cảnh báo Alarm (như hình dưới).
    * Khi sự cố được khắc phục (bật lại Worker), quản trị viên có thể dùng tính năng *Redrive* từ DLQ đưa tin nhắn trở lại hàng đợi chính để xử lý tiếp. **Tuyệt đối không có đơn hàng nào bị mất mát.**

![DLQ Alarm](/images/5-Workshop/5.9-Test-Validation/dlq_1.png)
![DLQ Email Alert](/images/5-Workshop/5.9-Test-Validation/dlq_2.jpg)

Đến đây, bạn đã hoàn tất việc kiểm thử và chứng minh được sức mạnh của kiến trúc phân rã (Decoupled Architecture) trên AWS. Chuyển sang phần tiếp theo để tiến hành dọn dẹp tài nguyên.
