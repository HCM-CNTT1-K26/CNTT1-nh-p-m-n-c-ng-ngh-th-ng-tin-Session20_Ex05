# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session20_Ex05
### 1. Thiết kế kiến trúc 3 lớp bảo vệ 

* **Lớp 1 (Knowledge - Kiến thức):** Mật khẩu chính (Master Password / Passphrase) hoặc PIN cá nhân.
* **Lớp 2 (Possession - Sở hữu & TOTP):** Mã TOTP (Time-based One-Time Password) sinh ngẫu nhiên mỗi 30 giây dựa trên thuật toán HMAC-SHA1 kết hợp giữa khóa bí mật (Secret Key) lưu cục bộ trên thiết bị và mốc thời gian UTC hiện tại.
* **Lớp 3 (Inherence/Hardware - Sinh trắc học hoặc Khóa cứng):** Xác thực sinh trắc học (FaceID/Vân tay) trên thiết bị hoặc phần cứng bảo mật độc lập (ví dụ: YubiKey).

**Cơ chế chống hacker khi bị lộ mật khẩu (Lớp 1):**

* Dù hacker đánh cắp được mật khẩu, chúng hoàn toàn bị chặn ở **Lớp 2** vì không sở hữu thiết bị chứa Secret Key để tạo mã TOTP hợp lệ tại thời điểm đó (mã thay đổi liên tục mỗi 30s và không truyền qua mạng khi đăng nhập).
* Nếu vượt qua được, chúng vẫn vướng **Lớp 3** (sinh trắc học của chính chủ), khiến hệ thống từ chối cấp quyền truy cập hoặc ký giao dịch.

---

### 2. Xử lý bẫy 

* **Về mã OTP (TOTP):** Bản chất thuật toán TOTP **không cần internet hay sóng di động** để sinh mã. Ứng dụng authenticator trên điện thoại tự tính toán mã dựa trên đồng hồ nội bộ của thiết bị và Secret Key đã lưu sẵn từ trước. Do đó, người dùng vẫn lấy được mã OTP bình thường khi offline.
* **Về việc thực hiện giao dịch:**
* Ví crypto lưu trữ Private Key cục bộ trên thiết bị. Người dùng có thể tiến hành tạo và ký giao dịch ngoại tuyến (Offline/Air-gapped signing) bằng cách nhập mã TOTP offline và xác thực sinh trắc học.
* Giao dịch sau đó sẽ được lưu tạm trong bộ nhớ cục bộ (Local Storage) của ứng dụng và tự động phát sóng (broadcast) lên blockchain ngay khi thiết bị kết nối lại internet.
