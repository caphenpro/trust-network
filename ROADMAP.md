# Trust Network – Development Roadmap

**Phiên bản:** 0.1.0  
**Trạng thái:** Lộ trình định hướng; các mốc chưa được xem là đã hoàn thành.

Lộ trình được chia thành các giai đoạn độc lập. Mục tiêu là có một nguyên mẫu nhỏ, kiểm thử được trước khi đầu tư vào mật mã nâng cao và sổ cái phân tán.

## Giai đoạn 0 – Công bố và đặc tả

**Mục tiêu:** Công khai phạm vi, nguyên tắc, giả định và các quyết định thiết kế ban đầu.

- [x] Tạo repository công khai.
- [x] Công bố tổng quan dự án.
- [x] Công bố lộ trình phát triển.
- [ ] Hoàn thiện SRS và glossary.
- [ ] Hoàn thiện mô hình đe dọa (threat model) sơ bộ.
- [ ] Chốt chính sách dữ liệu, quyền riêng tư và xử lý lạm dụng cho prototype.

**Tiêu chí hoàn thành:** Tài liệu mô tả rõ điều gì thuộc phạm vi prototype, điều gì chỉ là định hướng tương lai và các giới hạn đã biết.

## Giai đoạn 1 – Reference Prototype

**Mục tiêu:** Chứng minh luồng nghiệp vụ cốt lõi trên một môi trường thử nghiệm có kiểm soát.

### 1.1. Tài khoản và định danh
- Tạo tài khoản với mã định danh duy nhất, cố định.
- Gắn họ tên công khai với mã tài khoản qua sổ đăng ký định danh có phiên bản.
- Quản lý trạng thái tài khoản và khóa/credential.

### 1.2. Sao lưu và phục hồi
- Xuất gói sao lưu tài khoản được mã hóa bằng mật khẩu.
- Nhập gói sao lưu trên thiết bị khác.
- Khôi phục cùng account ID và public ID.
- Kiểm tra tính toàn vẹn của bản sao lưu.
- Nếu không còn bản sao lưu hoặc phương thức phục hồi, cho phép tạo tài khoản mới; không tự chuyển điểm và lịch sử từ tài khoản cũ.

### 1.3. Phát hiện tài khoản gần
- Thử nghiệm BLE trên thiết bị/trình duyệt tương thích.
- Tra cứu tên từ sổ đăng ký định danh, không phát tên trực tiếp qua beacon.
- Có QR Code hoặc luồng trực tuyến làm phương án dự phòng.
- Hiển thị danh sách tài khoản phát hiện được trong phiên quét.

### 1.4. Đánh giá và quyền xem
- Chỉ có hai giá trị: 1 (Thích), 0 (Không thích).
- Một evaluator chỉ đánh giá một target một lần trong một ngày.
- Không sửa/hủy đánh giá đã xác nhận.
- Ẩn điểm của target cho đến khi viewer hoàn tất đánh giá target.
- Không hiển thị evaluator cho target.

### 1.5. Chỉ số và công bố
- Tính Trust Score = lượt thích / tổng lượt đánh giá × 100%.
- Tính Confidence Count = số evaluator duy nhất trong vòng đời target.
- Không có dữ liệu thì hiển thị “Chưa có dữ liệu”.
- Tách ghi nhận giao dịch liên tục khỏi công bố snapshot theo kỳ một ngày.
- Định nghĩa ngày giao dịch offline theo thời điểm mạng lưới chấp nhận, không dựa vào đồng hồ thiết bị.

### 1.6. Đồng bộ và kiểm thử
- Hàng đợi đánh giá đã ký khi offline.
- Gửi và xử lý hàng đợi khi có mạng.
- Kiểm thử trùng lặp, phát lại (replay), mất kết nối và phục hồi ứng dụng.
- Kiểm thử trên ít nhất hai thiết bị thực.

**Tiêu chí hoàn thành:** Hai thiết bị thử nghiệm có thể tạo tài khoản, phát hiện/tra cứu nhau qua ít nhất một phương thức hỗ trợ, đánh giá, đồng bộ, tính chỉ số và tuân thủ quyền xem; một tài khoản có thể được phục hồi từ tệp sao lưu sang thiết bị khác.

## Giai đoạn 2 – Hoàn thiện và tăng cường an toàn

