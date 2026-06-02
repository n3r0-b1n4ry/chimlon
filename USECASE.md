# USECASE.md - Đặc tả Kịch bản Sử dụng (Use Cases)

Tài liệu này định nghĩa các kịch bản nghiệp vụ chính mà hệ thống AI Agent **"Chim Lợn"** thực hiện trên môi trường thực tế.

---

## UC-01: Tự động Tương tác và Bình luận Bảng tin (Smart Feed Engagement)

### 1. Tóm tắt
Agent tự động điều khiển trình duyệt lướt qua bảng tin (Newsfeed), phân loại nội dung bài viết, đưa ra cảm xúc thích hợp và sinh câu bình luận thông minh mang tính xây dựng để tăng tương tác tài khoản.

### 2. Các Tác nhân (Actors)
- AI Agent (Bộ não Host)
- Playwright Browser (Thực thi)

### 3. Điều kiện tiên quyết (Preconditions)
- Tài khoản mạng xã hội (ví dụ: X/Twitter) đã đăng nhập thành công và lưu Session/Cookies vào thư mục hồ sơ của Playwright.
- Cấu hình chủ đề quan tâm (Keywords/Topics Filter) đã được người dùng thiết lập trên giao diện App.

### 4. Luồng Nghi vụ Chính (Main Flow)
1. Người dùng bấm nút "Bắt đầu Tương tác Bảng tin" trên Menu Bar App.
2. App Host gửi lệnh khởi động trình duyệt dạng Streamable HTTP tới MCP Server.
3. Playwright mở trang chủ mạng xã hội và cuộn trang để tải bài viết mới.
4. Playwright trích xuất dữ liệu bài viết (Văn bản + Ảnh đính kèm) gửi về App Host qua luồng HTTP Stream.
5. Bộ não AI (Gemma 4 E2B) phân tích xem nội dung có trùng khớp với chủ đề quan tâm không.
6. Nếu phù hợp, AI thực hiện suy luận nghĩ (`<|think|>`) ra cảm xúc (Like, Love) và viết câu bình luận.
7. Host gửi cấu hình hành động dưới dạng JSON Struct về cho MCP Server.
8. Playwright thực hiện click nút Reaction và gõ chuỗi bình luận bằng cơ chế mô phỏng người thật.
9. Kết quả thành công được cập nhật lên giao diện Dashboard.

### 5. Luồng Thay thế (Alternative Flows)
- **Alt-Flow A: Bài viết không đúng chủ đề quan tâm:** Tại bước 5, nếu AI đánh giá nội dung thuộc chủ đề rác hoặc không quan tâm, Host ra lệnh cho Playwright bỏ qua (Skip) bài viết và cuộn tiếp xuống dưới.
- **Alt-Flow B: Phát hiện bài viết quảng cáo (Sponsored Post):** AI tự động bỏ qua để tránh tương tác với bot quảng cáo.

---

## UC-02: Giám sát và Phản hồi Thông báo Chạy ngầm (Background Reply)

### 1. Tóm tắt
Agent chạy ẩn dưới thanh Menu Bar theo chu kỳ, kiểm tra các thông báo mới (lượt nhắc tên, câu trả lời cũ) để thực hiện tương tác phản hồi phản pháo, duy trì độ hot của tài khoản.

### 2. Các Tác nhân
- AI Agent (Chạy chế độ tiết kiệm năng lượng CoreML/ANE)
- Playwright Browser (Chạy chế độ ẩn danh - Headless)

### 3. Điều kiện tiên quyết
- Tính năng chạy nền được kích hoạt, chu kỳ kiểm tra được đặt (ví dụ: mỗi 20 phút).

### 4. Luồng Nghiệp vụ Chính
1. Đồng hồ hệ thống kích hoạt bộ định thì (Timer) của App.
2. Host gửi yêu cầu ngầm tới MCP Server mở tab thông báo (`/notifications`).
3. Playwright cào danh sách các thông báo mới nhất chưa đọc.
4. Nếu có người bình luận vào bài viết của mình, Playwright bóc tách toàn bộ chuỗi hội thoại (Thread context) gửi về Host.
5. AI phân tích sắc thái (Tích cực, Tiêu cực, Hỏi đáp) để đưa ra câu trả lời phù hợp nhất với tính cách cấu hình của chủ tài khoản.
6. Host gửi lệnh phản hồi. Playwright truy cập trực tiếp URL của thread đó, click "Reply" và gửi văn bản.
7. Trình duyệt đóng hoàn toàn để tiết kiệm RAM. Máy trở về trạng thái nghỉ.

---

## UC-03: Vượt rào cản Bảo mật và Nhận diện Giao diện lỗi (Anti-Bot & UI Bypass)

### 1. Tóm tắt
Xử lý các tình huống bất thường khi mạng xã hội thay đổi mã HTML cốt lõi cấu trúc trang hoặc xuất hiện tường lửa, checkpoint bảo mật (Captcha).

### 2. Các Tác nhân
- Playwright Server
- Multimodal AI Agent (Thị giác máy tính)
- Người dùng (Hỗ trợ khi AI không xử lý được)

### 3. Luồng Nghiệp vụ Chính
1. Playwright nhận lệnh click nút "Bình luận" nhưng không thể tìm thấy phần tử HTML dựa trên các Selector (CSS/XPath) cũ do giao diện Web vừa cập nhật.
2. Playwright không báo lỗi sập hệ thống. Nó tự động chụp lại toàn bộ màn hình trình duyệt (Screenshot) dưới dạng ảnh Base64.
3. Bản ghi ảnh được gửi qua HTTP Stream về cho Host.
4. Bộ não AI kích hoạt mô hình Thị giác (Vision Mode) của Gemma 4. AI quét bức ảnh và xác định tọa độ Pixel vật lý (X, Y) của nút "Bình luận" trên màn hình.
5. Host gửi lệnh di chuyển chuột và click chính xác vào tọa độ hình học (X, Y) vừa tìm được.
6. Trình duyệt vượt qua điểm nghẽn thành công và tiếp tục luồng công việc bình thường.

### 4. Luồng Thay thế (Gặp Captcha phức tạp)
- Nếu ảnh chụp màn hình hiển thị bảng Captcha hình ảnh đố vui (Ví dụ: Tìm xe bus, đèn giao thông), AI sẽ thử giải quyết bằng tọa độ. Nếu độ tin cậy < 70%, App Host sẽ phát âm thanh cảnh báo và đẩy thông báo (Push Notification) lên macOS yêu cầu người dùng nhấp chuột giải thủ công trong vòng 60 giây trước khi Agent tiếp quản lại công việc.