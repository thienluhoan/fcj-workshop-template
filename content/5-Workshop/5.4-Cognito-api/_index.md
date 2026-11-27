---
title: "Cognito accounts and API testing"
date: 2026-11-23
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Step 1 – Admin creates accounts (self sign-up is disabled)
```powershell
$pool = (Get-Content backend-config/deployed_env.json | ConvertFrom-Json).cognito_user_pool_id
aws cognito-idp admin-create-user --user-pool-id $pool --username owner@example.com `
  --user-attributes Name=email,Value=owner@example.com Name=email_verified,Value=true `
  --desired-delivery-mediums EMAIL
aws cognito-idp admin-add-user-to-group --user-pool-id $pool --username owner@example.com --group-name ADMIN
```
Create a WAITER account too, to test authorization. On first sign-in the user must change the temporary password and set up TOTP MFA with an authenticator app.

### Step 2 – Test the API
The app client only allows SRP, so passwords never travel in plain text. The easiest way to get a token is to sign in through the web UI, then copy `idToken` from DevTools → Application → Local Storage.

```powershell
$api = (Get-Content backend-config/deployed_env.json | ConvertFrom-Json).api_gateway_invoke_url
curl.exe -s "$api/health"                                   # 200 {"status":"healthy"}
curl.exe -s -o NUL -w "%{http_code}" "$api/orders"          # 401: no token
curl.exe -s -X POST -H "Authorization: $env:WAITER_TOKEN" -H "Content-Type: application/json" -d '{"name":"Pho bo","price":50000}' "$api/menu"   # 403: wrong role
curl.exe -s -X POST -H "Authorization: $env:ADMIN_TOKEN" -H "Content-Type: application/json" -d '{"name":"Pho bo","price":50000}' "$api/menu"    # 200/201
```

| Scenario | Expected result | Blocked by |
| --- | --- | --- |
| No token | 401 Unauthorized | Cognito authorizer |
| Expired/forged token | 401 Unauthorized | Cognito authorizer |
| WAITER adds a menu item (`POST /menu`) | 403 Forbidden | `@require_roles(['ADMIN'])` in Lambda |
| ADMIN adds a menu item (`POST /menu`) | 200/201 | — |
| More than 50 requests/second | 429 Too Many Requests | API Gateway throttling |

*(Screenshots of 200/401/403 results – team to add)*

### Check the data
After placing an order in the UI, open **DynamoDB → securetable-orders-dev → Explore items** to see the new item, and **audit-logs** to see the action record with actor and timestamp.
