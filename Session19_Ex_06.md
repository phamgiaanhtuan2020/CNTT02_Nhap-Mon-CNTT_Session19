---


---

<h3 id="pre-push-checklist-và-cơ-chế-quét-tự-động">1. Pre-push checklist và cơ chế quét tự động</h3>
<p>Sử dụng công cụ <strong>GitLeaks</strong> tích hợp vào Git Hook (<code>pre-push</code>) để chặn commit chứa secret ngay tại máy cá nhân.</p>
<p>Tạo tệp <code>.githooks/pre-push</code>:</p>
<p>Bash</p>
<pre><code>#!/bin/sh
# Kịch bản tự động quét secret trước khi push

echo "Đang kiểm tra secret trong mã nguồn..."

# Kiểm tra công cụ gitleaks
if ! command -v gitleaks &amp;&gt; /dev/null; then
    echo "LỖI: Chưa cài đặt gitleaks. Vui lòng cài đặt trước khi push."
    exit 1
fi

# Chạy quét các commit sắp push
gitleaks detect --source . --verbose

if [ $? -ne 0 ]; then
    echo "CẢNH BÁO: Phát hiện API Key hoặc Password hardcode! Push bị hủy."
    exit 1
fi

echo "Mã nguồn an toàn. Tiếp tục push..."
exit 0

</code></pre>
<p>Phân quyền và kích hoạt hook cho toàn bộ team:</p>
<p>Bash</p>
<pre><code>chmod +x .githooks/pre-push
git config core.hooksPath .githooks

</code></pre>
<p>Danh sách kiểm tra thủ công cho lập trình viên:</p>
<ul>
<li>
<p>Không lưu trực tiếp API Key, Secret Token, Private Key vào bất kỳ tệp nguồn nào.</p>
</li>
<li>
<p>Kiểm tra các tệp cấu hình local (như <code>config.js</code>,  <code>settings.py</code>).</p>
</li>
<li>
<p>Xác nhận các tệp chứa dữ liệu nhạy cảm đã nằm trong <code>.gitignore</code>.</p>
</li>
</ul>
<h3 id="cấu-trúc-tệp-.env.example">2. Cấu trúc tệp .env.example</h3>
<p>Mẫu cấu trúc tệp <code>.env.example</code> chuẩn hóa cho các môi trường:</p>
<p>Đoạn mã</p>
<pre><code># ==========================================
# CẤU HÌNH HỆ THỐNG (ENVIRONMENT CONFIG)
# ==========================================
NODE_ENV=development # Giá trị chấp nhận: development | testing | production
PORT=3000

# ==========================================
# CƠ SỞ DỮ LIỆU (DATABASE CONFIG)
# ==========================================
DB_HOST=localhost
DB_PORT=5432
DB_NAME=ecommerce_db
DB_USER=your_db_user
DB_PASSWORD=your_db_password

# ==========================================
# BẢO MẬT VÀ XÁC THỰC (AUTHENTICATION)
# ==========================================
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRATION=86400

# ==========================================
# CỔNG THANH TOÁN (PAYMENT GATEWAY)
# ==========================================
STRIPE_PUBLIC_KEY=pk_test_your_stripe_public_key
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key

# ==========================================
# DỊCH VỤ EMAIL (MAIL SERVICE)
# ==========================================
SMTP_HOST=smtp.mailtrap.io
SMTP_PORT=2525
SMTP_USER=your_smtp_user
SMTP_PASSWORD=your_smtp_password

</code></pre>
<h3 id="hướng-dẫn-bật-2fa-bắt-buộc-trên-github-repository">3. Hướng dẫn bật 2FA bắt buộc trên GitHub Repository</h3>
<p>Để đảm bảo mọi thành viên có quyền đóng góp mã nguồn đều bật 2FA:</p>
<h4 id="bước-1-yêu-cầu-2fa-cấp-organization-nếu-dùng-github-organization">Bước 1: Yêu cầu 2FA cấp Organization (Nếu dùng GitHub Organization)</h4>
<ol>
<li>
<p>Truy cập vào <strong>Organization Settings</strong>.</p>
</li>
<li>
<p>Chọn mục <strong>Account security</strong>.</p>
</li>
<li>
<p>Tích chọn <strong>Require two-factor authentication for everyone in your organization</strong>.</p>
</li>
<li>
<p>Bấm <strong>Save</strong>. Các tài khoản chưa bật 2FA sẽ tự động bị xóa quyền truy cập cho đến khi hoàn tất cài đặt 2FA.</p>
</li>
</ol>
<h4 id="bước-2-thiết-lập-branch-protection-rules-trên-repository">Bước 2: Thiết lập Branch Protection Rules trên Repository</h4>
<ol>
<li>
<p>Vào <strong>Settings</strong> của Repository &gt; <strong>Branches</strong>.</p>
</li>
<li>
<p>Thêm rule cho branch chính (<code>main</code> hoặc <code>production</code>).</p>
</li>
<li>
<p>Bật các tùy chọn:</p>
<ul>
<li>
<p><strong>Require a pull request before merging</strong>.</p>
</li>
<li>
<p><strong>Require status checks to pass before merging</strong> (gắn cùng công cụ CI quét gitleaks/CodeQL).</p>
</li>
<li>
<p><strong>Restrict who can push to matching branches</strong>.</p>
</li>
</ul>
</li>
</ol>
<h3 id="cấu-trúc-repository-mẫu">4. Cấu trúc Repository mẫu</h3>
<p>Plaintext</p>
<pre><code>ecommerce-secure-repo/
├── .githooks/
│   └── pre-push
├── .github/
│   └── workflows/
│       └── security-scan.yml
├── .env.example
├── .gitignore
├── README.md
└── src/
    └── index.js

