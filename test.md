# 📋 MÔ TẢ CÁC CHỨC NĂNG HỆ THỐNG

## 🎯 TỔNG QUAN

Hệ thống Quản lý Nhân sự bao gồm **5 modules** với **25+ chức năng** chính.

---

## 1️⃣ MODULE QUẢN LÝ NHÂN SỰ (QLNS)

### 1.1. Quản lý Nhân viên
- **Thêm nhân viên mới**: Tạo hồ sơ nhân viên với đầy đủ thông tin cá nhân
- **Sửa thông tin**: Cập nhật thông tin nhân viên (email, SĐT, địa chỉ, v.v.)
- **Xóa nhân viên**: Xóa hồ sơ nhân viên không còn làm việc
- **Xem danh sách**: Hiển thị danh sách tất cả nhân viên dạng bảng
- **Tìm kiếm**: Tìm nhân viên theo tên, email, phòng ban, chức vụ
- **Lọc dữ liệu**: Lọc theo phòng ban, chức vụ, trạng thái làm việc

### 1.2. Quản lý Phòng ban
- **Thêm phòng ban**: Tạo phòng ban mới với mã và tên
- **Sửa phòng ban**: Cập nhật thông tin phòng ban
- **Xóa phòng ban**: Xóa phòng ban (nếu không có nhân viên)
- **Xem số lượng nhân viên**: Hiển thị số nhân viên trong mỗi phòng ban

### 1.3. Quản lý Chức vụ
- **Thêm chức vụ**: Tạo chức vụ mới với lương cơ bản
- **Sửa chức vụ**: Cập nhật tên và lương cơ bản
- **Xóa chức vụ**: Xóa chức vụ (nếu không có nhân viên)
- **Thiết lập lương**: Định nghĩa mức lương cơ bản cho từng chức vụ

---

## 2️⃣ MODULE CHẤM CÔNG

### 2.1. Quản lý Đợt đăng ký
- **Tạo đợt đăng ký**: Mở đợt đăng ký ca làm cho tháng mới
- **Đóng đợt đăng ký**: Kết thúc thời gian đăng ký
- **Xem danh sách đăng ký**: Theo dõi ai đã đăng ký ca làm

### 2.2. Đăng ký ca làm
- **Đăng ký ca Sáng**: 07:30 - 11:30 (4 giờ)
- **Đăng ký ca Chiều**: 13:30 - 17:30 (4 giờ)
- **Đăng ký ca Cả ngày**: 07:30 - 17:30 (8 giờ)
- **Xem lịch đã đăng ký**: Kiểm tra ca làm đã đăng ký

### 2.3. Chấm công
- **Ghi nhận giờ vào**: Lưu thời gian nhân viên vào làm
- **Ghi nhận giờ ra**: Lưu thời gian nhân viên ra về
- **Tính đi muộn tự động**: Hệ thống tự động tính số phút đi muộn
- **Tính về sớm tự động**: Hệ thống tự động tính số phút về sớm
- **Xác định trạng thái**: Tự động phân loại (Đi làm/Đi muộn/Về sớm/Vắng mặt)
- **Xem bảng chấm công**: Xem lịch sử chấm công theo ngày/tháng

### 2.4. Quản lý Đơn từ
- **Gửi đơn xin đi muộn**: Nhân viên gửi đơn xin đi muộn với lý do
- **Gửi đơn xin về sớm**: Nhân viên gửi đơn xin về sớm với lý do
- **Gửi đơn xin nghỉ phép**: Nhân viên gửi đơn xin nghỉ
- **Duyệt đơn từ**: Quản lý duyệt/từ chối đơn từ
- **Điều chỉnh chấm công**: Đơn được duyệt tự động điều chỉnh phút đi muộn/về sớm
- **Xem lịch sử đơn từ**: Theo dõi các đơn đã gửi và trạng thái

---

## 3️⃣ MODULE TÍNH LƯƠNG

