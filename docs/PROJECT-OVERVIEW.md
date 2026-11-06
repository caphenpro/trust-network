# Trust Network – Project Overview & Design Principles

Tài liệu này ghi lại các quyết định thiết kế nền tảng để các giai đoạn triển khai sau không làm sai lệch mục tiêu dự án.

## 1. Danh tính công khai và đánh giá ẩn danh là hai lớp riêng

Người dùng đã chọn công khai họ tên đầy đủ cùng mã tài khoản để nhận diện tài khoản gần đó. Tuy nhiên, việc công khai tên không đồng nghĩa công khai người đánh giá.

- Sổ đăng ký định danh ánh xạ mã tài khoản sang hồ sơ tên có phiên bản.
- Không ghi họ tên trực tiếp vào block bất biến.
- Thay đổi tên tạo phiên bản mới; không ghi đè lịch sử cũ.
- Sổ đánh giá không công khai evaluator account ID.
- Bluetooth chỉ trao đổi mã nhận diện cần thiết; tên được tra cứu từ registry.

## 2. Hai chỉ số không được nhập làm một

Với một target:
- Trust Score tính trên tổng số lượt đánh giá hợp lệ, bao gồm các lượt của cùng evaluator ở các ngày khác nhau.
- Confidence Count tính số evaluator duy nhất trong toàn bộ vòng đời target.

Vì vậy, hai target có cùng Trust Score có thể có Confidence Count khác nhau. Confidence Count cũng không tự chứng minh các evaluator là người thật hoặc độc lập.

## 3. Quyền xem kết quả

Viewer chỉ được xem snapshot đã công bố của target sau khi viewer hoàn tất đánh giá target đó. Trước thời điểm này, giao diện và API đều phải ẩn các số liệu liên quan.

Quy tắc này phải được thực thi ở backend/API, không chỉ ẩn bằng giao diện.

## 4. Ghi nhận và công bố là hai tiến trình

- Ghi nhận: giao dịch được xác thực và ghi nhận theo quy tắc của mạng lưới.
- Công bố: kết quả tổng hợp được đóng theo kỳ và công bố sau một ngày.

Đánh giá offline nằm trong hàng đợi cho đến khi được mạng lưới chấp nhận. Không dùng đồng hồ cục bộ làm nguồn xác định ngày chính thức cho giao dịch offline.

## 5. Nullifier và chống liên kết danh tính

Thiết kế mục tiêu:
- Daily nullifier: ngăn cùng evaluator đánh giá cùng target nhiều lần trong một ngày.
- Lifetime nullifier: cho phép đếm evaluator duy nhất theo từng target xuyên suốt vòng đời.

Nullifier phải được thiết kế theo ngữ cảnh target và mục đích sử dụng. Không công khai một định danh evaluator ổn định có thể liên kết hành vi đánh giá giữa nhiều target. Hash đơn giản của mã tài khoản không đủ để tạo ẩn danh.

## 6. Phục hồi tài khoản

Bản đầu sử dụng tệp sao lưu được mã hóa và người dùng chủ động giữ bản sao. Tệp cần mang đủ dữ liệu/khóa cần thiết để phục hồi cùng account ID và public ID, không phải tạo danh tính mới.

Nếu mất thiết bị và mất cả bản sao lưu/phương thức phục hồi:
- Cho phép tạo tài khoản mới.
- Không tự động hợp nhất tài khoản.
- Không chuyển lịch sử hoặc chỉ số từ tài khoản cũ sang tài khoản mới.
- Tài khoản cũ vẫn được giữ theo chính sách dữ liệu.

## 7. Giới hạn kỹ thuật cần công khai

- Web Bluetooth không được hỗ trợ đồng nhất trên các trình duyệt và hệ điều hành.
- QR/Internet là fallback khả thi cho prototype.
- BLE proximity không chứng minh người thật đang có mặt hoặc không thể bị relay.
- Một PostgreSQL server có nhiều bản sao không tự trở thành blockchain phân tán.
- Distributed ledger thực chất cần validator độc lập, cơ chế đồng thuận, chữ ký, finality, đồng bộ và quản trị.

## 8. Kiến trúc theo giai đoạn

Prototype có thể bắt đầu với React/TypeScript, IndexedDB, API và PostgreSQL. Các giao diện cần tách lớp để sau này thay thế backend tham chiếu bằng mạng validator mà không viết lại toàn bộ nghiệp vụ.

Không được mô tả các công nghệ định hướng là tính năng đã hoàn thành cho đến khi có mã nguồn, kiểm thử và kết quả triển khai tương ứng.
