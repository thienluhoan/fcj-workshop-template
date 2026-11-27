---
title: "Tài khoản Cognito và kiểm thử API"
date: 2026-11-23
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Bước 1 – Admin tạo tài khoản (người dùng không tự đăng ký được)
```powershell
$pool = (Get-Content backend-config/deployed_env.json | ConvertFrom-Json).cognito_user_pool_id
aws cognito-idp admin-create-user --user-pool-id $pool --username owner@example.com `
  --user-attributes Name=email,Value=owner@example.com Name=email_verified,Value=true `
  --desired-delivery-mediums EMAIL
aws cognito-idp admin-add-user-to-group --user-pool-id $pool --username owner@example.com --group-name ADMIN
```
Tạo thêm một tài khoản WAITER để thử phân quyền. Lần đăng nhập đầu, người dùng phải đổi mật khẩu tạm và cài MFA bằng ứng dụng Authenticator (TOTP).

### Bước 2 – Kiểm thử API
App client chỉ bật SRP nên mật khẩu không gửi dạng thô. Cách dễ nhất để lấy token là đăng nhập trên giao diện web, sau đó mở DevTools → Application → Local Storage và copy `idToken`.

```powershell
$api = (Get-Content backend-config/deployed_env.json | ConvertFrom-Json).api_gateway_invoke_url
curl.exe -s "$api/health"                                   # 200 {"status":"healthy"}
curl.exe -s -o NUL -w "%{http_code}" "$api/orders"          # 401: không có token
curl.exe -s -X POST -H "Authorization: $env:WAITER_TOKEN" -H "Content-Type: application/json" -d '{"name":"Pho bo","price":50000}' "$api/menu"   # 403: sai vai trò
curl.exe -s -X POST -H "Authorization: $env:ADMIN_TOKEN" -H "Content-Type: application/json" -d '{"name":"Pho bo","price":50000}' "$api/menu"    # 200/201
```

| Kịch bản | Kết quả mong đợi | Lớp chặn |
| --- | --- | --- |
| Không có token | 401 Unauthorized | Cognito authorizer |
| Token hết hạn/giả mạo | 401 Unauthorized | Cognito authorizer |
| WAITER thêm món (`POST /menu`) | 403 Forbidden | `@require_roles(['ADMIN'])` trong Lambda |
| ADMIN thêm món (`POST /menu`) | 200/201 | — |
| Gửi > 50 request/giây | 429 Too Many Requests | API Gateway throttling |

*(Ảnh minh hoạ kết quả 200/401/403 – nhóm bổ sung)*

### Kiểm tra dữ liệu
Sau khi tạo đơn trên giao diện, mở **DynamoDB → securetable-orders-dev → Explore items** để thấy item mới, và **audit-logs** để thấy bản ghi hành động kèm người thực hiện và thời điểm.
