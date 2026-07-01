---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# [AWS Tech Share] Chuyển Đổi Người Xem Thành Người Chơi: Xây Dựng Trải Nghiệm Game Tương Tác Cùng GameLift Streams

Hôm nay mình xin chia sẻ về cách AWS giải quyết bài toán "Interactive Streaming" – biến trải nghiệm xem stream game thành giao tiếp tương tác thời gian thực hai chiều.

Trong phát triển game, việc tổ chức playtest thường tốn nhiều thời gian (tải build, setup, review video). Đồng thời, việc tương tác với cộng đồng người xem cũng gặp rào cản khi người xem chỉ có thể gõ chat động. Để giải quyết bài toán này, AWS đã đưa ra một kiến trúc trực kết hợp vô cùng mạnh mẽ.


## 3 "TRỤ CỘT" CỦA KIẾN TRÚC GIẢI PHÁP:

- **Amazon GameLift Streams**: Hỗ trợ stream trực tiếp game từ server xuống trình duyệt (WebRTC) với độ trễ cực thấp, đạt chất lượng lên đến 1080p ở 60 FPS mà không cần người chơi phải tải hay cài đặt game.
- **Amazon IVS (Interactive Video Service)**: Tiếp nhận luồng phát gameplay và phân phối toàn cầu với độ trễ dưới 1 giây (subsecond latency).
- **AWS AppSync**: Đóng vai trò cầu nối, cung cấp WebSocket APIs để truyền tải tín nhắn, reaction và lệnh điều khiển từ khán giả ngược lại vào game ngay lập tức.


## LUỒNG XỬ LÝ (ARCHITECTURE FLOW) HOẠT ĐỘNG NHƯ THẾ NÀO?

1. Người dùng thao tác trên giao diện React Frontend, được xác thực bảo mật thông qua Amazon Cognito.
2. Amazon API Gateway và AWS Lambda sẽ chịu trách nhiệm giao tiếp và khởi tạo phiên stream (Stream session).
3. Ở phía server, GameLift Streams chạy game, đồng thời khởi chạy một tiến trình ngầm gọi là "Broadcast Sidecar".
4. Ứng dụng Sidecar này sẽ thực hiện bắt hình ảnh/âm thanh, mã hóa video chuẩn H.264 và đẩy luồng stream thẳng lên stage của Amazon IVS với độ trễ dưới 300ms.

<img src="/images/Blog3/717792483_1542699650893252_4468378101643065781_n.jpg" alt="Kiến trúc GameLift Streams 1" class="blog-image" />
<img src="/images/Blog3/718204942_1542699634226587_8876440514239223290_n.jpg" alt="Kiến trúc GameLift Streams 2" class="blog-image" />
<img src="/images/Blog3/718597666_1542699647559919_5008900173891667306_n.jpg" alt="Kiến trúc GameLift Streams 3" class="blog-image" />


## ĐIỂM NHẤN KỸ THUẬT: CƠ CHẾ "CONTROL HANDOFF"

Điểm ấn tượng nhất trong giải pháp này là cách xử lý việc "nhường quyền điều khiển" (Control Handoff) cho những người tham gia. Hệ thống dùng AWS AppSync Event API để điều phối các message kiểm soát trạng thái rõ ràng, ví dụ như:
- **TAKEOVER_REQUEST**: Yêu cầu nắm quyền điều khiển từ người chơi hiện tại.
- **TAKEOVER_APPROVED**: Chấp thuận và bắt đầu chia sẻ session thông qua API CreateStreamConnection.


## TỔNG KẾT

Kiến trúc này không chỉ tối ưu hóa quy trình Playtesting nội bộ của các studio mà còn mở ra tiềm năng không lờ cho các chiến dịch truyền thông: nơi người xem stream có thể trực tiếp tham gia vào game (ví dụ: vote hướng đi, spawn quái vật) ngay trên trình duyệt mà không gặp trở ngại nào.


Link bài viết gốc: https://aws.amazon.com/blogs/gametech/creating-interactive-gaming-experiences-with-amazon-gamelift-streams-and-amazon-interactive-video-service/