</code></pre>
<p>Nội dung tệp <code>.gitignore</code> chuẩn:</p>
<p>Đoạn mã</p>
<pre><code># Tệp môi trường chứa secret
.env
.env.local
.env.production

# Thư viện phụ thuộc
node_modules/

# Tệp nhật ký và bộ nhớ tạm
*.log
.DS_Store
dist/
build/

</code></pre>
<h3 id="báo-cáo-đánh-giá-rủi-ro-khi-thiếu-tệp-.gitignore">5. Báo cáo đánh giá rủi ro khi thiếu tệp .gitignore</h3>
<h4 id="rủi-ro-1-rò-rỉ-chứng-thư-bảo-mật-và-mã-truy-cập">Rủi ro 1: Rò rỉ chứng thư bảo mật và mã truy cập</h4>
<ul>
<li>
<p><strong>Mô tả:</strong> Thiếu <code>.gitignore</code> khiến tệp <code>.env</code> chứa database credentials, JWT secret, API key cổng thanh toán bị đưa lên kho mã nguồn công khai.</p>
</li>
<li>
<p><strong>Hậu quả:</strong> Kẻ tấn công quét tự động kho mã nguồn công khai (SaaS bot scanner), chiếm quyền truy cập cơ sở dữ liệu, đánh cắp dữ liệu khách hàng và thực hiện giao dịch giả mạo.</p>
</li>
</ul>
<h4 id="rủi-ro-2-thất-thoát-dữ-liệu-và-vi-phạm-pháp-luật">Rủi ro 2: Thất thoát dữ liệu và vi phạm pháp luật</h4>
<ul>
<li>
<p><strong>Mô tả:</strong> Thông tin cá nhân (PII), thông tin thẻ thanh toán của người dùng bị khai thác từ cơ sở dữ liệu bị lộ.</p>
</li>
<li>
<p><strong>Hậu quả:</strong> Doanh nghiệp chịu phạt vi phạm quy định bảo mật dữ liệu, đền bù thiệt hại tài chính và tổn hại nghiêm trọng đến uy tín thương hiệu.</p>
</li>
</ul>
<h4 id="rủi-ro-3-chi-phí-hạ-tầng-đột-biến">Rủi ro 3: Chi phí hạ tầng đột biến</h4>
<ul>
<li>
<p><strong>Mô tả:</strong> API Keys của các dịch vụ đám mây (AWS, Google Cloud, Stripe) bị kẻ xấu lợi dụng để đào tiền điện tử hoặc gửi email rác.</p>
</li>
<li>
<p><strong>Hậu quả:</strong> Phát sinh chi phí vận hành dịch vụ ngoài kiểm soát trong thời gian ngắn.</p>
</li>
</ul>
<h4 id="rủi-ro-4-mất-lịch-sử-mã-nguồn-sạch-git-history-contamination">Rủi ro 4: Mất lịch sử mã nguồn sạch (Git History Contamination)</h4>
<ul>
<li>
<p><strong>Mô tả:</strong> Khi secret đã lỡ push lên Git, dù xóa tệp ở commit sau thì dữ liệu nhạy cảm vẫn tồn tại trong lịch sử commit.</p>
</li>
<li>
<p><strong>Hậu quả:</strong> Tốn thời gian xử lý triệt để (phải dùng <code>git filter-repo</code> hoặc <code>BFG Repo-Cleaner</code> để ghi đè lịch sử), đồng thời buộc phải thu hồi và cấp lại toàn bộ API Keys/Credentials bị lộ.</p>
</li>
</ul>

