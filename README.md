# Hướng Dẫn Sử Dụng API Tự Động Thanh Toán

### ENDPOINT PANEL MANAGER: <a href="https://apikey.phukhuong79.com" target="_blank" >https://apikey.phukhuong79.com</a>

### SERVER API URL: https://apikey.phukhuong79.com/client/payment

### ACCESS_TOKEN DEMO : `7c0f4afa2c5a539da7b7c8c6e05ddd21`

## Method : GET /getBank.php
- Với yêu cầu GET /getBank.php sẽ lấy thông tin thanh toán.
### Tham Số Yêu Cầu
Khi gửi yêu cầu GET đến API, bạn cần cung cấp các tham số sau:

- `access_token`:[TYPE=string] AccessToken Lấy Ở Phần Thông Tin Cá Nhân Của Tài Khoản (Bắt Buộc).

### Định Dạng Phản Hồi
Phản hồi từ API sẽ là một đối tượng JSON chứa Thông Tin:

### Ví Dụ Phản Hồi Thành Công
```json
{
    "status": "success",
    "data": {
        "type": "ACB",
        "stk": "xxxxxxx",
        "ctk": "NGUYEN PHU KHUONG",
        "paychar": "paynpk"
    }
}
```
### Ví Dụ Phản Hồi Thành Công Nhưng Chưa Cập Nhật Thông Tin Thanh Toán
### Vui Lòng Cập Nhật Ở : https://apikey.phukhuong79.com/views/banking.php
```json
{
    "status": "success",
    "data": {}
}
```

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

### Ví Dụ Phản Hồi Thành Công
```json
{
    "status": "success",
    "data": {
        "enter_paycode": "paynpk99045a1b5245f7bb",
        "accountNo": "xxxx",
        "accountName": "NGUYEN PHU KHUONG",
        "amount": "10000",
        "qr_image": "https://img.vietqr.io/image/ACB-xxxx-qr_only.png?amount=10000&addInfo=paynpk99045a1b5245f7bb&accountName=NGUYEN PHU KHUONG"
    }
}
```
### Ví Dụ Phản Hồi Thất Bại
```json
{
    "status": "error",
    "msg":"Không Tìm Thấy licensekey hoặc client_api"
}
```

## Method : GET /tracking.php
- Với yêu cầu GET /tracking.php sẽ liên tục kiểm tra trạng thái của `enter_paycode`.
### Tham Số Yêu Cầu
Khi gửi yêu cầu POST đến API, bạn cần cung cấp các tham số sau:

- `enter_paycode`:[TYPE=string] Là Nội Dung Thanh Toán Được Lấy Ở Phần /create.php  (Bắt Buộc).
- `access_token`:[TYPE=string] AccessToken Lấy Ở Phần Thông Tin Cá Nhân Của Tài Khoản (Bắt Buộc).

### Ví Dụ Yêu Cầu

### Định Dạng Phản Hồi
Phản hồi từ API sẽ là một đối tượng JSON chứa Thông Tin:

### Ví Dụ Phản Hồi Thanh Toán Thành Công
```json
{
    "status": "success",
    "data": {
        "status": "Success", -> đã thanh toán thành công
        "enter_paycode": "xxxxxxxxxxxxxxxx",
        "timePay": "2024-06-18 21:22:20",
        "timeCreate": "2024-06-18 21:21:30"
    }
}
```
### Ví Dụ Phản Hồi Chưa Thanh Toán
```json
{
    "status": "success",
    "data": {
        "status": "Pending", -> đang chờ thanh toán
        "enter_paycode": "xxxxxxxxxxxxxxxx",
        "timePay": "2024-06-18 21:22:20",
        "timeCreate": "2024-06-18 21:21:30"
    }
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
