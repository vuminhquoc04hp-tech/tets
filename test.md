# 📋 MÔ TẢ CÁC MODEL (CHỨC NĂNG) THEO MODULE

## 🎯 TỔNG QUAN

Hệ thống có **5 modules** với **12 models** chính.

---

## 1️⃣ MODULE QUẢN LÝ NHÂN SỰ (nhan_su)

### Model 1.1: `nhan_vien` (Nhân viên)

**Mục đích:** Quản lý thông tin nhân viên trong công ty

**Chức năng:**
- Lưu trữ thông tin cá nhân (họ tên, email, SĐT, ngày sinh, giới tính, địa chỉ)
- Lưu trữ thông tin công việc (phòng ban, chức vụ, lương cơ bản, ngày vào làm)
- Quản lý trạng thái làm việc (đang làm/nghỉ việc)
- Tìm kiếm và lọc nhân viên
- Xuất báo cáo danh sách nhân viên

**Các trường quan trọng:**
- `ho_va_ten`: Họ và tên đầy đủ
- `email`: Email công ty (unique)
- `phong_ban_id`: Liên kết đến phòng ban
- `chuc_vu_id`: Liên kết đến chức vụ
- `luong_co_ban`: Lương cơ bản (lấy từ chức vụ)
- `trang_thai`: Trạng thái làm việc

**Ràng buộc:**
- Email phải duy nhất
- Phải thuộc 1 phòng ban và 1 chức vụ
- Ngày sinh < ngày hiện tại

---

### Model 1.2: `phong_ban` (Phòng ban)

**Mục đích:** Quản lý các phòng ban trong công ty

**Chức năng:**
- Tạo/sửa/xóa phòng ban
- Lưu trữ mã và tên phòng ban
- Hiển thị số lượng nhân viên trong phòng ban
- Phân loại nhân viên theo phòng ban

**Các trường quan trọng:**
- `ten_phong_ban`: Tên phòng ban (VD: "Phòng Kỹ thuật")
- `ma_phong_ban`: Mã phòng ban (VD: "IT", unique)

**Ràng buộc:**
- Mã phòng ban phải duy nhất
- Không thể xóa phòng ban đang có nhân viên

**Ví dụ dữ liệu:**
- Phòng Kỹ thuật (IT)
- Phòng Kinh doanh (SALES)
- Phòng Nhân sự (HR)

---

### Model 1.3: `chuc_vu` (Chức vụ)

**Mục đích:** Quản lý các chức vụ và mức lương tương ứng

**Chức năng:**
- Tạo/sửa/xóa chức vụ
- Định nghĩa lương cơ bản cho từng chức vụ
- Hiển thị số lượng nhân viên có chức vụ
- Tự động gán lương cơ bản cho nhân viên mới

**Các trường quan trọng:**
- `ten_chuc_vu`: Tên chức vụ (VD: "Trưởng phòng")
- `luong_co_ban`: Mức lương cơ bản mặc định

**Ràng buộc:**
- Không thể xóa chức vụ đang có nhân viên

**Ví dụ dữ liệu:**
- Giám đốc: 30,000,000 VNĐ
- Trưởng phòng: 20,000,000 VNĐ
- Nhân viên: 10,000,000 VNĐ

---

## 2️⃣ MODULE CHẤM CÔNG (cham_cong)

### Model 2.1: `dot_dang_ky` (Đợt đăng ký)

**Mục đích:** Quản lý các đợt mở đăng ký ca làm

**Chức năng:**
- Tạo đợt đăng ký mới cho tháng
- Thiết lập thời gian đăng ký (từ ngày - đến ngày)
- Thiết lập thời gian làm việc (từ ngày - đến ngày)
- Mở/đóng đợt đăng ký
- Theo dõi số lượng đăng ký

**Các trường quan trọng:**
- `ten_dot`: Tên đợt (VD: "Đợt đăng ký tháng 2/2026")
- `ngay_bat_dau`: Ngày mở đăng ký
- `ngay_ket_thuc`: Ngày đóng đăng ký
- `ngay_bat_dau_lam_viec`: Ngày bắt đầu làm việc
- `ngay_ket_thuc_lam_viec`: Ngày kết thúc làm việc
- `trang_thai`: Trạng thái (mo/dong)

