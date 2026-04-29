# Bài kiểm tra số 2

Họ và tên: Bùi Xuân Dũng

Lớp: K59KMT.K01

MSSV: K23548016009

Đề tài: Quản lí quán cafe
# Phần 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu
## 1.1. Khởi tạo bảng
<img width="1920" height="1080" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/bab97028-5a7e-4ed4-a175-d7b97847e228" />
_Khởi tạo database QuanLyQuanCafe_K235480106009_

## 1.2. Tạo 3 bảng có quan hệ với nhau

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

## 2.1. Các loại hàm hệ thống

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

## 2.2. Hàm Do Người Dùng Tự Viết 
Hàm do người dùng tự viết trong SQL (User Defined Function - UDF) dùng để xử lý các yêu cầu riêng mà hàm có sẵn không đáp ứng được, giúp tái sử dụng code và dễ quản lý.

Các loại hàm tự viết:

**Scalar Function** 

•	Trả về 1 giá trị duy nhất. 

•	Dùng khi cần tính toán hoặc xử lý dữ liệu đơn.

**Inline Table-Valued Function**

•	Trả về 1 bảng dữ liệu từ một câu lệnh SELECT. 

•	Dùng khi cần lọc hoặc lấy danh sách dữ liệu.

**Multi-Statement Table-Valued Function**

•	Trả về bảng dữ liệu sau nhiều bước xử lý.

•	Dùng khi logic phức tạp. 

**Vì sao cần tự viết hàm riêng?**

Vì system function chỉ là hàm chung, không phù hợp mọi bài toán thực tế. Mỗi doanh nghiệp, trường học, cửa hàng... có quy tắc xử lý riêng nên cần tạo hàm riêng để đáp ứng nhu cầu đó.

### 2.2.1. Viết 01 Scalar Function
- Viết một hàm nhận vào số tiền và % giảm giá, trả về số tiền thực tế khách phải trả sau khi đã giảm
```
CREATE FUNCTION [ApDungGiamGia](@GiaGoc MONEY, @PhanTramGiam INT)
RETURNS MONEY
AS
BEGIN
    DECLARE @SoTienGiam MONEY;
    SET @SoTienGiam = (@GiaGoc * @PhanTramGiam) / 100;
    RETURN @GiaGoc - @SoTienGiam;
END;
GO

SELECT 
    [TenSanPham], 
    [GiaBan] AS [GiaGoc], 
    [dbo].[ApDungGiamGia]([GiaBan], 20) AS [GiaSauKhuyenMai]
FROM [SanPham];
```
<img width="1920" height="1080" alt="Screenshot (47)" src="https://github.com/user-attachments/assets/264a3a4a-1550-4428-ab24-b6e290094712" />

_Giảm giá 20% cho tất cả hóa đơn_

### 2.2.2 Viết 01 Inline Table-Valued Function

- Trả về danh sách chi tiết các mặt hàng đã bán trong một khoảng ngày cụ thể
```
CREATE FUNCTION [TimHoaDonTheoNgay](@TuNgay DATETIME, @DenNgay DATETIME)
RETURNS TABLE
AS
RETURN (
    SELECT 
        [MaHoaDon], 
        [MaSanPham], 
        [SoLuong], 
        [NgayBan]
    FROM [HoaDonChiTiet]
    WHERE [NgayBan] BETWEEN @TuNgay AND @DenNgay
);
GO

SELECT * FROM [dbo].[TimHoaDonTheoNgay]('2026-04-01', '2026-04-30');
```
<img width="1920" height="1080" alt="Screenshot (49)" src="https://github.com/user-attachments/assets/a90771fa-21b4-4428-87c1-5c771d232d04" />

_Danh sách các sản phẩm đã bán trong 1 tháng_

### 2.2.3 Viết 01 Multi-statement Table-Valued Function

- Thống kê doanh thu sản phẩm
```
CREATE FUNCTION [ThongKeDoanhThu]()
RETURNS @ThongKeTable TABLE (TenSP NVARCHAR(200), TongTien MONEY)
AS
BEGIN
    INSERT INTO @ThongKeTable
    SELECT s.[TenSanPham], SUM(h.[SoLuong] * s.[GiaBan])
    FROM [SanPham] s JOIN [HoaDonChiTiet] h ON s.[MaSanPham] = h.[MaSanPham]
    GROUP BY s.[TenSanPham];
    RETURN;
END;
GO

SELECT * FROM [dbo].[ThongKeDoanhThu]();
```
<img width="1920" height="1080" alt="Screenshot (50)" src="https://github.com/user-attachments/assets/519fc918-7ea8-4607-b5b2-f562e94381b6" />

_Doanh thu của các sản phẩm_