### 3.1. Quản lý Đợt làm việc
- **Tạo đợt làm việc**: Tạo đợt làm việc cho tháng mới
- **Đóng đợt làm việc**: Kết thúc đợt sau khi tính lương xong
- **Xem thông tin đợt**: Kiểm tra ngày bắt đầu, kết thúc

### 3.2. Tính lương
- **Tính lương tự động**: Tính lương cho tất cả nhân viên theo công thức
- **Tạo bảng lương**: Tự động tạo bảng lương cho từng nhân viên
- **Xác nhận bảng lương**: Duyệt bảng lương trước khi trả
- **Đánh dấu đã trả**: Cập nhật trạng thái sau khi trả lương
- **Xem phiếu lương**: Nhân viên xem phiếu lương của mình
- **In phiếu lương**: Xuất phiếu lương ra PDF

**Công thức:**
```
Tổng lương = Lương cơ bản + Tổng trợ cấp
```

### 3.3. Quản lý Trợ cấp
- **Thêm trợ cấp ăn trưa**: 30,000 VNĐ/ngày × số ngày làm việc
- **Thêm trợ cấp xăng xe**: 500,000 VNĐ/tháng (cố định)
- **Thêm trợ cấp điện thoại**: 200,000 VNĐ/tháng (cố định)
- **Thêm trợ cấp nhà ở**: 1,000,000 VNĐ/tháng (tùy chọn)
- **Thêm trợ cấp khác**: Các khoản trợ cấp đặc biệt
- **Sửa/Xóa trợ cấp**: Điều chỉnh trợ cấp trước khi xác nhận
- **Tính tổng tự động**: Hệ thống tự động cộng tổng trợ cấp

### 3.4. Ngày trả lương
- **Thiết lập ngày trả**: Chọn ngày trả lương cho đợt
- **Xem lịch trả lương**: Danh sách các ngày trả lương
- **Nhắc nhở tự động**: Thông báo trước ngày trả lương

---

## 4️⃣ MODULE DASHBOARD

### 4.1. Biểu đồ Nhân sự
- **Biểu đồ cột - Nhân viên theo phòng ban**: Số lượng nhân viên/phòng ban
- **Biểu đồ tròn - Nhân viên theo chức vụ**: Phân bổ % theo chức vụ
- **Xem chi tiết**: Click vào biểu đồ để xem danh sách

### 4.2. Biểu đồ Chấm công
- **Biểu đồ tròn - Trạng thái chấm công**: Phân bổ trạng thái (Đi làm/Đi muộn/Vắng mặt)
- **Biểu đồ cột - Phút đi muộn**: Top nhân viên đi muộn nhiều nhất
- **Biểu đồ cột - Số lần chấm công**: Thống kê theo nhân viên

### 4.3. Biểu đồ Lương
- **Biểu đồ cột - Lương theo nhân viên**: So sánh lương giữa các nhân viên
- **Biểu đồ cột - Cấu trúc lương**: Lương cơ bản vs Trợ cấp
- **Biểu đồ tròn - Trợ cấp theo loại**: Phân bổ các loại trợ cấp

### 4.4. Menu tổng hợp
- **Truy cập nhanh**: Menu tổng hợp đến tất cả chức năng
- **Phân quyền**: Hiển thị menu theo quyền người dùng

---

## 5️⃣ MODULE TÍCH HỢP GOOGLE CALENDAR

### 5.1. Cấu hình
- **Thiết lập Service Account**: Nhập thông tin xác thực Google
- **Nhập Calendar ID**: Chọn calendar để đồng bộ
- **Kiểm tra kết nối**: Test kết nối với Google API

### 5.2. Đồng bộ Calendar
- **Tạo event tự động**: Tạo event "Ngày trả lương" trên Google Calendar
- **Lưu link event**: Lưu đường dẫn để xem event
- **Xem trên Google**: Mở event trực tiếp trên Google Calendar
- **Nhắc nhở email**: Google gửi email nhắc nhở 1 ngày trước
- **Nhắc nhở popup**: Thông báo popup 1 giờ trước

