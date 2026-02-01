# 📐 SƠ ĐỒ KIẾN TRÚC HỆ THỐNG QUẢN LÝ NHÂN SỰ

## 🎯 Tổng quan

Hệ thống Quản lý Nhân sự được xây dựng trên nền tảng **Odoo 15.0**, áp dụng kiến trúc **MVC (Model-View-Controller)** và **Layered Architecture** với 6 tầng chính.

---

## 📊 Các tầng kiến trúc

### 1️⃣ **Presentation Layer (Tầng giao diện)**

**Mục đích:** Cung cấp giao diện người dùng

**Thành phần:**
- 🌐 **Web Browser**: Chrome, Firefox, Edge
- 📱 **Mobile Browser**: Responsive design
- 💻 **Desktop Client**: Truy cập qua localhost:8069

**Công nghệ:**
- HTML5, CSS3, JavaScript
- QWeb Templates (Odoo)
- Bootstrap framework

---

### 2️⃣ **Odoo Framework (Tầng framework)**

**Mục đích:** Cung cấp nền tảng và các dịch vụ cốt lõi

**Thành phần:**
- 🎮 **Web Controllers**: Xử lý HTTP requests/responses
- 🗄️ **ORM (Object-Relational Mapping)**: Ánh xạ object ↔ database
- 🔒 **Security**: Authentication, Authorization, Access Control
- ⚙️ **Workflow Engine**: Quản lý quy trình nghiệp vụ

**Tính năng:**
- Multi-tenancy support
- Session management
- Caching mechanism
- Logging & monitoring

---

### 3️⃣ **Custom Modules (Tầng module tùy chỉnh)**

Đây là **tầng nghiệp vụ chính** của hệ thống, bao gồm 4 module:

#### 📗 **Module 1: QLNS (Quản lý Nhân sự)**
**Chức năng:**
- ✅ Quản lý thông tin nhân viên
- ✅ Quản lý phòng ban
- ✅ Quản lý chức vụ
- ✅ Lưu trữ hồ sơ nhân viên

**Models:**
- `nhan_vien` (Nhân viên)
- `phong_ban` (Phòng ban)
- `chuc_vu` (Chức vụ)

---

#### 📙 **Module 2: Chấm công**
**Chức năng:**
- ✅ Ghi nhận giờ vào/ra
- ✅ Tính toán đi muộn, về sớm
- ✅ Quản lý đăng ký ca làm
- ✅ Xử lý đơn từ (nghỉ phép, đi muộn, về sớm)

**Models:**
- `bang_cham_cong` (Bảng chấm công)
- `dang_ky_ca_lam_theo_ngay` (Đăng ký ca làm)
- `don_tu` (Đơn từ)
- `dot_dang_ky` (Đợt đăng ký)

**Tính năng nổi bật:**
- Tự động tính phút đi muộn/về sớm
- Tích hợp với đơn từ để điều chỉnh
- Hỗ trợ nhiều ca làm (Sáng, Chiều, Cả ngày)

---

#### 📘 **Module 3: Tính lương**
**Chức năng:**
- ✅ Tính toán lương cơ bản
- ✅ Quản lý trợ cấp (ăn trưa, xăng xe, điện thoại, v.v.)
- ✅ Tự động tính lương theo công thức
- ✅ Quản lý ngày trả lương
- ✅ **Tích hợp Google Calendar** (Tính năng nâng cao)

**Models:**
- `tinh_luong.bang_luong` (Bảng lương)
- `tinh_luong.tro_cap` (Trợ cấp)
- `tinh_luong.ngay_tra_luong` (Ngày trả lương)

**Công thức tính lương:**
```
Tổng lương = Lương cơ bản + Tổng trợ cấp
```

---

#### 📊 **Module 4: Dashboard**
**Chức năng:**
- ✅ Hiển thị biểu đồ thống kê
- ✅ Phân tích dữ liệu (Graph views, Pivot tables)
- ✅ Menu tổng hợp truy cập nhanh
- ✅ KPI cards

**Loại biểu đồ:**
- 📊 **Bar Chart**: Thống kê nhân viên theo phòng ban, lương theo nhân viên
- 🥧 **Pie Chart**: Phân bổ nhân viên theo chức vụ, trạng thái chấm công
- 📈 **Line Chart**: Xu hướng lương theo tháng

---

### 4️⃣ **Integration Layer (Tầng tích hợp)**

