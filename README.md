# Trust Network

**Mạng lưới tín nhiệm tham chiếu – phát hiện gần, đánh giá ẩn danh và công bố chỉ số theo chu kỳ.**

> **Trạng thái dự án:** Planning / Reference Architecture  
> **Phiên bản công bố:** 0.1.0 – Project Overview  
> **Mục đích:** Nghiên cứu, tham khảo và thử nghiệm; chưa phải dịch vụ đánh giá con người đã được chứng nhận.

Trust Network là dự án mã nguồn mở thử nghiệm một mô hình mạng lưới cho phép người tham gia phát hiện các tài khoản đang hoạt động ở gần, gửi đánh giá nhị phân ẩn danh và tham khảo các chỉ số tổng hợp được công bố theo chu kỳ.

Dự án được phát triển theo từng giai đoạn. Phiên bản đầu ưu tiên một nguyên mẫu dễ triển khai, có thể chạy thử và có khả năng mở rộng; các cơ chế mật mã nâng cao và sổ cái phân tán hoàn chỉnh sẽ được nghiên cứu, bổ sung sau.

## Mục tiêu

- Cấp cho mỗi tài khoản một mã định danh cố định, duy nhất.
- Cho phép người dùng tìm thấy tài khoản đang tham gia ở gần qua Bluetooth Low Energy (BLE); QR Code hoặc phương thức trực tuyến là phương án dự phòng.
- Cho phép đánh giá bằng hai giá trị: `1 = Thích`, `0 = Không thích`.
- Giữ kín danh tính người đánh giá.
- Tách biệt số lượt đánh giá với số người đánh giá duy nhất.
- Ghi nhận sự kiện khi mạng lưới xác thực; tổng hợp và công bố kết quả theo kỳ.
- Hỗ trợ hoạt động offline-first, đồng bộ khi có kết nối.
- Cho phép sao lưu và phục hồi tài khoản khi đổi hoặc mất điện thoại.

## Hai chỉ số

### 1. Điểm tín nhiệm – Trust Score

Tỷ lệ đánh giá tích cực trên tổng số lượt đánh giá:

`S = L / (L + D) × 100%`

Trong đó:
- `L`: tổng số lượt thích.
- `D`: tổng số lượt không thích.

Nếu chưa có đánh giá hợp lệ, hiển thị **“Chưa có dữ liệu”**, không hiển thị 0%.

Một người có thể đánh giá cùng tài khoản vào nhiều ngày khác nhau. Các lượt hợp lệ được tính vào tổng lượt đánh giá của chỉ số này.

### 2. Điểm tin cậy – Confidence Count

Số tài khoản đánh giá duy nhất đã tham gia đánh giá mục tiêu trong toàn bộ vòng đời:

`T = U`

Trong đó `U` là số evaluator duy nhất đối với tài khoản được đánh giá. Một evaluator đánh giá cùng mục tiêu nhiều ngày vẫn chỉ đóng góp một đơn vị vào chỉ số này.

> Hai chỉ số mô tả dữ liệu trong hệ thống, không phải kết luận tuyệt đối về phẩm chất, uy tín hay mức độ an toàn của một con người.

## Nguyên tắc hoạt động đã thống nhất

1. Mỗi tài khoản có mã định danh cố định, ví dụ `TN-XXXXXXXX`.
2. Họ tên đầy đủ được công khai trong sổ đăng ký định danh, gắn với mã tài khoản.
3. Khi người dùng bật chế độ quét, ứng dụng phát hiện các tài khoản tham gia ở gần và tra cứu tên từ sổ đăng ký.
4. Không yêu cầu kết bạn hoặc chờ người kia chấp thuận trước khi đánh giá.
5. Mỗi cặp tài khoản chỉ được đánh giá một lần trong một ngày. Có thể đánh giá lại vào ngày khác; đánh giá đã xác nhận không được sửa hoặc hủy.
6. Người dùng chỉ xem điểm của một tài khoản sau khi đã hoàn tất đánh giá tài khoản đó.
7. Người nhận không được biết danh tính người đánh giá; không công khai lịch sử evaluator hoặc thời điểm chính xác có thể hỗ trợ suy đoán.
8. Sự kiện được ghi nhận vào sổ cái khi kết nối và xác thực thành công. Khi offline, đánh giá đã ký được lưu hàng đợi cục bộ và gửi khi có mạng.
9. Kết quả được tổng hợp và công bố sau một ngày theo kỳ. Thời điểm ghi nhận giao dịch và thời điểm công bố kết quả là hai bước riêng biệt.
10. Không tin cậy đồng hồ thiết bị để xác định ngày chính thức cho giao dịch offline; ngày được xác định theo quy tắc chấp nhận của mạng lưới.

