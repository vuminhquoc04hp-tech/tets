# 📐 SƠ ĐỒ KIẾN TRÚC HỆ THỐNG - MERMAID DIAGRAMS

## 1. 🏗️ Sơ đồ kiến trúc tổng thể (Architecture Diagram)

```mermaid
graph TB
    subgraph "Presentation Layer - Giao diện người dùng"
        A1[Web Browser<br/>Chrome/Firefox]
        A2[Mobile Browser]
    end
    
    subgraph "Odoo Framework 15.0"
        B1[Web Controllers]
        B2[ORM]
        B3[Security]
        B4[Workflow Engine]
    end
    
    subgraph "Custom Modules - Tầng nghiệp vụ"
        C1[📗 QLNS<br/>Nhân sự<br/>- Nhân viên<br/>- Phòng ban<br/>- Chức vụ]
        C2[📙 Chấm công<br/>- Bảng chấm công<br/>- Đăng ký ca làm<br/>- Đơn từ]
        C3[📘 Tính lương<br/>- Bảng lương<br/>- Trợ cấp<br/>- Ngày trả lương]
        C4[📊 Dashboard<br/>- Biểu đồ<br/>- Thống kê<br/>- KPI]
    end
    
    subgraph "Integration Layer"
        D1[🔗 Google Calendar<br/>Integration<br/>- Service Account<br/>- Calendar API]
    end
    
    subgraph "External Services"
        E1[📅 Google Calendar]
        E2[📧 Gmail API]
    end
    
    subgraph "Database Layer"
        F1[(PostgreSQL<br/>- nhan_vien<br/>- bang_cham_cong<br/>- bang_luong<br/>- etc.)]
    end
    
    A1 & A2 --> B1
    B1 --> B2
    B2 --> B3
    B3 --> B4
    B4 --> C1 & C2 & C3 & C4
    C3 --> D1
    D1 --> E1 & E2
    C1 & C2 & C3 & C4 --> F1
    
    style C1 fill:#90EE90
    style C2 fill:#FFB347
    style C3 fill:#87CEEB
    style C4 fill:#DDA0DD
    style D1 fill:#FFE4B5
    style E1 fill:#FFD700
    style F1 fill:#B0C4DE
```

---

## 2. 🔄 Sơ đồ luồng dữ liệu (Data Flow Diagram)

```mermaid
flowchart LR
    subgraph Input
        A[👤 Nhân viên]
    end
    
    subgraph "Module Chấm công"
        B[Ghi nhận giờ vào/ra]
        C[Tính đi muộn/về sớm]
        D[Xử lý đơn từ]
    end
    
    subgraph "Module Tính lương"
        E[Tính lương cơ bản]
        F[Cộng trợ cấp]
        G[Tạo bảng lương]
        H[Tạo ngày trả lương]
    end
    
    subgraph "Google Calendar Integration"
        I[Đồng bộ API]
        J[Tạo Event]
    end
    
    subgraph Output
        K[📅 Google Calendar]
        L[💰 Phiếu lương]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    G --> L
    
    style A fill:#90EE90
    style K fill:#FFD700
    style L fill:#87CEEB
```

---

## 3. 📊 Sơ đồ quan hệ Module (Module Relationship)

```mermaid
graph TD
    A[Odoo Framework]
    
    B[QLNS<br/>Nhân sự]
    C[Chấm công]
    D[Tính lương]
    E[Dashboard]
    F[Google Calendar<br/>Integration]
    
    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    
    B -->|Cung cấp<br/>thông tin NV| C
    B -->|Cung cấp<br/>thông tin NV| D
    C -->|Dữ liệu<br/>chấm công| D
    D -->|Dữ liệu<br/>lương| E
    C -->|Dữ liệu<br/>chấm công| E
    B -->|Dữ liệu<br/>nhân sự| E
    D -->|Ngày trả lương| F
    
    style A fill:#E6E6FA
    style B fill:#90EE90
    style C fill:#FFB347
    style D fill:#87CEEB
    style E fill:#DDA0DD
    style F fill:#FFE4B5
```

---

## 4. 🗄️ Sơ đồ cơ sở dữ liệu (ERD - Entity Relationship Diagram)