**Ràng buộc:**
- Ngày kết thúc đăng ký < Ngày bắt đầu làm việc
- Chỉ có 1 đợt "mo" tại 1 thời điểm

---

### Model 2.2: `dang_ky_ca_lam_theo_ngay` (Đăng ký ca làm)

**Mục đích:** Lưu đăng ký ca làm của nhân viên theo từng ngày

**Chức năng:**
- Nhân viên đăng ký ca làm (Sáng/Chiều/Cả ngày)
- Kiểm tra trùng lặp (1 nhân viên chỉ đăng ký 1 ca/ngày)
- Tự động tạo bảng chấm công dựa trên đăng ký
- Xem lịch đã đăng ký

**Các trường quan trọng:**
- `nhan_vien_id`: Nhân viên đăng ký
- `dot_dang_ky_id`: Đợt đăng ký
- `ngay_lam`: Ngày làm việc
- `ca_lam`: Ca làm (Sáng/Chiều/Cả ngày)

**Loại ca làm:**
| Ca | Giờ vào | Giờ ra | Số giờ |
|----|---------|--------|--------|
| Sáng | 07:30 | 11:30 | 4h |
| Chiều | 13:30 | 17:30 | 4h |
| Cả ngày | 07:30 | 17:30 | 8h |

**Ràng buộc:**
- UNIQUE (nhan_vien_id, ngay_lam)

---

### Model 2.3: `bang_cham_cong` (Bảng chấm công) ⭐

**Mục đích:** Ghi nhận chấm công hàng ngày và tính toán tự động

**Chức năng:**
- Ghi nhận giờ vào/ra thực tế của nhân viên
- **Tự động tính giờ vào ca, giờ ra ca** (dựa vào ca làm đã đăng ký)
- **Tự động tính phút đi muộn** = max(0, giờ vào - giờ vào ca)
- **Tự động tính phút về sớm** = max(0, giờ ra ca - giờ ra)
- **Tự động xác định trạng thái** (Đi làm/Đi muộn/Về sớm/Vắng mặt)
- **Điều chỉnh theo đơn từ** (nếu đơn được duyệt)
- Xem lịch sử chấm công theo ngày/tháng

**Các trường quan trọng:**
- `nhan_vien_id`: Nhân viên chấm công
- `ngay_cham_cong`: Ngày chấm công
- `dang_ky_ca_lam_id`: Ca đã đăng ký
- `ca_lam`: Ca làm (related từ đăng ký)
- `gio_vao_ca`: Giờ vào ca chuẩn (computed)
- `gio_ra_ca`: Giờ ra ca chuẩn (computed)
- `gio_vao`: Giờ vào thực tế (nhập tay)
- `gio_ra`: Giờ ra thực tế (nhập tay)
- `phut_di_muon_goc`: Phút đi muộn gốc (computed)
- `phut_di_muon`: Phút đi muộn sau điều chỉnh (computed)
- `phut_ve_som_goc`: Phút về sớm gốc (computed)
- `phut_ve_som`: Phút về sớm sau điều chỉnh (computed)
- `trang_thai`: Trạng thái chấm công (computed)
- `don_tu_id`: Đơn từ liên quan

**Trạng thái:**
- `di_lam`: Đi làm đúng giờ
- `di_muon`: Đi muộn
- `ve_som`: Về sớm
- `di_muon_ve_som`: Cả 2
- `vang_mat`: Vắng mặt
- `vang_mat_co_phep`: Vắng mặt có phép

**Ràng buộc:**
- UNIQUE (nhan_vien_id, ngay_cham_cong)

---

### Model 2.4: `don_tu` (Đơn từ)

**Mục đích:** Quản lý đơn xin phép của nhân viên