**Mục tiêu:** Giảm rủi ro thao túng, rò rỉ dữ liệu và lỗi vận hành.

- Nghiên cứu cơ chế xác minh một người/một credential phù hợp với mức độ riêng tư đã chọn.
- Thiết kế anonymous credentials, blind signature hoặc ZK proof phù hợp; không thay bằng hash tài khoản đơn giản.
- Thiết kế daily nullifier để ngăn đánh giá lặp trong ngày.
- Thiết kế lifetime nullifier theo từng target để đếm evaluator duy nhất mà không tạo mã liên kết toàn cục.
- Bổ sung rate limit, phát hiện hành vi bất thường, báo cáo lạm dụng và quy trình xử lý.
- Rà soát quyền truy cập API, RLS, quản lý khóa, mã hóa dữ liệu và nhật ký vận hành.
- Kiểm thử phục hồi dữ liệu, thay đổi thiết bị và tình huống mất khóa.
- Hoàn thiện chính sách quyền riêng tư, lưu trữ, sửa định danh và xử lý yêu cầu dữ liệu.
- Đánh giá độc lập về bảo mật trước khi mở rộng người dùng.

**Tiêu chí hoàn thành:** Có threat model, kiểm thử bảo mật có thể tái lập, quy trình xử lý lạm dụng và tài liệu rõ các giới hạn còn tồn tại.

## Giai đoạn 3 – Sổ cái phân tán

**Mục tiêu:** Chuyển từ backend tham chiếu sang kiến trúc có nhiều validator độc lập.

- Xác định mô hình validator và quản trị validator.
- Thử nghiệm permissioned BFT với các validator độc lập.
- Ký block và kiểm tra chữ ký.
- Xây dựng cơ chế đồng thuận, finality và chống phát lại.
- Tạo Merkle root/checkpoint cho các kỳ công bố.
- Xây dựng đồng bộ node, phát hiện phân nhánh và phục hồi sổ cái.
- Định nghĩa quy trình nâng cấp giao thức và xử lý sự cố.
- Đánh giá khả năng phân quyền thực chất; nhiều bản sao cơ sở dữ liệu đơn lẻ không được xem là blockchain phân tán.

**Tiêu chí hoàn thành:** Có nhiều validator độc lập, quy trình đồng thuận/finality được kiểm thử, node có thể đồng bộ và xác minh lịch sử mà không phụ thuộc một máy chủ duy nhất.

## Các nguyên tắc xuyên suốt

1. **Privacy by design:** không đưa evaluator ID vào dữ liệu đánh giá công khai.
2. **Data minimization:** chỉ lưu dữ liệu cần thiết cho chức năng và kiểm toán.
3. **Recoverability:** tài khoản không phụ thuộc vào một thiết bị; bản sao lưu phải được bảo vệ.
4. **Offline-first:** thao tác ngoại tuyến phải có trạng thái rõ ràng và đồng bộ an toàn.
5. **No false guarantees:** phát hiện gần không chứng minh hiện diện; blockchain không chứng minh con người thật.
6. **Incremental delivery:** mỗi giai đoạn phải có tiêu chí kiểm thử trước khi mở rộng.
7. **Transparent limitations:** công bố rõ sai số, thiên lệch, nguy cơ Sybil và thông đồng.

## Ngoài phạm vi giai đoạn đầu

- Không tuyên bố xác minh danh tính pháp lý.
- Không tuyên bố điểm số là thước đo khách quan về phẩm chất con người.
- Không triển khai token, tiền mã hóa hoặc cơ chế thưởng tài chính.
- Không cam kết BLE hoạt động trên mọi trình duyệt/hệ điều hành.
- Không xây dựng blockchain công khai ngay trong prototype.

## Trạng thái hiện tại

| Hạng mục | Trạng thái |
|---|---|
| Repository công khai | Đã tạo |
| Tổng quan dự án | Đang công bố |
| Lộ trình | Đang công bố |
| SRS chi tiết | Cần hoàn thiện |
| Database migration | Chưa triển khai |
| Backend/API | Chưa triển khai |
| Ứng dụng PWA | Chưa triển khai |
| Validator / distributed ledger | Định hướng tương lai |

Các trạng thái sẽ được cập nhật theo commit và kết quả kiểm thử thực tế, không đánh dấu hoàn thành chỉ dựa trên kế hoạch.
