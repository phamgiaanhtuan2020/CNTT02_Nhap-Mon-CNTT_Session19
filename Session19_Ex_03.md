---


---

<h3 id="báo-cáo-phân-tích-ipo-khôi-phục-quyền-truy-cập-aws">Báo cáo phân tích (IPO) khôi phục quyền truy cập AWS</h3>
<h4 id="input-đầu-vào">Input (Đầu vào)</h4>
<ul>
<li>Bộ mã dự phòng (Backup Codes) đã được lưu trữ an toàn từ trước (ví dụ: trong trình quản lý mật khẩu hoặc ghi chú bảo mật ngoài thiết bị di động).</li>
</ul>
<h4 id="process-quy-trình-xử-lý">Process (Quy trình xử lý)</h4>
<ol>
<li>
<p>Truy cập trang đăng nhập AWS, điền thông tin tài khoản và mật khẩu chính xác.</p>
</li>
<li>
<p>Tại màn hình yêu cầu mã xác thực hai lớp (MFA), chọn tùy chọn sử dụng mã dự phòng (Backup Code).</p>
</li>
<li>
<p>Nhập một mã dự phòng từ danh sách (mỗi mã chỉ dùng được một lần).</p>
</li>
<li>
<p>Sau khi truy cập thành công vào bảng điều khiển AWS, lập tức vào phần cài đặt bảo mật (IAM / Security Credentials).</p>
</li>
<li>
<p>Vô hiệu hóa và xóa thiết bị MFA liên kết với chiếc điện thoại đã mất.</p>
</li>
<li>
<p>Thiết lập cấu hình MFA mới trên điện thoại thay thế và tạo ra một bộ mã dự phòng mới hoàn toàn.</p>
</li>
</ol>
<h4 id="output-đầu-ra">Output (Đầu ra)</h4>
<ul>
<li>
<p>Khôi phục thành công quyền kiểm soát hệ thống AWS.</p>
</li>
<li>
<p>Thiết bị cũ bị tước quyền truy cập vĩnh viễn, bộ mã dự phòng cũ bị hủy bỏ và hệ thống được bảo mật lại bằng cấu hình mới.</p>
</li>
</ul>
<h3 id="tại-sao-không-được-lưu-backup-codes-dưới-dạng-ảnh-trong-thư-viện-điện-thoại">Tại sao không được lưu Backup Codes dưới dạng ảnh trong thư viện điện thoại?</h3>
<ul>
<li>
<p><strong>Rủi ro lộ dữ liệu khi mất máy:</strong> Nếu điện thoại bị mất mà kẻ gian mở được khóa màn hình hoặc can thiệp vào bộ nhớ, chúng sẽ dễ dàng tìm thấy bức ảnh chụp mã dự phòng trong thư viện ảnh.</p>
</li>
<li>
<p><strong>Nguy cơ từ tính năng đồng bộ đám mây:</strong> Các ứng dụng sao lưu ảnh tự động (như Google Photos, iCloud) có thể đưa ảnh chứa mã dự phòng lên không gian mạng. Nếu tài khoản đám mây bị xâm nhập, kẻ tấn công sẽ nắm giữ toàn bộ mã vượt qua lớp bảo mật MFA của mọi hệ thống.</p>
</li>
<li>
<p><strong>Thiếu lớp mã hóa:</strong> Hình ảnh chứa văn bản thô không được mã hóa bảo vệ như các phần mềm quản lý mật khẩu chuyên dụng, tạo ra lỗ hổng nghiêm trọng cho toàn bộ hệ thống doanh nghiệp.</p>
</li>
</ul>