# Phần 3: Xây dựng Store Procedure
## 3.1 Các loại System SP
Trong SQL Server, có nhiều Stored Procedure có sẵn (System Stored Procedure) do hệ thống cung cấp để hỗ trợ quản trị cơ sở dữ liệu, xem thông tin hệ thống, quản lý user, backup, kiểm tra cấu trúc bảng,...

Một số Stored Procedure

`sp_help`: Xem thông tin cấu trúc bảng.

`sp_rename`: Đổi tên đối tượng.

`sp_who`: Kiểm tra người dùng đang kết nối.

## 3.2. Viết 01 Store Procedure đơn giản để thực hiện lệnh INSERT hoặc UPDATE dữ liệu
- Thêm một loại sản phẩm mới, nhưng phải kiểm tra xem tên loại đó đã tồn tại chưa để tránh trùng lặp dữ liệu.
```
CREATE PROCEDURE [sp_ThemLoaiSanPham]
    @TenLoai NVARCHAR(100)
AS
BEGIN
    IF EXISTS (SELECT 1 FROM [LoaiSanPham] WHERE [TenLoai] = @TenLoai)
    BEGIN
        PRINT N'Lỗi: Tên loại sản phẩm này đã tồn tại!';
    END
    ELSE
    BEGIN
        INSERT INTO [LoaiSanPham] ([TenLoai]) VALUES (@TenLoai);
        PRINT N'Thêm loại sản phẩm thành công.';
    END
END;
GO
EXEC [sp_ThemLoaiSanPham] N'Kem';
```
<img width="1920" height="1080" alt="Screenshot (65)" src="https://github.com/user-attachments/assets/9f97443b-4ca1-41ce-871c-da661a5a83f1" />

_Kem đã được thêm thành công vào Menu_

## 3.3. Viết 01 Store Procedure có sử dụng tham số OUTPUT để trả về một giá trị tính toán
- Tính tổng doanh thu của quán trong 1 ngày cụ thể
```
CREATE PROC [sp_DoanhThuTheoNgay]
    @Ngay DATE,
    @TongDoanhThu MONEY OUTPUT
AS
BEGIN
    SELECT @TongDoanhThu = SUM(H.[SoLuong] * S.[GiaBan])
    FROM [HoaDonChiTiet] H
    JOIN [SanPham] S ON H.[MaSanPham] = S.[MaSanPham]
    WHERE CAST(H.[NgayBan] AS DATE) = @Ngay;
    SET @TongDoanhThu = ISNULL(@TongDoanhThu, 0);
END;
GO
DECLARE @MoneyResult MONEY;
EXEC [sp_DoanhThuTheoNgay] '2026-04-27', @MoneyResult OUTPUT;
SELECT @MoneyResult AS [DoanhThuNgayHomNay];
```
<img width="1920" height="1080" alt="Screenshot (54)" src="https://github.com/user-attachments/assets/5d98fd90-36cf-4db4-b103-451e11ac8859" />


_Doanh thu của quán trong 1 ngày cụ thể_

## 3.4. Viết 01 Store Procedure trả về một tập kết quả (Result set) từ lệnh SELECT sau khi đã join nhiều bảng

- Xuất ra danh sách các món ăn/uống có tổng số lượng bán ra lớn hơn một con số chỉ định
```
CREATE PROC [sp_DanhSachMonBanChay]
    @MucToiThieu INT
AS
BEGIN
    SELECT S.[TenSanPham], SUM(H.[SoLuong]) AS [TongBan]
    FROM [SanPham] S
    JOIN [HoaDonChiTiet] H ON S.[MaSanPham] = H.[MaSanPham]
    GROUP BY S.[TenSanPham]
    HAVING SUM(H.[SoLuong]) >= @MucToiThieu
    ORDER BY [TongBan] DESC;
END;
GO
```
<img width="1920" height="1080" alt="Screenshot (55)" src="https://github.com/user-attachments/assets/b430f113-2f84-485f-9646-58f86c907369" />

_Em đã chạy thành công lệnh_

```
EXEC [sp_DanhSachMonBanChay] 3
```
<img width="1920" height="1080" alt="Screenshot (56)" src="https://github.com/user-attachments/assets/3303bfc1-61d2-49fe-8e1c-a23efbf878ec" />

_Các sản phẩm có số lượng bán > 3 được liệt kê_

