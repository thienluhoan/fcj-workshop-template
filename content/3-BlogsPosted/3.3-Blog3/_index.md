---
title: "Blog 3 (Group blog) - Upload to S3 with Presigned URLs"
date: 2026-11-04
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

### Publication Details

- **Posted by:** KT Trang, a teammate.
- **Published:** 4 November 2026, at 1:37 p.m. as shown in the supplied screenshot.
- **Published in:** AWS Study Group VN on Facebook.
- **Recognition:** A team blog included in this report under the team's shared blog credit arrangement.
- **Original post:** [Read on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2325113631586929).

### How Can a Client Upload to S3 Without Receiving an Access Key?

Avatar and PDF uploads are common application features. Sending every file through the backend before uploading it to S3 makes the backend carry the file-transfer bandwidth. Direct uploads avoid that overhead, but the frontend must not receive an access key. Presigned URLs address this need.

#### 1. What Is a Presigned URL?

A presigned URL is signed using the credentials of the IAM principal that creates it. It permits a specific operation, such as GET or PUT, on a specific object for a limited time. The bucket remains private.

The URL cannot grant more permissions than its signer has. If the backend role lacks `s3:PutObject`, requests using the generated URL still receive a 403 response.

#### 2. Upload Flow

1. The client calls the API with a JWT to request upload permission.
2. The backend checks the user, generates a key such as `uploads/{userId}/{uuid}.jpg`, and signs a URL that expires after five minutes.
3. The backend returns the presigned URL to the client.
4. The client sends the file directly to S3 using PUT; the file does not pass through the backend.

{{< event-photo src="images/blogs/s3-presigned-url/upload-flow.png" alt="A client requests a presigned URL through API Gateway and Lambda, then uploads directly to a private S3 bucket using PUT." caption="KT Trang's upload-flow diagram: the backend grants temporary upload access while S3 receives the file directly." >}}

#### 3. The Configured Expiration Is Not Always the Actual Lifetime

- URLs created with an SDK or the CLI can have a maximum expiration of seven days; the Console allows one minute to twelve hours.
- A URL expires at its configured expiration or when the signing credentials expire, whichever comes first. URLs signed with Lambda, ECS or EC2 role credentials can therefore expire earlier than configured.

#### 4. Practical Considerations

- Anyone holding the URL can use it, potentially multiple times until it expires. Use a short lifetime and avoid logging the URL.
- Uploading to an existing key can overwrite the object. Avoid using a client-supplied filename as the object key.
- Presigned PUT is difficult to use for file-size limits. When a size restriction is needed, presigned POST supports the `content-length-range` condition.
- Validate the file type and contents on the server after upload.

S3 handles the file transfer, while the backend decides who may upload, which object they may upload and how long permission lasts.

### Publication Evidence

{{< event-photo src="images/blogs/s3-presigned-url/facebook-publication.png" alt="KT Trang's AWS Study Group VN post, dated November 4 at 1:37 p.m." caption="Screenshot of Blog 3 posted by KT Trang, supplied for the team report." >}}

### Reference

- [Amazon S3 User Guide: Download and upload objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
