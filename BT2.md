# Bài kiểm tra số 2

Họ và tên: Bùi Xuân Dũng

Lớp: K59KMT.K01

MSSV: K23548016009

Đề tài: Quản lí quán cafe
# Phần 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu
1.1 Khởi tạo bảng
<img width="1920" height="1080" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/bab97028-5a7e-4ed4-a175-d7b97847e228" />
_Khởi tạo database QuanLyQuanCafe_K235480106009_

1.2 Tạo 3 bảng có quan hệ với nhau

- Bảng Loại Sản Phẩm
```
CREATE TABLE [LoaiSanPham] (
    [MaLoai] INT PRIMARY KEY IDENTITY(1,1), -- PK: Khóa chính tự tăng
    [TenLoai] NVARCHAR(100) NOT NULL
);
```
<img width="1920" height="1080" alt="Screenshot (43)" src="https://github.com/user-attachments/assets/318f5b26-7863-4660-8c20-0167951661e7" />

_Tạo bảng [LoaiSanPham]_
- Bảng Sản Phẩm
```
CREATE TABLE [SanPham] (
    [MaSanPham] INT PRIMARY KEY IDENTITY(1,1),
    [TenSanPham] NVARCHAR(200) NOT NULL,
    [GiaBan] MONEY DEFAULT 0,
    [MaLoai] INT,                            -- FK: Khóa ngoại liên kết LoaiSanPham
    [TrangThai] BIT DEFAULT 1,
    
    CONSTRAINT [FK_SanPham_Loai] FOREIGN KEY ([MaLoai]) REFERENCES [LoaiSanPham]([MaLoai]),
    CONSTRAINT [CK_GiaBan] CHECK ([GiaBan] >= 0) -- CK: Ràng buộc giá không âm
);
```
<img width="1920" height="1080" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/8b66fff1-b276-4a04-ab87-d467c6155fbb" />

_Tạo bảng [SanPham]_
- Bảng Hóa Đơn Chi Tiết
```
CREATE TABLE [HoaDonChiTiet] (
    [MaHoaDon] INT,
    [MaSanPham] INT,
    [SoLuong] INT NOT NULL,
    [NgayBan] DATETIME DEFAULT GETDATE(),
    
    PRIMARY KEY ([MaHoaDon], [MaSanPham]),    -- PK phức hợp
    CONSTRAINT [FK_HDCT_SanPham] FOREIGN KEY ([MaSanPham]) REFERENCES [SanPham]([MaSanPham]),
    CONSTRAINT [CK_SoLuong] CHECK ([SoLuong] > 0) -- CK: Số lượng phải lớn hơn 0
);
```
<img width="1920" height="1080" alt="Screenshot (45)" src="https://github.com/user-attachments/assets/f2f8aa6c-c690-454d-adf0-95a01ec52389" />

_Tạo bảng [HoaDonChiTiet]_
- Chèn dữ liệu vào bảng
```
INSERT INTO [LoaiSanPham] ([TenLoai])
VALUES 
(N'Cà phê'),
(N'Trà trái cây'),
(N'Đồ ăn vặt'),
(N'Sinh tố'),
(N'Soda'),
(N'Bánh ngọt'),
(N'Nước ép nguyên chất');

INSERT INTO [SanPham] ([TenSanPham], [GiaBan], [MaLoai], [TrangThai])
VALUES 
-- Nhóm Cà phê (MaLoai 1)
(N'Americano', 30000, 1, 1),
(N'Cappuccino', 45000, 1, 1),
(N'Latte Macchiato', 48000, 1, 1),
(N'Cà phê Trứng', 55000, 1, 1),
-- Nhóm Trà (MaLoai 2)
(N'Trà Vải', 40000, 2, 1),
(N'Trà Sen Vàng', 45000, 2, 1),
(N'Trà Nhài', 38000, 2, 1),
-- Nhóm Đồ ăn (MaLoai 3 & 6)
(N'Khô gà lá chanh', 25000, 3, 1),
(N'Bánh Tiramisu', 42000, 6, 1),
(N'Bánh Cheesecake Chanh Dây', 45000, 6, 1),
(N'Bánh Hạnh Nhân', 35000, 6, 1),
-- Nhóm Nước ép & Sinh tố (MaLoai 4 & 7)
(N'Nước ép Cam Cà rốt', 40000, 7, 1),
(N'Nước ép Dưa hấu', 35000, 7, 1),
(N'Sinh tố Xoài', 50000, 4, 1),
(N'Sinh tố Việt quất', 60000, 4, 1),
-- Nhóm Soda (MaLoai 5)
(N'Soda Chanh', 35000, 5, 1),
(N'Soda Bạc hà', 42000, 5, 1);

INSERT INTO [HoaDonChiTiet] ([MaHoaDon], [MaSanPham], [SoLuong], [NgayBan])
VALUES 
(201, 1, 3, '2026-01-10 08:00'),
(201, 8, 2, '2026-01-10 08:00'),
(202, 5, 1, '2026-01-12 14:30'),
(203, 10, 4, '2026-02-15 19:00'),
(204, 2, 2, '2026-03-01 10:00'),
(205, 15, 1, '2026-03-05 15:00'),
(206, 4, 2, '2026-04-10 09:00'),
(207, 12, 3, '2026-04-20 16:20'),
(208, 17, 5, GETDATE()),
(209, 3, 2, GETDATE()),
(210, 9, 1, GETDATE());
```
<img width="1920" height="1080" alt="Screenshot (46)" src="https://github.com/user-attachments/assets/dda505b7-51c0-4941-8407-575a9200c4d7" />

_Chèn dữ liệu vào các bảng_

# Phần 2: Xây dựng Function


Built-in Functions: Là các hàm hệ thống có sẵn như GETDATE(), LEN(), UPPER(), ROUND(). Một hàm đặc sắc là COALESCE(@val1, @val2, ...) - trả về giá trị non-null đầu tiên, cực kỳ hữu dụng khi xử lý dữ liệu thiếu. Một số hàm hệ thống đặc sắc theo góc nhìn của em là

- `ISNULL()`: Kiểm tra nếu giá trị là NULL thì thay thế bằng một giá trị khác
```
SELECT 
    [TenSanPham], 
    ISNULL([GiaBan], 0) AS [GiaHienThi]
FROM [SanPham];
```
<img width="1920" height="1080" alt="Screenshot (51)" src="https://github.com/user-attachments/assets/e2444665-f13c-4ce3-828c-f374569641c9" />

_Em đã khai thác hàm thành công_

- `GETDATE()`: Lấy ngày giờ hiện tại của hệ thống
```
SELECT  
    GETDATE() AS [ThoiDiemHienTai]
```
<img width="1920" height="1080" alt="Screenshot (52)" src="https://github.com/user-attachments/assets/75cf7e4b-c8ca-45bc-b4e5-ffd4798d2384" />

 _Em đã lấy ngày giờ hiện tại của hệ thông_

 - `RANK()`: Xếp hạng các sản phẩm theo giá bán từ cao xuống thấp
```
SELECT 
    [TenSanPham], 
    [GiaBan],
    RANK() OVER (ORDER BY [GiaBan] DESC) AS [XepHangGia]
FROM [SanPham];
```
<img width="1920" height="1080" alt="Screenshot (53)" src="https://github.com/user-attachments/assets/21635479-8164-4e0a-9f8f-f208cc903d37" />

_Giá các sản phẩm được sắp xếp theo thứ tự_















