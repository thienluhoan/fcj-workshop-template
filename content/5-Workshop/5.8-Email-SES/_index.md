---
title: "Email with SES"
date: 2026-11-27
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

SEAL sends password-reset codes and contest notifications. Amazon SES sends them from `no-reply@<DOMAIN>`, a send-only address on the team's own domain.

#### Step 1. Deploy the SES stack

`TestRecipient` is an address you own; SES sends it a verification link.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-ses --template-file infra/v3/ses.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> TestRecipient=<YOUR_EMAIL>
```

The template creates, all in the Route 53 hosted zone:

| Record | Purpose |
|---|---|
| Domain identity with **Easy DKIM** (3 CNAME records) | Mail is signed, so receivers can check it really comes from the domain |
| Custom MAIL FROM `mail.<DOMAIN>` (MX + SPF) | SPF alignment for bounces |
| DMARC `p=none` | Receivers report results without rejecting mail yet |

Click the link SES sends to `TestRecipient`. In **SES → Identities**, the domain must show **Verified** and DKIM **Successful**.

![SES domain identity, DKIM and MAIL FROM](/images/5-Workshop/5.8-Email-SES/ses-identity.png)

*The domain identity in SES (ap-southeast-1):*

- *Identity status **Verified**.*
- *DKIM configuration **Successful**, Easy DKIM with RSA 2048-bit keys.*
- *Custom MAIL FROM `mail.<DOMAIN>` **Successful**, so the bounce domain aligns with SPF.*
- *The identity ARN is masked. The account is still in the SES sandbox, so mail only goes to verified addresses.*

#### Step 2. Send a test email

```powershell
aws sesv2 send-email --region ap-southeast-1 --from-email-address no-reply@<DOMAIN> --destination "ToAddresses=<YOUR_EMAIL>" --content "Simple={Subject={Data=SEAL test},Body={Text={Data=Hello from SES}}}"
```

In the received mail, **Show original** must show `DKIM: PASS` and `SPF: PASS`.

After the backend is configured with the sender address, the application sends its own mail through the same identity. Below, the coordinator approved a new account and the user received the notice:

![Account approved e-mail sent by SEAL through SES](/images/5-Workshop/5.8-Email-SES/account-approved-email.png)

*The **Account approved** e-mail from `no-reply@<DOMAIN>`, sent by the backend through SES when a coordinator approves a registration. It lands in the **Inbox**, not in spam, because DKIM, SPF (custom MAIL FROM) and DMARC are aligned with the domain. The account is still in the SES sandbox, so this works only because the recipient address was verified first.*

{{% notice warning %}}
A new SES account is in the **sandbox**: it only sends to verified addresses, 200 messages per day. The team kept the sandbox during the workshop because the restored database contains real student addresses. Request production access only after the data has been cleaned and the sender address is configured in the backend.
{{% /notice %}}