**Chức năng:**
- Nhân viên gửi đơn xin đi muộn/về sớm/nghỉ phép
- Quản lý duyệt đơn từ (chờ duyệt/đã duyệt/từ chối)
- **Tự động điều chỉnh chấm công** khi đơn được duyệt
- Lưu lịch sử đơn từ
- Thống kê đơn từ theo nhân viên

**Các trường quan trọng:**
- `nhan_vien_id`: Nhân viên gửi đơn
- `loai_don`: Loại đơn (di_muon/ve_som/nghi_phep)
- `ngay_ap_dung`: Ngày áp dụng đơn
- `thoi_gian_xin`: Thời gian xin (phút)
- `ly_do`: Lý do xin phép
- `trang_thai_duyet`: Trạng thái (cho_duyet/da_duyet/tu_choi)
- `nguoi_duyet_id`: Người duyệt
- `ngay_duyet`: Ngày duyệt

**Loại đơn:**
- `di_muon`: Xin đi muộn → Giảm phút đi muộn
- `ve_som`: Xin về sớm → Giảm phút về sớm
- `nghi_phep`: Xin nghỉ phép → Trạng thái = vắng mặt có phép

**Quy trình:**
```
cho_duyet → da_duyet (Quản lý duyệt)
         ↘ tu_choi (Quản lý từ chối)
```

---

## 3️⃣ MODULE TÍNH LƯƠNG (tinh_luong)

### Model 3.1: `dot_lam_viec` (Đợt làm việc)

**Mục đích:** Quản lý các đợt làm việc theo tháng

**Chức năng:**
- Tạo đợt làm việc cho tháng mới
- Thiết lập thời gian làm việc (từ ngày - đến ngày)
- Mở/đóng đợt làm việc
- Liên kết với bảng lương và ngày trả lương

**Các trường quan trọng:**
- `ten_dot`: Tên đợt (VD: "Tháng 2/2026")
- `ngay_bat_dau`: Ngày bắt đầu (VD: 01/02/2026)
- `ngay_ket_thuc`: Ngày kết thúc (VD: 28/02/2026)
- `thang`: Tháng (1-12)
- `nam`: Năm
- `trang_thai`: Trạng thái (dang_mo/da_dong)

**Ràng buộc:**
- UNIQUE (thang, nam) - Mỗi tháng chỉ có 1 đợt
- Không thể xóa đợt đã có bảng lương

---

### Model 3.2: `tinh_luong.bang_luong` (Bảng lương) ⭐

**Mục đích:** Lưu thông tin lương của nhân viên theo tháng

**Chức năng:**
- Tính lương tự động cho tất cả nhân viên
- Lấy lương cơ bản từ chức vụ
- **Tự động tính tổng trợ cấp** (SUM của các trợ cấp)
- **Tự động tính tổng lương** = Lương cơ bản + Tổng trợ cấp
- Quản lý trạng thái (Nháp/Đã xác nhận/Đã trả)
- In phiếu lương

**Các trường quan trọng:**
- `nhan_vien_id`: Nhân viên
- `dot_lam_viec_id`: Đợt làm việc
- `luong_co_ban`: Lương cơ bản
- `tong_tro_cap`: Tổng trợ cấp (computed)
- `tong_luong`: Tổng lương (computed)
- `thang`: Tháng lương
- `state`: Trạng thái (draft/confirmed/paid)
- `ghi_chu`: Ghi chú

**Công thức:**
```
Tổng lương = Lương cơ bản + Tổng trợ cấp
```

**Workflow:**
```
draft (Nháp) → confirmed (Đã xác nhận) → paid (Đã trả)
```

**Ràng buộc:**
- UNIQUE (nhan_vien_id, dot_lam_viec_id)

---

### Model 3.3: `tinh_luong.tro_cap` (Trợ cấp)

**Mục đích:** Quản lý các khoản trợ cấp của nhân viên

**Chức năng:**
- Thêm/sửa/xóa trợ cấp cho bảng lương
- Hỗ trợ 5 loại trợ cấp phổ biến
- Tự động cộng vào tổng trợ cấp
- Thống kê trợ cấp theo loại