## Sao lưu và phục hồi tài khoản

Phiên bản đầu dự kiến hỗ trợ:
- Xuất gói sao lưu tài khoản thành tệp.
- Bảo vệ gói sao lưu bằng mật khẩu/mã hóa.
- Nhập tệp sao lưu trên điện thoại mới để phục hồi cùng tài khoản và mã định danh.
- Nếu mất thiết bị và không còn bản sao lưu hoặc phương thức phục hồi, người dùng có thể tạo tài khoản mới.

Tài khoản cũ và lịch sử đã ghi nhận vẫn được giữ theo chính sách lưu trữ. Không tự động chuyển điểm hoặc lịch sử từ tài khoản cũ sang tài khoản mới. Cơ chế phục hồi nâng cao có thể được bổ sung trong tương lai.

## Kiến trúc định hướng

| Lớp | Công nghệ / phương án dự kiến |
|---|---|
| Ứng dụng | React, TypeScript, Vite, PWA |
| Offline | IndexedDB, hàng đợi đồng bộ |
| Phát hiện gần | BLE; QR Code/Internet dự phòng |
| Backend tham chiếu | API TypeScript, PostgreSQL |
| Chữ ký | Ed25519 |
| Bảo vệ riêng tư | Anonymous credentials, nullifier, ZK proof khi phù hợp |
| Sổ cái tương lai | Nhiều validator độc lập, đồng thuận, finality, checkpoint |
| Phát triển | GitHub, kiểm thử tự động, tài liệu hóa |

Đây là định hướng kỹ thuật, không phải tuyên bố rằng các thành phần đã được triển khai. BLE trên trình duyệt có giới hạn tương thích giữa các hệ điều hành và trình duyệt; có thể cần ứng dụng native hoặc lớp tích hợp riêng để hỗ trợ rộng hơn.

## Lộ trình

Chi tiết được quản lý tại [ROADMAP.md](ROADMAP.md).

- **Giai đoạn 1 – Reference Prototype:** tài khoản, định danh, phát hiện gần, đánh giá 0/1, chỉ số, quyền xem sau đánh giá, sao lưu/phục hồi tệp và đồng bộ cơ bản.
- **Giai đoạn 2 – Hoàn thiện:** tăng cường bảo mật, chống tài khoản giả và thao túng, kiểm thử riêng tư, cải thiện đồng bộ và phục hồi.
- **Giai đoạn 3 – Distributed Ledger:** validator độc lập, đồng thuận, chữ ký block, finality, checkpoint và phục hồi sổ cái.

## Giới hạn, rủi ro và trách nhiệm

- BLE hoặc QR chỉ cung cấp tín hiệu phát hiện/trao đổi mã; không tự chứng minh một người thật sự hiện diện.
- Blockchain không tự chứng minh danh tính thật, không tự ngăn Sybil và không tự bảo đảm ẩn danh.
- Chỉ số có thể bị ảnh hưởng bởi tài khoản giả, thông đồng, thiên lệch mẫu và hành vi thao túng.
- Việc công khai họ tên cần được thiết kế cùng chính sách quyền riêng tư, cơ chế sửa thông tin và quy trình xử lý yêu cầu dữ liệu.
- Sổ đánh giá không được công khai định danh evaluator. Hash đơn giản từ mã tài khoản không đủ để bảo vệ ẩn danh.
- Phiên bản thử nghiệm không nên được dùng làm căn cứ duy nhất cho quyết định tuyển dụng, tín dụng, pháp lý, an toàn hoặc quyết định có ảnh hưởng lớn đến quyền lợi cá nhân.

## Đóng góp

Hiện tại repository tập trung công bố ý tưởng, phạm vi và lộ trình. Khi cấu trúc mã nguồn và quy trình đóng góp được thiết lập, hướng dẫn đóng góp sẽ được bổ sung.

## Giấy phép

Giấy phép mã nguồn sẽ được xác định và bổ sung trong repository trước khi phát hành mã nguồn triển khai. Việc cấp phép mã nguồn không thay thế chính sách quyền riêng tư hoặc nghĩa vụ pháp lý liên quan đến dữ liệu cá nhân.

---

**Repository:** [caphenpro/trust-network](https://github.com/caphenpro/trust-network)  
**Project status:** Planning / Reference Architecture
