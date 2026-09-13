# Trả lời kịch bản — Sequence Diagram chức năng Đăng nhập RikkeiShop

## 1. Các thành phần (Lifeline)

| Tên | Loại |
|---|---|
| Khách hàng | Actor |
| Màn hình UI | Object |
| AuthServer | Object |

## 2. Phân loại từng thông điệp trong kịch bản

| # | Thông điệp | Chiều | Loại | Giải thích |
|---|---|---|---|---|
| 1 | `nhapThongTin(username, password)` | Khách hàng → Màn hình UI | **Sync** | Gửi dữ liệu và chờ UI xử lý tiếp, chưa có kết quả trả về ngay |
| 2 | `verifyAccount()` | Màn hình UI → AuthServer | **Sync** | Đề bài nêu rõ UI "gửi yêu cầu... và chờ kết quả" |
| 3 | `checkCredentials()` | AuthServer → AuthServer | **Self** | Xử lý nội bộ của AuthServer, không có lifeline khác tham gia |
| 4a | Trả về Token thành công | AuthServer → Màn hình UI | **Return** | Trả kết quả cho lời gọi Sync ở bước 2, nhánh `[Thông tin hợp lệ]` |
| 5a | Hiển thị Trang chủ | Màn hình UI → Khách hàng | **Return** | Trả kết quả cho lời gọi Sync ở bước 1, nhánh `[Thông tin hợp lệ]` |
| 4b | Trả về lỗi "Sai mật khẩu" | AuthServer → Màn hình UI | **Return** | Trả kết quả cho lời gọi Sync ở bước 2, nhánh `[Thông tin không hợp lệ]` |
| 5b | Hiển thị cảnh báo lỗi | Màn hình UI → Khách hàng | **Return** | Trả kết quả cho lời gọi Sync ở bước 1, nhánh `[Thông tin không hợp lệ]` |

## 3. Vị trí khối `alt`

Khối rẽ nhánh `alt` bắt đầu **ngay sau** self-message `checkCredentials()`, bao trọn 2 ngăn:

- **Ngăn 1** — guard `[Thông tin hợp lệ]`: gồm thông điệp 4a và 5a.
- **Ngăn 2** — guard `[Thông tin không hợp lệ]`: gồm thông điệp 4b và 5b.

Hai ngăn được phân tách bằng một đường kẻ ngang nét đứt, đúng ký hiệu Combined Fragment chuẩn UML.

## 4. Lưu ý khi vẽ

Các thông điệp trả kết quả ở cả hai nhánh (4a/5a và 4b/5b) đều là **Return** — không phải Sync — vì bản chất chúng là phản hồi cho hai lời gọi Sync đã mở ra ở bước 1 và bước 2, không mở ra lời gọi mới nào.