**Các trường quan trọng:**
- `bang_luong_id`: Bảng lương
- `loai`: Loại trợ cấp
- `so_tien`: Số tiền (VNĐ)
- `mo_ta`: Mô tả chi tiết

**5 Loại trợ cấp:**

| Loại | Tên | Số tiền mặc định | Cách tính |
|------|-----|------------------|-----------|
| `an_trua` | Tiền ăn trưa | 30,000 VNĐ/ngày | 30,000 × số ngày làm việc |
| `xang_xe` | Tiền xăng xe | 500,000 VNĐ/tháng | Cố định |
| `dien_thoai` | Tiền điện thoại | 200,000 VNĐ/tháng | Cố định |
| `nha_o` | Tiền nhà ở | 1,000,000 VNĐ/tháng | Tùy chọn |
| `khac` | Trợ cấp khác | Tùy chỉnh | Tùy chỉnh |

**Ví dụ:**
```
Tiền ăn trưa: 30,000 × 20 ngày = 600,000 VNĐ
Tiền xăng xe: 500,000 VNĐ
Tiền điện thoại: 200,000 VNĐ
→ Tổng trợ cấp: 1,300,000 VNĐ
```

---

### Model 3.4: `tinh_luong.ngay_tra_luong` (Ngày trả lương)

**Mục đích:** Quản lý ngày trả lương và tích hợp Google Calendar

**Chức năng:**
- Thiết lập ngày trả lương cho đợt làm việc
- **Tích hợp Google Calendar API** (tạo event tự động)
- Lưu link event Google Calendar
- Theo dõi trạng thái đồng bộ
- Nhắc nhở tự động qua email

**Các trường quan trọng:**
- `ten_dot_chi_tra`: Tên đợt chi trả
- `dot_lam_viec_id`: Đợt làm việc
- `ngay_tra`: Ngày trả lương
- `sync_calendar_status`: Trạng thái đồng bộ (not_synced/synced/error)
- `google_event_id`: Google Event ID
- `google_event_link`: Link event trên Google Calendar

**Ràng buộc:**
- Ngày trả >= Ngày kết thúc đợt làm việc
- UNIQUE (dot_lam_viec_id) - Mỗi đợt chỉ có 1 ngày trả

**Tích hợp Google Calendar:**
- Event title: "💰 Trả lương - Tháng X/YYYY"
- Reminder email: 1 ngày trước
- Reminder popup: 1 giờ trước

---

## 4️⃣ MODULE DASHBOARD (hr_dashboard)

### Model 4.1: Không có model riêng

**Mục đích:** Hiển thị biểu đồ và thống kê từ các module khác

**Chức năng:**
- Sử dụng dữ liệu từ `nhan_vien`, `bang_cham_cong`, `bang_luong`, `tro_cap`
- Tạo graph views (biểu đồ cột, tròn, đường)
- Tạo pivot views (bảng phân tích)
- Menu tổng hợp truy cập nhanh

**Các biểu đồ:**

**Nhân sự:**
- Biểu đồ cột: Số nhân viên theo phòng ban
- Biểu đồ tròn: Phân bổ theo chức vụ

**Chấm công:**
- Biểu đồ tròn: Trạng thái chấm công
- Biểu đồ cột: Phút đi muộn theo nhân viên
- Biểu đồ cột: Số lần chấm công

**Lương:**
- Biểu đồ cột: Tổng lương theo nhân viên
- Biểu đồ cột xếp chồng: Cấu trúc lương (Lương CB vs Trợ cấp)
- Biểu đồ tròn: Phân bổ trợ cấp theo loại

---

## 5️⃣ MODULE GOOGLE CALENDAR INTEGRATION

### Model 5.1: `google.calendar.config` (Cấu hình Google Calendar)

**Mục đích:** Lưu cấu hình kết nối Google Calendar API

**Chức năng:**
- Lưu thông tin Service Account JSON
- Lưu Calendar ID
- Kiểm tra kết nối
- Quản lý nhiều cấu hình

