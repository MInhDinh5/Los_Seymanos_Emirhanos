# Hướng dẫn cài đặt & chạy dự án (SETUP)

Dự án là ứng dụng web ASP.NET Core MVC cho quán cà phê, có 2 phía:

- **Khách hàng**: xem menu, đặt món (`OrderController`)
- **Barista**: đăng nhập, quản lý đơn hàng & sản phẩm (`BaristaController`)

## 1. Yêu cầu môi trường

| Thành phần | Phiên bản |
|---|---|
| .NET SDK | **.NET 7 SDK** (mặc định của dự án — xem `CafeApp/CafeApp.csproj`, `TargetFramework: net7.0`) |
| IDE (tuỳ chọn) | Visual Studio 2022 / VS Code / Rider |
| Hệ điều hành | Windows / macOS / Linux |

> ℹ️ Lưu ý: .NET 7 đã hết hỗ trợ chính thức (EOL). Nếu máy bạn cài SDK mới hơn (VD: .NET 8/9/10), dự án vẫn build và chạy tốt nhờ khả năng `rollForward` của SDK — chỉ hiện warning `NETSDK1138`. Nếu bạn muốn, có thể nâng `TargetFramework` lên `net8.0` (xem Issue liên quan).

Kiểm tra SDK đã cài:

```bash
dotnet --version
```

## 2. Lấy mã nguồn

```bash
# Repo nhóm
git clone https://github.com/cafe-nhom-02/Los_Seymanos_Emirhanos.git
cd Los_Seymanos_Emirhanos

# (Nếu bạn là contributor) Thiết lập fork + upstream
git remote add origin https://github.com/<tai-khoan>/Los_Seymanos_Emirhanos.git
git remote add upstream https://github.com/cafe-nhom-02/Los_Seymanos_Emirhanos.git
```

## 3. Cơ sở dữ liệu

Dự án dùng **SQLite**, không cần cài gì thêm:

- File DB: `CafeApp/CafeDB.db`
- Chuỗi kết nối: xem `CafeApp/appsettings.json` (`ConnectionStrings:database`)
- Khi chạy app, `DatabaseInitializer` (`CafeApp/Data/InitialBarista.cs`) sẽ **tự động**:
  1. Chạy migration để tạo/quản lý schema
  2. Seed tài khoản barista mặc định nếu bảng `Baristas` rỗng

## 4. Chạy ứng dụng

```bash
dotnet run --project CafeApp
```

Hoặc bằng Visual Studio: mở `CafeApp.sln` → chọn profile **http** → F5.

Sau khi chạy, mở trình duyệt tại địa chỉ in ra console (mặc định):

- Trang khách: `http://localhost:5247/`
- Trang barista: `http://localhost:5247/Barista/Login`

## 5. Tài khoản đăng nhập (seed sẵn)

| Tài khoản | Mật khẩu |
|---|---|
| `admin@gmail.com` | `admin` |

## 6. Sự cố thường gặp (Troubleshooting)

| Vấn đề | Nguyên nhân | Cách xử lý |
|---|---|---|
| `Address already in use` / Kestrel không bind được port | Lần chạy trước chưa tắt, port 5247 đang bị chiếm | Tắt process cũ (`CafeApp.exe`) hoặc chạy trên port khác: `dotnet run --project CafeApp --urls http://localhost:5300` |
| Warning `NETSDK1138: net7.0 out of support` | Dùng SDK mới hơn .NET 7 | Chỉ là warning, không ảnh hưởng chạy |
| DB không cập nhật đúng như mong đợi | Đã có `CafeDB.db` cũ từ lần chạy trước | Xoá file `CafeApp/CafeDB.db` (+ `.db-shm`, `.db-wal`) rồi chạy lại — app sẽ migrate + seed lại |
| Build không thấy `CafeDB.db` | File DB bị `.gitignore` (đúng chuẩn) | App tự tạo lại khi chạy lần đầu |

## 7. Cấu trúc thư mục nhanh

```
CafeApp/
├── Controllers/      # OrderController, BaristaController, ProductController, HomeController
├── Data/             # DataContext (EF Core) + DatabaseInitializer (seed/migrate)
├── Models/           # Order, OrderProduct, Product, Barista
├── Migrations/       # Migration EF Core
├── Views/            # Razor views (Barista/, Order/, Product/, Home/, Shared/)
└── wwwroot/          # css, js, images
```