**Mục đích:** Kết nối với các dịch vụ bên ngoài

#### 🔗 **Google Calendar Integration**

**Chức năng:**
- Tự động tạo event "Ngày trả lương" trên Google Calendar
- Nhắc nhở tự động (email + popup)
- Đồng bộ 2 chiều

**Công nghệ:**
- **Google Calendar API v3**
- **Service Account Authentication**
- **OAuth 2.0**

**Thư viện Python:**
```python
google-auth==2.48.0
google-auth-oauthlib==1.2.4
google-api-python-client==2.188.0
```

**Quy trình hoạt động:**
1. User tạo "Ngày trả lương" trong Odoo
2. Click button "Đồng bộ Google Calendar API"
3. Module gọi Google Calendar API
4. Event được tạo trên Google Calendar
5. Nhận link event để xem trực tiếp

---

### 5️⃣ **External Services (Dịch vụ bên ngoài)**

**Các dịch vụ tích hợp:**

#### 📅 **Google Calendar**
- Lưu trữ sự kiện ngày trả lương
- Gửi nhắc nhở tự động
- Đồng bộ với nhiều thiết bị

#### 📧 **Gmail API** (Tương lai)
- Gửi email phiếu lương
- Thông báo chấm công
- Nhắc nhở deadline

---

### 6️⃣ **Database Layer (Tầng cơ sở dữ liệu)**

**DBMS:** PostgreSQL 12+

**Cấu trúc:**
```
📁 Database: odoo
├── 📊 nhan_vien (15 records)
├── 📊 phong_ban (5 records)
├── 📊 chuc_vu (8 records)
├── 📊 bang_cham_cong (20 records)
├── 📊 dang_ky_ca_lam_theo_ngay (15 records)
├── 📊 don_tu (5 records)
├── 📊 bang_luong (15 records)
├── 📊 tro_cap (23 records)
└── 📊 ngay_tra_luong (3 records)
```

**Đặc điểm:**
- ACID compliance
- Foreign key constraints
- Indexing cho performance
- Backup & recovery

---

## 🔄 Luồng dữ liệu (Data Flow)

### 1. **Luồng chấm công → Tính lương**
```
Nhân viên → Chấm công → Tính số giờ làm → Tính lương
```

### 2. **Luồng tích hợp Google Calendar**
```
Tạo ngày trả lương → Click đồng bộ → Google Calendar API → Event được tạo
```

### 3. **Luồng hiển thị Dashboard**
```
Database → ORM → Models → Graph Views → Web Browser
```

---

## 🛡️ Bảo mật (Security)

### **Access Control**
- ✅ Role-based access control (RBAC)
- ✅ Record rules
- ✅ Field-level security

### **Authentication**
- ✅ Username/Password
- ✅ Session management
- ✅ Google OAuth (cho Calendar API)

### **Data Protection**
- ✅ SQL injection prevention (ORM)
- ✅ XSS protection
- ✅ CSRF tokens

---

## 📈 Khả năng mở rộng (Scalability)

### **Hiện tại:**
- 15 nhân viên
- 5 phòng ban
- 1 server

### **Tương lai:**
- Horizontal scaling với load balancer
- Database replication
- Caching layer (Redis)
- Microservices architecture

---

## 🔧 Công nghệ sử dụng

### **Backend:**
- Python 3.10
- Odoo 15.0
- PostgreSQL 12+

### **Frontend:**
- HTML5, CSS3
- JavaScript (ES6+)
- QWeb Templates
- Bootstrap 4

### **Integration:**
- Google Calendar API v3
- RESTful API
- JSON

### **DevOps:**
- Git (Version control)
- WSL2 (Development environment)
- Virtual Environment (venv)

---

## 📝 Tổng kết

Hệ thống được thiết kế theo:
- ✅ **Kiến trúc phân tầng** (Layered Architecture)
- ✅ **Nguyên tắc MVC** (Model-View-Controller)
- ✅ **Separation of Concerns**
- ✅ **Modularity & Reusability**
- ✅ **Scalability & Maintainability**

**Điểm nổi bật:**
- 🎯 Tích hợp Google Calendar API (External API)
- 📊 Dashboard với biểu đồ phân tích
- 🔄 Tự động hóa quy trình tính lương
- 🛡️ Bảo mật đa lớp

---

**Người thực hiện:** [Tên của bạn]  
**Ngày:** 02/02/2026  
**Phiên bản:** 1.0