**Các trường quan trọng:**
- `name`: Tên cấu hình
- `calendar_id`: Calendar ID (email)
- `service_account_json`: Nội dung JSON file (Service Account)
- `active`: Kích hoạt

**Hướng dẫn:**
1. Tạo Service Account trên Google Cloud Console
2. Download JSON key
3. Paste nội dung JSON vào trường `service_account_json`
4. Nhập Calendar ID (email của bạn)
5. Share Calendar với email của Service Account

---

### Model 5.2: Mở rộng `tinh_luong.ngay_tra_luong`

**Chức năng bổ sung:**
- Method `action_sync_to_google_calendar_api()`: Đồng bộ với Google Calendar
- Method `action_open_google_event()`: Mở event trên Google Calendar
- Lưu trạng thái đồng bộ
- Xử lý lỗi khi đồng bộ

**Quy trình đồng bộ:**
```
1. Đọc cấu hình Google Calendar
2. Parse Service Account JSON
3. Tạo Google API client
4. Gọi Calendar API: events.insert
5. Lưu Event ID và Link
6. Cập nhật trạng thái: synced
```

---

## 📊 TỔNG HỢP

### Bảng tổng hợp Models

| Module | Số Models | Models chính |
|--------|-----------|--------------|
| **QLNS** | 3 | nhan_vien, phong_ban, chuc_vu |
| **Chấm công** | 4 | bang_cham_cong, dang_ky_ca_lam, don_tu, dot_dang_ky |
| **Tính lương** | 4 | bang_luong, tro_cap, ngay_tra_luong, dot_lam_viec |
| **Dashboard** | 0 | (Sử dụng views) |
| **Google Calendar** | 1 | google.calendar.config |
| **TỔNG** | **12** | **models** |

---

### Models có Computed Fields (Tự động tính)

| Model | Computed Fields | Mô tả |
|-------|-----------------|-------|
| `bang_cham_cong` | gio_vao_ca, gio_ra_ca | Tính theo ca làm |
| `bang_cham_cong` | phut_di_muon, phut_ve_som | Tính theo giờ vào/ra |
| `bang_cham_cong` | trang_thai | Dựa vào phút đi muộn/về sớm |
| `bang_luong` | tong_tro_cap | SUM(tro_cap.so_tien) |
| `bang_luong` | tong_luong | luong_co_ban + tong_tro_cap |

---

### Models có Workflow (State Machine)

| Model | States | Workflow |
|-------|--------|----------|
| `bang_luong` | draft → confirmed → paid | Nháp → Xác nhận → Đã trả |
| `don_tu` | cho_duyet → da_duyet/tu_choi | Chờ → Duyệt/Từ chối |
| `dot_dang_ky` | mo → dong | Mở → Đóng |
| `dot_lam_viec` | dang_mo → da_dong | Đang mở → Đã đóng |

---

### Models có tích hợp External API

| Model | API | Chức năng |
|-------|-----|-----------|
| `ngay_tra_luong` | Google Calendar API v3 | Tạo event tự động |

---

## 🎯 TÍNH NĂNG NỔI BẬT

### ⭐ Tự động hóa
- ✅ `bang_cham_cong`: Tự động tính đi muộn, về sớm, trạng thái
- ✅ `bang_luong`: Tự động tính tổng trợ cấp, tổng lương
- ✅ `don_tu`: Tự động điều chỉnh chấm công khi duyệt

### ⭐ Tích hợp bên ngoài
- ✅ `ngay_tra_luong`: Tích hợp Google Calendar API
- ✅ Tạo event tự động
- ✅ Nhắc nhở email & popup

### ⭐ Computed Fields
- ✅ 5 computed fields trong `bang_cham_cong`
- ✅ 2 computed fields trong `bang_luong`
- ✅ Tự động cập nhật khi dữ liệu thay đổi

---

**Tạo bởi:** Hệ thống Quản lý Nhân sự  
**Ngày:** 02/02/2026  
**Phiên bản:** 1.0
