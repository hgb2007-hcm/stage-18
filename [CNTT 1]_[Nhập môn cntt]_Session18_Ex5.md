# Tối ưu tiện ích mở rộng cho hệ thống điều phối vận tải

## ***Phần 1. Phân tích: Vai trò của JSON Viewer trong gỡ lỗi API dữ liệu GPS***

Khi máy chủ truyền dữ liệu lộ trình xe hoặc tọa độ GPS về client, dữ liệu thường ở dạng chuỗi JSON thô (Raw JSON) chưa qua định dạng (ví dụ: một khối văn bản nén không xuống dòng, thiếu thụt lề).

- Tầm quan trọng của JSON Viewer:

   - Trực quan hóa cấu trúc: Tự động thụt lề (indentation), mã hóa màu sắc (syntax highlighting) giúp phân biệt rõ ràng giữa Key, String, Number và Boolean.

   - Thao tác linh hoạt: Hỗ trợ thu gọn/mở rộng (Collapse/Expand) các Object và Array phức tạp, giúp kỹ sư nhanh chóng định vị chính xác mảng tọa độ GPS [latitude, longitude] hay trạng thái thiết bị mà không bị rối mắt.

   - Copy & Kiểm tra nhanh: Cho phép copy đường dẫn thuộc tính (JSON Path) để dùng ngay trong code hoặc kiểm tra xem API trả về có thiếu trường dữ liệu nào không.

## ***Phần 2. Triển khai: Danh sách 5 Extensions thiết yếu & Phân quyền an toàn***

**1. Danh sách 5 Extensions chia theo 3 nhóm chức năng**

**Nhóm 1: Dev Suite (Soi & Gỡ lỗi API)**

- JSON Formatter / JSON Viewer: Tự động định dạng và làm đẹp chuỗi JSON thô từ các Endpoint API GPS.

- Postman Interceptor (hoặc Talend API Tester): Hỗ trợ soi, chỉnh sửa và gửi thử các HTTP Request API điều phối trực tiếp trên trình duyệt.

**Nhóm 2: Productivity (Tối ưu giao diện & Thao tác)**

- ColorZilla: Công cụ chấm mã màu (Color Picker) giúp kỹ sư bắt nhanh mã màu hex/RGB của biểu tượng xe, trạng thái đơn hàng trên bản đồ UI.

- Wappalyzer: Phân tích nhanh các công nghệ/thư viện mà trang web điều phối đối thủ hoặc đối tác đang sử dụng.

**Nhóm 3: Security (Bảo mật & Tăng tốc độ)**

- uBlock Origin: Extension chặn quảng cáo và mã theo dõi độc hại hiệu quả nhất, tối ưu dung lượng bộ nhớ và nhẹ hơn đáng kể so với AdBlock Plus.

**2. Thao tác ghim tiện ích lên thanh công cụ (Toolbar)**

- Nhấp vào biểu tượng Mảnh ghép puzzle ($\ puzzle$) ở góc trên bên phải trình duyệt Chrome/Edge.

- Tìm tên extension cần dùng và nhấp vào biểu tượng Ghim (Pin) bên cạnh. Icon của tiện ích sẽ xuất hiện trực tiếp trên Toolbar để thao tác chỉ với 1-click.

**3. Thiết lập chế độ Site Access (On Click) để bảo vệ dữ liệu nhạy cảm**

Để ngăn các extension tự động đọc dữ liệu tọa độ GPS hoặc thông tin đơn hàng nhạy cảm trên trang greenlog.vn:

- Nhấp chuột phải vào biểu tượng Extension trên Toolbar $\rightarrow$ Chọn This Can Read and Change Site Data (Tiện ích này có thể đọc và thay đổi dữ liệu trang web).

- Chuyển từ mặc định "On All Sites" sang "When You Click the Extension" (Khi bạn nhấp vào tiện ích).

- Kết quả: Extension chỉ được phép hoạt động khi bạn chủ động click vào nó, triệt tiêu rủi ro lộ lọt dữ liệu nội bộ.

## ***Phần 3. Kiểm thử: Quy trình kiểm tra & Tối ưu mức tiêu thụ tài nguyên (RAM/CPU)***

Nếu trình duyệt bị giật lag khi tải biểu đồ dữ liệu lớn (Waterfall chart) trên tab Network, bạn cần cô lập và tắt các extension chiếm dụng tài nguyên theo các bước sau:

**Quy trình sử dụng Task Manager của trình duyệt:**

1. Nhấn tổ hợp phím Shift + Esc trên trình duyệt (hoặc vào Menu 3 chấm $\rightarrow$ More Tools $\rightarrow$ Task Manager).

2. Bảng quản lý tiến trình sẽ hiển thị rõ mức tiêu thụ RAM (Memory footprint) và CPU của từng Tab cũng như từng Extension đang chạy ngầm (Extension: <Tên extension>).

3. Sắp xếp danh sách theo cột Memory để tìm ra "thủ phạm" đang ngốn tài nguyên nhất.    

