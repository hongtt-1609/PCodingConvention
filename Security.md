# DotNet Security

Phần này bao gồm một tập hợp các quy tắc về bảo mật trong DotNet

## 1. Mã hóa
- Sử dụng API bảo vệ dữ liệu của Windows (DPAPI) để lưu trữ cục bộ dữ liệu nhạy cảm.
- Mã hóa password bằng một trong các thuật toán băm như: SHA512, Rfc2898DeriveBytes, PBKDF2.
- Sử dụng Nuget để cập nhật các package. Không nên sử dụng từ file dll.

## 2. Truy cập dữ liệu
- Không sử dụng câu sql với tham số được nối chuỗi.

## 3. Quy ước bảo mật web
#### Quy tắc chung