# Phần 4: Trigger và Xử lý logic nghiệp vụ
## 4.1. Trigger để tự động làm gì đó tại 1 bảng B khi mà dữ liệu thay đổi dữ liệu ở bảng A
- Khi bán hàng (Insert vào HoaDonChiTiet), tự động cập nhật trạng thái Sản phẩm thành "Hết hàng" (TrangThai = 0) nếu tổng số lượng bán vượt quá 100
```
CREATE TRIGGER [trg_CapNhatTrangThai]
ON [HoaDonChiTiet]
AFTER INSERT
AS
BEGIN
    UPDATE [SanPham]
    SET [TrangThai] = 0
    WHERE [MaSanPham] IN (SELECT [MaSanPham] FROM inserted)
    AND (SELECT SUM([SoLuong]) FROM [HoaDonChiTiet] WHERE [MaSanPham] IN (SELECT [MaSanPham] FROM inserted)) > 100;
END;
```
<img width="1920" height="1080" alt="Screenshot (57)" src="https://github.com/user-attachments/assets/2dc340fb-f765-49ac-b40f-7e3f0db9e27c" />


_Lệnh đã được thực hiện thành công_

## 4.2. Thử nghiệm Trigger vòng lặp

-Viết Trigger cho Bảng A (SanPham)

Khi chèn một sản phẩm mới, tự động cập nhật một ghi chú vào tên của Loại sản phẩm tương ứng ở Bảng B
```
CREATE TRIGGER [trg_A_to_B]
ON [SanPham]
AFTER INSERT
AS
BEGIN
    UPDATE [LoaiSanPham]
    SET [TenLoai] = [TenLoai] + N' (Updated)'
    WHERE [MaLoai] IN (SELECT [MaLoai] FROM inserted);
END;
GO
```
<img width="1920" height="1080" alt="Screenshot (59)" src="https://github.com/user-attachments/assets/5a801285-71bc-4ca6-84f1-f62eea795b00" />

_Tạo thành công bảng Trigger A_

- Viết Trigger cho Bảng B (LoaiSanPham)

Khi Bảng B được cập nhật tên, tự động cập nhật lại trạng thái của các Sản phẩm thuộc loại đó ở Bảng A
```
CREATE TRIGGER [trg_B_to_A]
ON [LoaiSanPham]
AFTER UPDATE
AS
BEGIN
    UPDATE [SanPham]
    SET [TrangThai] = 1
    WHERE [MaLoai] IN (SELECT [MaLoai] FROM inserted);
END;
GO
```
<img width="1920" height="1080" alt="Screenshot (60)" src="https://github.com/user-attachments/assets/a7591053-54b8-4f26-82a1-dfd9e5b0d690" />

_Tạo thành công bảng Trigger B_

- Thử nghiệm
```
INSERT INTO [SanPham] ([TenSanPham], [GiaBan], [MaLoai]) 
VALUES (N'Cà phê Thử Nghiệm', 20000, 1);
```
<img width="1920" height="1080" alt="Screenshot (61)" src="https://github.com/user-attachments/assets/f505d26e-fa73-4302-bc7b-65f19aad9284" />

_Lệnh đã được thực thi thành công mà không xảy ra vấn đề_

#### Nhận xét:

Việc hai Trigger cập nhật qua lại lẫn nhau tạo thành một Đệ quy gián tiếp. Nếu không có điều kiện chặn, nó sẽ làm cạn kiệt tài nguyên hệ thống.

Trong thực tế, việc để hai bảng "soi gương" nhau bằng Trigger là một thiết kế tồi. Nó gây khó khăn cho việc bảo trì và làm chậm hiệu năng đáng kể do mỗi lệnh Update đơn giản đều phải gánh thêm chuỗi giao dịch phức tạp.

# Phần 5: Cursor và Duyệt dữ liệu
## 5.1. Viết một đoạn script sử dụng CURSOR để duyệt qua danh sách của 1 câu lệnh SQL dạng SELECT, duyệt qua từng bản ghi, xử lý riêng từng bản ghi
- Duyệt qua danh sách các hóa đơn. Nếu hóa đơn có giá trị > 100,000đ, in thông báo tặng Voucher 20k, ngược lại tặng Voucher 5k
```
DECLARE @MaHD INT, @TongTien MONEY;

DECLARE cur_Voucher CURSOR FOR 
    SELECT [MaHoaDon], SUM([SoLuong] * [GiaBan])
    FROM [HoaDonChiTiet] H JOIN [SanPham] S ON H.[MaSanPham] = S.[MaSanPham]
    GROUP BY [MaHoaDon];

OPEN cur_Voucher;
FETCH NEXT FROM cur_Voucher INTO @MaHD, @TongTien;

WHILE @@FETCH_STATUS = 0
BEGIN
    IF @TongTien > 100000
        PRINT N'Hóa đơn ' + CAST(@MaHD AS NVARCHAR) + N': Tặng Voucher 20.000đ';
    ELSE
        PRINT N'Hóa đơn ' + CAST(@MaHD AS NVARCHAR) + N': Tặng Voucher 5.000đ';
        
    FETCH NEXT FROM cur_Voucher INTO @MaHD, @TongTien;
END;

CLOSE cur_Voucher;
DEALLOCATE cur_Voucher;
```
<img width="1920" height="1080" alt="Screenshot (62)" src="https://github.com/user-attachments/assets/2b9eb11c-1731-4795-a2b6-e9322a28b5ed" />

