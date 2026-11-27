---
title: "Cleaning up resources"
date: 2026-11-23
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

### One-command cleanup
```powershell
.\scripts\nuke_and_teardown.ps1          # type DESTROY to confirm
.\scripts\nuke_and_teardown.ps1 -Force   # no prompt
```
The script runs in order:
1. Aborts unfinished multipart uploads and deletes all objects, versions and delete markers in the 3 buckets. Versioned buckets cannot be removed by `terraform destroy` while old versions remain.
2. `terraform destroy -auto-approve`, retrying automatically on dependency errors.
3. Deletes CloudWatch log groups auto-created by API Gateway/Lambda to avoid hidden log storage charges.
4. Rescans DynamoDB, Cognito, API Gateway and WAF in `ap-southeast-1` to confirm **no resources remain**.

### Manual checks
- **Billing → Bills** and **Cost Explorer** the next day: no service should still be incurring charges.
- Check **every region**, not just Singapore, in case something was created in the wrong region.
- Unsubscribe the SNS email if no longer needed. Keep AWS Budgets so you are still alerted.

### Recovery drill
Run `nuke_and_teardown.ps1 -Force`, then `deploy_and_test.ps1`, and time it. The whole infrastructure must come back and `/health` must return `healthy` without any manual Console steps.