```mermaid
erDiagram
    NHAN_VIEN ||--o{ BANG_CHAM_CONG : "có"
    NHAN_VIEN ||--o{ BANG_LUONG : "có"
    NHAN_VIEN }o--|| PHONG_BAN : "thuộc"
    NHAN_VIEN }o--|| CHUC_VU : "có"
    
    BANG_CHAM_CONG }o--|| DANG_KY_CA_LAM : "dựa trên"
    BANG_CHAM_CONG }o--o| DON_TU : "liên quan"
    
    BANG_LUONG ||--o{ TRO_CAP : "có"
    BANG_LUONG }o--|| DOT_LAM_VIEC : "thuộc"
    
    NGAY_TRA_LUONG }o--|| DOT_LAM_VIEC : "cho"
    NGAY_TRA_LUONG ||--o| GOOGLE_EVENT : "tạo"
    
    NHAN_VIEN {
        int id PK
        string ho_va_ten
        string email
        date ngay_sinh
        int phong_ban_id FK
        int chuc_vu_id FK
    }
    
    PHONG_BAN {
        int id PK
        string ten_phong_ban
        string ma_phong_ban
    }
    
    CHUC_VU {
        int id PK
        string ten_chuc_vu
        float luong_co_ban
    }
    
    BANG_CHAM_CONG {
        int id PK
        int nhan_vien_id FK
        date ngay_cham_cong
        datetime gio_vao
        datetime gio_ra
        float phut_di_muon
        float phut_ve_som
        string trang_thai
    }
    
    BANG_LUONG {
        int id PK
        int nhan_vien_id FK
        float luong_co_ban
        float tong_tro_cap
        float tong_luong
        date thang
    }
    
    TRO_CAP {
        int id PK
        int bang_luong_id FK
        string loai
        float so_tien
    }
    
    NGAY_TRA_LUONG {
        int id PK
        string ten_dot_chi_tra
        date ngay_tra
        int dot_lam_viec_id FK
        string google_event_id
    }
```

---

## 5. 🔐 Sơ đồ bảo mật (Security Architecture)

```mermaid
graph TB
    subgraph "User Layer"
        A[👤 User Login]
    end
    
    subgraph "Authentication"
        B[Username/Password]
        C[Session Token]
    end
    
    subgraph "Authorization"
        D[Role-Based Access Control]
        E[Record Rules]
        F[Field-Level Security]
    end
    
    subgraph "Data Access"
        G[ORM Layer<br/>SQL Injection Prevention]
        H[XSS Protection]
        I[CSRF Tokens]
    end
    
    subgraph "External API Security"
        J[Google OAuth 2.0]
        K[Service Account]
        L[API Key Management]
    end
    
    subgraph "Database"
        M[(Encrypted Data)]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> M
    
    D --> J
    J --> K
    K --> L
    
    style A fill:#FFB6C1
    style M fill:#B0C4DE
    style J fill:#FFD700
```

---

## 6. 🚀 Sơ đồ triển khai (Deployment Diagram)

```mermaid
graph TB
    subgraph "Client Side"
        A[Web Browser]
        B[Mobile Browser]
    end
    
    subgraph "Server - localhost:8069"
        C[Odoo Server<br/>Python 3.10]
        D[Web Server<br/>Werkzeug]
    end
    
    subgraph "Application"
        E[Custom Modules<br/>QLNS, Chấm công,<br/>Tính lương, Dashboard]
    end
    
    subgraph "Database Server - localhost:5431"
        F[(PostgreSQL 12+)]
    end
    
    subgraph "External Services"
        G[Google Calendar API<br/>calendar.google.com]
    end
    
    subgraph "Development Environment"
        H[WSL2 Ubuntu 22.04]
        I[Virtual Environment<br/>venv]
    end
    
    A & B -->|HTTP/HTTPS| D
    D --> C
    C --> E
    E -->|ORM| F
    E -->|REST API| G
    C --> I
    I --> H
    
    style A fill:#90EE90
    style F fill:#B0C4DE
    style G fill:#FFD700
    style H fill:#E6E6FA
```

---

## 7. 📈 Sơ đồ Use Case (Use Case Diagram)

```mermaid
graph LR
    subgraph "Actors"
        A[👤 Nhân viên]
        B[👨‍💼 Quản lý]
        C[💻 Admin]
    end
    
    subgraph "Use Cases - QLNS"
        D[Xem thông tin cá nhân]
        E[Quản lý nhân viên]
        F[Quản lý phòng ban]
    end
    
    subgraph "Use Cases - Chấm công"
        G[Chấm công]
        H[Đăng ký ca làm]
        I[Gửi đơn từ]
        J[Duyệt đơn từ]
    end
    
    subgraph "Use Cases - Tính lương"
        K[Xem phiếu lương]
        L[Tính lương]
        M[Quản lý trợ cấp]
        N[Đồng bộ Google Calendar]
    end
    
    subgraph "Use Cases - Dashboard"
        O[Xem biểu đồ]
        P[Xuất báo cáo]
    end
    
    A --> D
    A --> G
    A --> H
    A --> I
    A --> K
    A --> O
    
    B --> E
    B --> J
    B --> L
    B --> M
    B --> N
    B --> O
    B --> P
    
    C --> E
    C --> F
    C --> L
    C --> M
    C --> N
    C --> P
    
    style A fill:#90EE90
    style B fill:#FFB347
    style C fill:#87CEEB
```

---

## 8. ⚙️ Sơ đồ quy trình nghiệp vụ (Business Process Flow)

