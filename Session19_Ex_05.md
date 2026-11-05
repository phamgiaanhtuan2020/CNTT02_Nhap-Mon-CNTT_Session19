---


---

<p>Thiết kế kiến trúc bảo vệ</p>
<p>Lớp thứ nhất là mật khẩu chính của người dùng. Lớp thứ hai là mã TOTP sinh tự động từ khóa bí mật lưu trong vùng nhớ an toàn của thiết bị và thời gian hiện tại. Lớp thứ ba là xác thực sinh trắc học trên phần cứng thiết bị.</p>
<p>Khi hacker có mật khẩu, chúng mới vượt qua lớp thứ nhất. Hacker hoàn toàn không thể vượt qua lớp thứ hai vì không có thiết bị vật lý chứa khóa bí mật để sinh mã TOTP hợp lệ theo thời gian thực. Thêm vào đó, lớp sinh trắc học ở lớp thứ ba ngăn chặn việc mạo danh trực tiếp.</p>
<p>Xử lý trường hợp ngoại lệ trên máy bay</p>
<p>Thuật toán TOTP không yêu cầu kết nối mạng hay sóng di động vì mã được tính toán trực tiếp từ thời gian của thiết bị và khóa bí mật đã lưu sẵn từ trước. Khi không có internet, ứng dụng vẫn tự động tạo ra mã OTP chính xác để người dùng thực hiện giao dịch.</p>

