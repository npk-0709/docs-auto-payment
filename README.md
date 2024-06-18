# Hướng Dẫn Sử Dụng API LICENSE v2.0

## ENDPOINT PANEL MANAGER: <a href="https://apikey.phukhuong79.com" target="_blank" >https://apikey.phukhuong79.com</a>

## SERVER API URL: https://apikey.phukhuong79.com/client/payment

## ACCESS_TOKEN DEMO : `7c0f4afa2c5a539da7b7c8c6e05ddd21`

## Method : GET /create.php
- Với yêu cầu GET /create.php sẽ khởi tạo thông tin thanh toán.
### Tham Số Yêu Cầu
Khi gửi yêu cầu GET đến API, bạn cần cung cấp các tham số sau:

- `licensekey`: [TYPE=string] Mã Bản Quyền Của Bạn (Bắt Buộc). 
- `client_api`: [TYPE=string] Mã Công Cụ Phía Máy Khách Của Bạn (Bắt Buộc).
- `day`: [TYPE=string] Số Ngày Đăng Kí Thêm , Khi Thanh Toán Sẽ Được Cộng Trực Tiếp Vào API (Bắt Buộc).
- `amount`: [TYPE=string] Số Tiền Cần Thanh Toán (VNĐ) (Bắt Buộc).
- `access_token`:[TYPE=string] AccessToken Lấy Ở Phần Thông Tin Cá Nhân Của Tài Khoản (Bắt Buộc).

### Định Dạng Phản Hồi
Phản hồi từ API sẽ là một đối tượng JSON chứa Thông Tin:
### Nếu API Tồn Tại

- `status`: Trạng Thái Của Truy Vấn -> 'success'.
- `licensekey`: Mã Bản Quyền Của Bạn.
- `client_api`: Mã Công Cụ Phía Máy Khách Của Bạn.
- `ngay_kich_hoat`: Ngày Kích Hoạt Lần Đầu Của API.
- `ngay_het_han`: Ngày Hết Hạn Của API .
- `trang_thai`: 1 Trong 2 'Expires'(Hết Hạn) hoặc 'Active'(Còn Hạn).

### Nếu API Không Tồn Tại
- `status`: Trạng Thái Của Truy Vấn -> 'error'.
- `msg`: Thông Báo Lỗi.

### Ví Dụ Phản Hồi Thành Công ( Tìm Thấy API Trong CSDL)
```json
{
    "status": "success",
    "licensekey": "KAUTO-4E7AF-EBCFB-xxxxx-xxxx-xxxxx",
    "client_api": "ToolV1",
    "ngay_kich_hoat": "2024-06-16 22:54:08",
    "ngay_het_han": "2024-09-24 22:54:08",
    "trang_thai": "Active"
}
```
### Ví Dụ Phản Hồi Thất Bại( Không Tìm Thấy API Trong CSDL)
```json
{
    "status": "error",
    "msg":"Not Found"
}
```

## Method : POST
- Với yêu cầu POST bạn sẽ đăng kí mới 1 API nếu API đó kiểm tra là chưa được đăng kí.
### Tham Số Yêu Cầu
Khi gửi yêu cầu POST đến API, bạn cần cung cấp các tham số sau:

- `licensekey`:[TYPE=string] Mã Bản Quyền Của Bạn (Bắt Buộc). 
- `client_api`:[TYPE=string] Mã Công Cụ Phía Máy Khách Của Bạn (Bắt Buộc).
- `ngay_het_han`:[TYPE=string Format: "YYYY/MM/DD H:I:S"] Thời Gian Hết Hạn Của API  (Bắt Buộc).
- `access_token`:[TYPE=string] AccessToken Lấy Ở Phần Thông Tin Cá Nhân Của Tài Khoản (Bắt Buộc).

### Ví Dụ Yêu Cầu

```bash
curl -G https://apikey.phukhuong79.com/api/client.php \
    --data-urlencode "licensekey=Mã_Bản_Quyền_Của_Bạn" \
    --data-urlencode "client_api=Mã_Công_Cụ_Phía_Máy_Khách_Của_Bạn" \
    --data-urlencode "ngay_het_han=2024-06-17 11:46:58"\
    --data-urlencode "access_token=AccessToken_Của_Bạn"

```
### Định Dạng Phản Hồi
Phản hồi từ API sẽ là một đối tượng JSON chứa Thông Tin:
### Nếu Đăng Kí Mới

- `status`: Trạng Thái Của Truy Vấn -> 'success' .
- `type`: Loại Truy Vấn -> 'register' .
- `ngay_het_han`: Ngày Hết Hạn Của API .
- `trang_thai`: 1 Trong 2 'Expires'(Hết Hạn) hoặc 'Active'(Còn Hạn).

### Nếu API Đã Tồn Tại , Thì Sẽ UPDATE Lại Ngày Hết Hạn
- `status`: Trạng Thái Của Truy Vấn -> 'success' .
- `type`: Loại Truy Vấn -> 'extend' .

### Ví Dụ Phản Hồi Thành Công ( Đăng Kí Mới )
```json
{
    "status": "success",
    "type": "register",
    "ngay_het_han": "2024-09-24 22:54:08",
    "trang_thai": "Active"
}
```
### Ví Dụ Phản Hồi Thành Công ( Nếu API Đã Tồn Tại )
```json
{
    "status": "error",
    "msg":"extend"
}
```

## Thông Báo Lỗi Khác 
### Không Có Access_Token or Access_Token Không Chính Xác
```json
{
    "status": "error",
    "msg": "XÁC THỰC THẤT BẠI"
}
```
Hoặc
```json
{
    "status": "error",
    "msg": "Thiếu Trường `access_token`"
}
```
