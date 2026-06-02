# AI Agent "Chim Lợn" (Apple M4/M5 Optimized)

Hệ thống Trợ lý AI "Chim Lợn" tự động hóa tương tác mạng xã hội (Facebook, X, LinkedIn) chạy hoàn toàn cục bộ (Local) trên kiến trúc chip Apple Silicon M4 & M5. Dự án áp dụng thiết kế hệ thống hiện đại, tách biệt "Bộ não AI" và "Cánh tay thực thi Trình duyệt" thông qua giao thức **Model Context Protocol (MCP)** và truyền tải bằng **Streamable HTTP**.

## 🚀 Tính năng cốt lõi
- **Suy luận Đa phương thức Cục bộ:** Sử dụng mô hình tư duy thế hệ mới Gemma 4 E2B tối ưu trên phần cứng M4/M5 qua MLX và CoreML.
- **Kiến trúc Trình duyệt Phi tập trung:** Kết nối Playwright Automation Engine thông qua cổng MCP biệt lập, tăng tính ổn định và cô lập lỗi.
- **Giao tiếp Streamable HTTP:** Đồng bộ trạng thái thời gian thực giữa tiến trình nền và UI thông qua cơ chế luồng dữ liệu đơn endpoint.
- **Cơ chế Giả lập Người thật (Antidetect):** Tự động hóa hành vi di chuột, cuộn trang ngẫu nhiên và tốc độ gõ phím biến thiên để vượt qua thuật toán quét Bot.
- **Thị giác Máy tính (VLM Perception):** Chụp ảnh màn hình trình duyệt để nhận diện phần tử UI, loại bỏ điểm nghẽn "gãy mã nguồn" khi mạng xã hội thay đổi cấu trúc HTML DOM.

## 📁 Cấu trúc Bộ tài liệu Hệ thống
Để hiểu sâu và vận hành hệ thống, vui lòng tham khảo các tài liệu chuyên sâu sau:
1. 📘 **[ARCHITECTURE.md](ARCHITECTURE.md):** Chi tiết cấu trúc kỹ thuật hệ thống, giao thức MCP Streamable HTTP và chiến lược tối ưu phần cứng Apple M4/M5.
2. 📗 **[USECASE.md](USECASE.md):** Đặc tả các kịch bản sử dụng (Use Cases), điều kiện tiên quyết, luồng nghiệp vụ chính và luồng thay thế.
3. 📙 **[APPFLOW.md](APPFLOW.md):** Bản đồ luồng đi của dữ liệu từ tầng Nhận thức (Perception) -> Suy luận (Reasoning) -> Thực thi (Action Execution).

## 🛠 Yêu cầu Hệ thống & Khởi chạy nhanh

### Yêu cầu Phần cứng & Phần mềm
- **Hardware:** Apple M4 & M5 Series (Base, Pro, Max) với cấu hình Bộ nhớ Thống nhất (Unified Memory).
- **OS:** macOS 14.0 (Sonoma) hoặc mới hơn.
- **Runtime:** Python 3.11+ (cho MLX/Playwright Server) và Node.js 20+ (nếu dùng MCP Node Server).

### Khởi chạy Nhanh (Quick Start)
1. **Khởi động MCP Playwright Server:**
```bash
   cd server/playwright-mcp
   pip install -r requirements.txt
   playwright install chromium
   python main.py --transport http-stream
   ```
2. **Khởi động App UI (Host):**
   Mở source code Xcode dự án Native App Swift, cấu hình địa chỉ MCP Server endpoint (`http://localhost:8080/mcp`) và bấm `Run`.