_Các hóa đơn đã được giảm giá theo từng giá trị_

# 5.2. Cách giải quyết KHÔNG dùng Cursor
```
SELECT 
    [MaHoaDon], 
    SUM([SoLuong] * [GiaBan]) AS [TongTien],
    CASE 
        WHEN SUM([SoLuong] * [GiaBan]) > 100000 THEN N'Tặng Voucher 20.000đ'
        ELSE N'Tặng Voucher 5.000đ'
    END AS [UuDai]
FROM [HoaDonChiTiet] H 
JOIN [SanPham] S ON H.[MaSanPham] = S.[MaSanPham]
GROUP BY [MaHoaDon];
```
<img width="1920" height="1080" alt="Screenshot (63)" src="https://github.com/user-attachments/assets/58e06d34-93d1-4d84-af8c-9acfc68665e0" />

_Không dùng Cursor bài toán vẫn được giải quyết (Dùng CASE WHEN)_

#### Nhận xét về tốc độ:

Cách dùng SELECT luôn nhanh hơn vì nó xử lý cả khối dữ liệu cùng lúc trong bộ nhớ đệm.

Cursor phải mở con trỏ, duyệt từng dòng, tốn tài nguyên hơn rất nhiều khi bảng có hàng triệu dòng.

# 5.3. Viết bài toán mà chỉ CURSOR mới giải quyết được
Giả sử một khách hàng nợ bạn nhiều lần. Hôm nay, họ đến trả một khoản tiền là 1,000,000 VND.

Yêu cầu: Hệ thống phải tự động trừ vào các khoản nợ cũ nhất trước. Nếu tiền vẫn còn dư, trừ tiếp vào khoản nợ mới hơn cho đến khi hết tiền thì thôi.

Tại sao SQL thuần khó làm? Lệnh `UPDATE` không thể tự biết khi nào thì "hết tiền dư" để dừng lại

```DECLARE @MaKhachHang INT = 1;
DECLARE @SoTienTra MONEY = 1000000; -- Số tiền khách mang đến

DECLARE @MaNo INT, @SoTienNo MONEY, @SoTienTru MONEY;

-- 1. Cursor duyệt các khoản nợ từ cũ nhất đến mới nhất
DECLARE cur_No CURSOR FOR 
    SELECT MaNo, SoTienNo 
    FROM SoNo 
    WHERE MaKhachHang = @MaKhachHang AND SoTienNo > 0
    ORDER BY NgayGhiNo ASC;

OPEN cur_No;
FETCH NEXT FROM cur_No INTO @MaNo, @SoTienNo;

-- 2. Vòng lặp phân bổ tiền trả nợ
WHILE @@FETCH_STATUS = 0 AND @SoTienTra > 0
BEGIN
    -- Nếu tiền khách trả lớn hơn hoặc bằng khoản nợ hiện tại
    IF @SoTienTra >= @SoTienNo
    BEGIN
        SET @SoTienTru = @SoTienNo; -- Trừ hết sạch khoản nợ này
    END
    ELSE
    BEGIN
        SET @SoTienTru = @SoTienTra; -- Chỉ trừ được một phần khoản nợ
    END

    -- Cập nhật bảng SoNo
    UPDATE SoNo 
    SET SoTienNo = SoTienNo - @SoTienTru,
        GhiChu = GhiChu + N' (Đã trả ' + CAST(@SoTienTru AS NVARCHAR) + ')'
    WHERE MaNo = @MaNo;

    -- Trừ số tiền khách trả còn lại trong tay
    SET @SoTienTra = @SoTienTra - @SoTienTru;

    -- Lấy khoản nợ tiếp theo
    FETCH NEXT FROM cur_No INTO @MaNo, @SoTienNo;
END;

CLOSE cur_No;
DEALLOCATE cur_No;

-- Thông báo số tiền còn dư nếu khách trả quá nhiều
IF @SoTienTra > 0
    PRINT N'Khách hàng đã trả hết nợ, tiền còn dư: ' + CAST(@SoTienTra AS NVARCHAR);
ELSE
    PRINT N'Đã phân bổ hết số tiền khách trả vào các khoản nợ.';
```
<img width="1920" height="1080" alt="Screenshot (64)" src="https://github.com/user-attachments/assets/24a887a6-8777-45e2-863a-8469a1a47a12" />

_Khách hàng đã trả hết nợ, số tiền dư đã đucợ cập nhật trên hệ thống_

















