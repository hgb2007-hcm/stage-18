#  Kiểm tra và tối ưu hiệu năng Portal

## ***Phần 1. Phân tích: Xác định "nút thắt cổ chai" (Bottleneck) qua biểu đồ Waterfall***

Khi mở tab Network trong DevTools (F12) và kích hoạt ô Disable Cache để đo đạc chính xác, biểu đồ Waterfall sẽ chỉ ra các "nút thắt cổ chai" sau:

- Các Request bị lỗi 404 Not Found (Tô đỏ): Các file hình ảnh bản đồ lộ trình hoặc tài nguyên CSS/JS bị sai đường dẫn. Trình duyệt liên tục phải gửi yêu cầu và chờ phản hồi lỗi từ server, làm gián đoạn tiến trình render.

- Kích thước file hình ảnh quá lớn (Large Asset Payload): Các tệp hình ảnh bản đồ chưa qua nén (dung lượng lên tới vài MB) chiếm thời gian tải (Download time) cực lớn trong hàng đợi Network, làm nghẽn băng thông truyền tải các file kịch bản quan trọng khác.

- Độ trễ thời gian chờ (Waiting / TTFB): Server mất quá nhiều thời gian để xử lý hoặc phản hồi do không có cơ chế cache tĩnh, khiến biểu đồ Waterfall bị kéo dài thườn thượt với các thanh màu xám và xanh dương đậm.

## ***Phần 2. Triển khai: Quy trình 3 bước tối ưu hiệu năng***

Để giải quyết triệt để tình trạng trên và đảm bảo các file quan trọng luôn đạt mã 200 OK trước khi Rendering Engine tô vẽ giao diện, hãy thực hiện quy trình sau:

**Sửa điểm gãy và lỗi đứt link (404)**

- Kiểm tra các request bị tô đỏ trên tab Network, rà soát lại đường dẫn URL của hình ảnh bản đồ lộ trình hoặc file script trong mã nguồn để sửa lại chính xác tuyệt đối, tránh tình trạng link hỏng gây gián đoạn render.

**Nén và tối ưu tài nguyên hình ảnh**

- Tiến hành nén các file hình ảnh bản đồ (chuyển sang định dạng tối ưu như WebP hoặc giảm dung lượng mà không làm mất chi tiết). Điều này giúp giảm kích thước gói tin xuống mức tối thiểu, tăng tốc độ tải trên thiết bị của shipper.
Verification: Kích thước (Size) của các tệp hình ảnh trên tab Network giảm từ hàng MB xuống còn vài chục KB.

**Cấu hình cơ chế Browser Cache cho tài nguyên tĩnh**

- Thiết lập header Cache-Control cho các tệp tĩnh (hình ảnh, CSS, JavaScript) trên server để trình duyệt tự động lưu trữ vào bộ nhớ đệm cho những lần truy cập sau, không phải tải lại từ đầu.

## ***Phần 3. Kiểm thử: Ý nghĩa của mã trạng thái 304 Not Modified***

Khi bạn reload lại trang mà một số tài nguyên trả về mã 304 Not Modified, đây là một tín hiệu cực kỳ tốt cho băng thông hệ thống:

- Bản chất: Mã 304 cho biết tài nguyên trên trình duyệt của người dùng vẫn khớp hoàn toàn với phiên bản mới nhất hiện đang nằm trên máy chủ.

- Lợi ích băng thông: Thay vì truyền tải lại toàn bộ nội dung file (dung lượng nặng) qua mạng, máy chủ chỉ phản hồi một gói tin header siêu nhỏ xác nhận dữ liệu cũ vẫn dùng tốt. Điều này giúp tiết kiệm tối đa băng thông mạng, giảm tải cho server và tăng tốc độ hiển thị trang web gần như tức thời cho shipper.