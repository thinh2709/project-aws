---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Tìm Hiểu Giải Pháp Multi-Build trên Amazon GameLift Servers

## GIỚI THIỆU

Trong quá trình phát triển game online, việc cập nhật và thử nghiệm nhiều phiên bản máy chủ trò chơi là một công việc diễn ra thường xuyên. Tuy nhiên, việc triển khai từng phiên bản mới lên hạ tầng có thể mất nhiều thời gian và ảnh hưởng đến quá trình kiểm thử.

AWS đã giới thiệu giải pháp Multi-Build trên Amazon GameLift Servers, cho phép phát triển lưu trữ và vận hành nhiều phiên bản máy chủ trò chơi trên cùng một fleet, giúp tăng tốc độ phát triển và thử nghiệm game.


## AMAZON GAMELIFT SERVERS LÀ GÌ?

Amazon GameLift Servers là dịch vụ lưu trữ máy chủ game được quản lý hoàn toàn bởi AWS. Dịch vụ này giúp các nhà phát triển triển khai, quản lý và mở rộng hệ thống máy chủ game trên phạm vi toàn cầu.

Nhiều tựa game lớn trên thị trường đã sử dụng GameLift để đảm bảo khả năng mở rộng và độ ổn định cho hàng triệu người chơi.


## VẤN ĐỀ TRONG QUÁ TRÌNH PHÁT TRIỂN GAME

Thông thường, mỗi khi có phiên bản máy chủ mới, nhà phát triển phải:
- Build lại server.
- Triển khai lại lên hạ tầng.
- Kiểm tra và xác nhận hoạt động.
- Chuyển đổi giữa các phiên bản khi cần thử nghiệm.

Quy trình này tốn khá nhiều thời gian, đặc biệt trong các giai đoạn Alpha và Beta khi việc cập nhật diễn ra liên tục.


## GIẢI PHÁP MULTI-BUILD

Multi-Build là giải pháp cho phép lưu trữ nhiều phiên bản máy chủ trò chơi trên cùng một fleet Amazon GameLift.

Thay vì phải triển khai lại toàn bộ hệ thống, nhà phát triển chỉ cần:
1. Upload phiên bản mới lên Amazon S3.
2. Hệ thống tự động đồng bộ xuống các máy chủ GameLift.
3. Khi tạo Game Session, chỉ định phiên bản BuildVersion cần sử dụng.
4. Máy chủ sẽ tự động khởi động đúng phiên bản tương ứng.

Nhờ đó việc chuyển đổi giữa các phiên bản diễn ra nhanh chóng và linh hoạt hơn.


## KIẾN TRÚC HOẠT ĐỘNG

Giải pháp sử dụng hai container chính:

### 1. GAME SERVER CONTAINER
Container này chịu nhiệm vụ:
- Chạy game server.
- Nhận yêu cầu tạo game session.
- Khởi động đúng phiên bản server theo BuildVersion.

### 2. S3 SYNC CONTAINER
Container này hoạt động nền và liên tục:
- Kiểm tra thay đổi trên Amazon S3.
- Đồng bộ các bản build mới.
- Xóa các bản build cũ không còn sử dụng.
- Đảm bảo dữ liệu được tải xuống đầy đủ trước khi thực thi.

Hai container cùng sử dụng một thư mục chia sẻ để truy cập các bản build đã được đồng bộ.

<img src="/images/Blog2/716962056_1849193866486013_5935965608591198664_n.jpg" alt="Kiến trúc Multi-Build GameLift 1" class="blog-image" />
<img src="/images/Blog2/719483076_1849193823152684_8697794779991986075_n.jpg" alt="Kiến trúc Multi-Build GameLift 2" class="blog-image" />


## CÁC DỊCH VỤ AWS ĐƯỢC SỬ DỤNG

Giải pháp Multi-Build kết hợp nhiều dịch vụ AWS:
- Amazon GameLift Servers
- Amazon S3
- Amazon ECR
- AWS IAM
- AWS CodeBuild
- AWS CloudFormation
- Amazon CloudWatch Logs

Nhờ đó toàn bộ quy trình triển khai và quản lý được tự động hóa gần như hoàn toàn.


## LỢI ÍCH CỦA MULTI-BUILD

### TĂNG TỐC ĐỘ PHÁT TRIỂN
Các nhóm phát triển có thể kiểm thử nhiều phiên bản game cùng lúc mà không cần triển khai lại fleet.

### TIẾT KIỆM THỜI GIAN
Việc upload bản build mới chỉ mất vài phút thay vì thực hiện quy trình triển khai đầy đủ.

### HỖ TRỢ ALPHA VÀ BETA TESTING
Nhiều phiên bản game có thể tồn tại song song để phục vụ các nhóm người chơi thử nghiệm khác nhau.

### DỄ DÀNG QUẢN LÝ
Các bản build được quản lý tập trung trên Amazon S3 và tự động đồng bộ tới các máy chủ.


## KẾT LUẬN

Amazon GameLift Servers Multi-Build là một giải pháp hữu ích dành cho các nhóm phát triển game muốn tăng tốc độ thử nghiệm và phát hành phiên bản mới. Bằng cách cho phép nhiều bản build cùng tồn tại trên một fleet duy nhất, AWS giúp giảm đáng kể thời gian triển khai và tối ưu quy trình phát triển game.

Đối với các dự án game đang trong giai đoạn Alpha, Beta hoặc phát triển tính năng mới, đây là một giải pháp đáng để tìm hiểu và áp dụng.

Link tài liệu: https://aws.amazon.com/vi/blogs/gametech/rapid-game-server-iteration-on-amazon-gamelift-servers/