```mermaid
sequenceDiagram
    participant NV as 👤 Nhân viên
    participant CC as Chấm công
    participant TL as Tính lương
    participant GC as Google Calendar
    participant QL as 👨‍💼 Quản lý
    
    NV->>CC: 1. Chấm công hàng ngày
    CC->>CC: 2. Tính đi muộn/về sớm
    
    alt Có đơn từ
        NV->>CC: 3a. Gửi đơn từ
        QL->>CC: 3b. Duyệt đơn từ
        CC->>CC: 3c. Điều chỉnh chấm công
    end
    
    Note over CC,TL: Cuối tháng
    
    TL->>CC: 4. Lấy dữ liệu chấm công
    TL->>TL: 5. Tính lương + trợ cấp
    TL->>NV: 6. Tạo phiếu lương
    
    QL->>TL: 7. Tạo ngày trả lương
    TL->>GC: 8. Đồng bộ Google Calendar
    GC->>QL: 9. Tạo event & nhắc nhở
    
    Note over NV,QL: Ngày trả lương
    QL->>NV: 10. Trả lương
```

---

## 9. 🔗 Sơ đồ tích hợp Google Calendar (Integration Flow)

```mermaid
flowchart TD
    A[Bắt đầu] --> B[Tạo Ngày trả lương<br/>trong Odoo]
    B --> C[Click button<br/>'Đồng bộ Google Calendar API']
    C --> D{Đã cấu hình<br/>Service Account?}
    
    D -->|Không| E[Hiển thị lỗi:<br/>Chưa cấu hình]
    E --> F[Vào Settings<br/>→ Google Calendar Config]
    F --> G[Paste Service Account JSON]
    G --> H[Nhập Calendar ID]
    H --> C
    
    D -->|Có| I[Đọc cấu hình]
    I --> J[Parse JSON credentials]
    J --> K[Tạo Google API client]
    K --> L[Gọi Calendar API:<br/>events.insert]
    
    L --> M{API call<br/>thành công?}
    
    M -->|Không| N[Hiển thị lỗi]
    N --> O[Log error details]
    O --> P[Kết thúc]
    
    M -->|Có| Q[Lưu Event ID<br/>vào Odoo]
    Q --> R[Lưu Event Link]
    R --> S[Hiển thị thông báo<br/>thành công]
    S --> T[User click<br/>'Xem trên Google Calendar']
    T --> U[Mở link event<br/>trên browser]
    U --> P
    
    style A fill:#90EE90
    style E fill:#FFB347
    style N fill:#FF6B6B
    style S fill:#87CEEB
    style P fill:#DDA0DD
```

---

## 10. 📊 Sơ đồ Dashboard Architecture

```mermaid
graph TB
    subgraph "User Interface"
        A[Dashboard Menu]
    end
    
    subgraph "Dashboard Views"
        B1[📈 Biểu đồ nhân viên]
        B2[📈 Biểu đồ chấm công]
        B3[📈 Biểu đồ lương]
        B4[📈 Biểu đồ trợ cấp]
    end
    
    subgraph "Graph Types"
        C1[Bar Chart<br/>Theo phòng ban]
        C2[Pie Chart<br/>Theo chức vụ]
        C3[Line Chart<br/>Xu hướng tháng]
        C4[Pivot Table<br/>Phân tích chi tiết]
    end
    
    subgraph "Data Sources"
        D1[(nhan_vien)]
        D2[(bang_cham_cong)]
        D3[(bang_luong)]
        D4[(tro_cap)]
    end
    
    A --> B1 & B2 & B3 & B4
    
    B1 --> C1 & C2
    B2 --> C1 & C2 & C3
    B3 --> C1 & C3 & C4
    B4 --> C2 & C4
    
    C1 & C2 --> D1
    C1 & C2 & C3 --> D2
    C1 & C3 & C4 --> D3
    C2 & C4 --> D4
    
    style A fill:#DDA0DD
    style D1 fill:#90EE90
    style D2 fill:#FFB347
    style D3 fill:#87CEEB
    style D4 fill:#FFD700
```

---

## 📝 Cách sử dụng

### Xem trên GitHub/GitLab:
- Các sơ đồ Mermaid sẽ tự động render

### Xem trên VS Code:
1. Cài extension: "Markdown Preview Mermaid Support"
2. Mở file này
3. Nhấn `Ctrl+Shift+V` để preview

### Xuất ra hình ảnh:
- Sử dụng https://mermaid.live
- Copy code Mermaid
- Export PNG/SVG

### Chỉnh sửa:
- Thay đổi text trong `[]` hoặc `{}`
- Thêm/bớt node bằng cách thêm/xóa dòng
- Thay đổi màu: `style NodeName fill:#COLOR`

---

**Tạo bởi:** Hệ thống Quản lý Nhân sự  
**Ngày:** 02/02/2026  
**Công cụ:** Mermaid.js
