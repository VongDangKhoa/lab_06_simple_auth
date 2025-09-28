# lab_06_simple_auth

Tài liệu hướng dẫn kiểm thử hai demo xác thực đơn giản trong thư mục `simple_auth`.

Hai file demo:

- `basic_auth.js` — ví dụ rất cơ bản về xác thực (console).
- `cookie_auth.js` — ví dụ sử dụng cookie/session để giữ trạng thái đăng nhập.

Ảnh minh họa nằm trong thư mục `img/` (image-1 → image-6). Mỗi ảnh được chèn bên dưới tương ứng với bước kiểm thử.

---

## A. Kiểm thử `basic_auth.js`

1) Bước 1 — Khởi động demo

   Mở PowerShell, vào thư mục `simple_auth` rồi chạy:

   ```powershell
   node basic_auth.js
   ```

   Ảnh minh họa màn hình khi server/script khởi động:

   ![Bước 1 - chạy basic_auth](img/image-1.png)

2) Bước 2 — Gửi request (POST / kiểm tra)

   Gửi request theo hướng dẫn trong file (ví dụ dùng curl hoặc gửi HTTP request từ công cụ REST):

   Ví dụ curl (nếu endpoint là `/auth/register`):

   ```powershell
   curl -X POST http://localhost:3000/auth/register -H "Content-Type: application/json" -d '{"username":"alice","password":"pass123"}'
   ```

   Ảnh minh họa: request và các output liên quan

   ![Bước 2 - POST (1)](img/image-2.png)

   ![Bước 2 - POST (2)](img/image-3.png)

3) Bước 3 — (Nếu có) Hiển thị kết quả/kiểm tra tiếp

   Tùy demo, bạn có thể thấy kết quả trong console hoặc qua response HTTP. Ảnh trên thể hiện các trạng thái khác nhau trong quá trình test.

---

## B. Kiểm thử `cookie_auth.js` (session + cookie)

1) Bước 1 — Khởi động server

   Vào thư mục `simple_auth` và chạy:

   ```powershell
   node cookie_auth.js
   ```

   Ảnh minh họa khi server khởi động:

   ![Bước 1 - chạy cookie_auth](img/image-4.png)

2) Bước 2 — Đăng ký / gửi POST

   Tương tự, gửi POST để đăng ký hoặc gửi login:

   ```powershell
   curl -X POST http://localhost:3001/auth/register -H "Content-Type: application/json" -d '{"username":"alice","password":"pass123"}'
   ```

   Ảnh minh họa request + phản hồi (POST / register / show):

   ![Bước 2 - POST và hiển thị (1)](img/image-5.png)

   ![Bước 2 - POST và hiển thị (2)](img/image-6.png)

3) Bước 3 — Login, lưu cookie

   Dùng curl và lưu cookie ra file để kiểm thử các route bảo vệ sau khi login:

   ```powershell
   curl -c cookies.txt -X POST http://localhost:3001/auth/login -H "Content-Type: application/json" -d '{"username":"alice","password":"pass123"}'
   ```

4) Bước 4 — Truy cập route bảo vệ (ví dụ `/auth/profile`)

   Gọi GET kèm cookie đã lưu:

   ```powershell
   curl -b cookies.txt http://localhost:3001/auth/profile
   ```

5) Bước cuối — Logout

   Gọi endpoint logout để hủy session:

   ```powershell
   curl -b cookies.txt http://localhost:3001/auth/logout
   ```