### 5.3. Quản lý đồng bộ
- **Xem trạng thái**: Kiểm tra đã đồng bộ hay chưa
- **Đồng bộ lại**: Thử lại nếu lỗi
- **Xem lỗi**: Hiển thị thông báo lỗi nếu có

---

## 📊 TỔNG HỢP CHỨC NĂNG

| Module | Số chức năng | Mô tả ngắn |
|--------|--------------|------------|
| **QLNS** | 10 | Quản lý nhân viên, phòng ban, chức vụ |
| **Chấm công** | 15 | Đăng ký ca, chấm công, đơn từ |
| **Tính lương** | 13 | Tính lương, trợ cấp, ngày trả |
| **Dashboard** | 7 | Biểu đồ thống kê, báo cáo |
| **Google Calendar** | 6 | Tích hợp API, đồng bộ event |
| **TỔNG** | **51** | **chức năng** |

---

## 🎯 PHÂN QUYỀN

### 👤 Nhân viên (User)
- ✅ Xem thông tin cá nhân
- ✅ Đăng ký ca làm
- ✅ Xem bảng chấm công của mình
- ✅ Gửi đơn từ
- ✅ Xem phiếu lương của mình

### 👨‍💼 Quản lý (Manager)
- ✅ Tất cả quyền của Nhân viên
- ✅ Xem thông tin nhân viên trong phòng ban
- ✅ Duyệt đơn từ
- ✅ Xem báo cáo phòng ban

### 💻 Admin (Administrator)
- ✅ Tất cả quyền của Quản lý
- ✅ Thêm/Sửa/Xóa nhân viên
- ✅ Quản lý phòng ban, chức vụ
- ✅ Tạo đợt đăng ký, đợt làm việc
- ✅ Tính lương cho tất cả nhân viên
- ✅ Cấu hình Google Calendar
- ✅ Xem tất cả báo cáo

---

## 🔄 QUY TRÌNH NGHIỆP VỤ CHÍNH

### 1. Quy trình Chấm công
```
Admin tạo đợt đăng ký 
→ Nhân viên đăng ký ca làm 
→ Hệ thống tạo bảng chấm công 
→ Nhân viên chấm công hàng ngày 
→ Hệ thống tính đi muộn/về sớm 
→ Nhân viên gửi đơn từ (nếu cần) 
→ Quản lý duyệt đơn 
→ Hệ thống điều chỉnh chấm công
```

### 2. Quy trình Tính lương
```
Admin tạo đợt làm việc 
→ Admin tính lương tự động 
→ Hệ thống tạo bảng lương + trợ cấp 
→ Admin kiểm tra & xác nhận 
→ Admin tạo ngày trả lương 
→ Hệ thống đồng bộ Google Calendar 
→ Đến ngày trả lương 
→ Admin đánh dấu đã trả
```

---

## 💡 TÍNH NĂNG NỔI BẬT

### ⭐ Tự động hóa
- ✅ Tự động tính đi muộn, về sớm
- ✅ Tự động tính lương theo công thức
- ✅ Tự động tính tổng trợ cấp
- ✅ Tự động xác định trạng thái chấm công

### ⭐ Tích hợp bên ngoài
- ✅ Google Calendar API (External API)
- ✅ Tạo event tự động
- ✅ Nhắc nhở qua email & popup

### ⭐ Phân tích dữ liệu
- ✅ 6+ loại biểu đồ
- ✅ Thống kê theo nhiều tiêu chí
- ✅ Báo cáo xuất PDF/Excel

### ⭐ Thân thiện người dùng
- ✅ Giao diện trực quan
- ✅ Tìm kiếm & lọc dữ liệu
- ✅ Thông báo rõ ràng
- ✅ Responsive design

---

**Tạo bởi:** Hệ thống Quản lý Nhân sự  
**Ngày:** 02/02/2026  
**Phiên bản:** 1.0
