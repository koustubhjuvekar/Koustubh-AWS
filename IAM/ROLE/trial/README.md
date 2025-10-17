# 🧩 Assume Role — User from Same AWS Account

This guide demonstrates how to **create a user (John)** in your AWS account and allow him to **assume a role** with specific permissions (in this case, full access to S3) — all within the **same AWS account**.

---

## 🔹 Step 1: Create a Test User — `John`

- Go to: **IAM → Users → Create user → John**
- Keep all default settings and click **Create user**

<img width="1366" height="640" alt="image" src="https://github.com/user-attachments/assets/97898440-0980-4aca-ba52-05ffd27c9876" />

- After creation, `John` will appear as a simple user without any permissions.

<img width="1350" height="637" alt="image" src="https://github.com/user-attachments/assets/014bdd4d-11b5-417e-b8b7-a2741adca0d6" />

---

## 🔹 Step 2: Enable Console Access for `John`

- Go to: **IAM → Users → John → Security credentials**
- Click on **Enable console access**
- Set a password
- (Optional) Disable “Change password after first login” — useful for quick testing

<img width="1349" height="643" alt="image" src="https://github.com/user-attachments/assets/be46ce75-4f60-4bd8-a04f-ecc1804ff2d3" />

> 📝 Copy the login link and open it in another browser window to log in as `John`:  
> `https://494341429801.signin.aws.amazon.com/console`

---

## 🔹 Step 3: Verify That `John` Has No Permissions

- Log in to the AWS console as `John`
- Go to **S3**
- You’ll see an access denied error, as no permissions are assigned yet

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/f0e83a19-1a01-48c3-9726-63e412bb7115" />

---

## 🔹 Step 4: Create a Role for `John`

- Go back to your **Admin account**
- Navigate to: **IAM → Roles → Create role**
- Choose:
  - **Trusted entity type:** AWS Account
  - **This account (494341429801)**
- Attach the following permission policy: **AmazonS3FullAccess**
- Set:
  - **Role name:** `s3-Full-Access-Role`
- Leave the remaining options as default and click **Create role**

<img width="1366" height="639" alt="image" src="https://github.com/user-attachments/assets/a93490c3-a45a-4c59-926a-5b988658385c" />

---

## 🔹 Step 5: Attach Inline Policy to `John`

Now, we’ll allow `John` to **assume** the newly created role.

- Go to: **IAM → Users → John → Add permissions → Create inline policy**
- Select **JSON** tab

Then, open another tab:
- Go to **IAM → Roles → s3-Full-Access-Role**
- Copy the **Role ARN**

Example ARN: `arn:aws:iam::494341429801:role/s3-Full-Access-Role`


Now paste it into the JSON policy editor as below:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::494341429801:role/s3-Full-Access-Role"
    }
  ]
}
```

## 🔹 Step 6: Assume the Role as `John`

### 🔸 Option 1 — Manually Switch Role

- Log in to the AWS Management Console as **John**.
- In the upper-right corner, click on the **profile name**.
- Select **Switch role**.

Fill in the following details:
- **Account ID:** `494341429801`
- **Role name:** `s3-Full-Access-Role`
- (Optional) Customize **Display name** and **Color**.

Then, click **Switch role**.

<img width="1366" height="727" alt="image" src="https://github.com/user-attachments/assets/8489f107-d4b3-4d50-8212-cae646328fad" />

Once switched, `John` will temporarily assume the IAM role **s3-Full-Access-Role**, gaining permissions defined in that role (here, full access to S3).

---

### 🔸 Option 2 — Use a Pre-Filled Switch Role Link

Instead of manually entering details, the Admin can share a **pre-filled switch-role URL**.

- In the Admin account, navigate to: **IAM → Roles → s3-Full-Access-Role**.
- Copy the **Switch role** link:
  
  -  `https://signin.aws.amazon.com/switchrole?roleName=s3-Full-Access-Role&account=494341429801`


- Share this URL with `John`.  
- When `John` opens the link, the **Account ID** and **Role name** fields will already be filled.

Example configuration:
- **Name:** `s3-man`
- **Display color:** `Red`
- Click **Switch role**.

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/ab2bae76-5542-4fe5-bf78-37e1437a1284" />

After switching, the console will display:
- **Active role:** `s3-Full-Access-Role`
- **Account ID:** `494341429801`

<img width="1366" height="730" alt="image" src="https://github.com/user-attachments/assets/9e6dfd24-b432-418a-8b19-885d53b6dc28" />

---

## 🔹 Step 7: Test S3 Access

- Log in as `John` under the assumed role session.
- Navigate to **S3** and create a bucket.  
  Example bucket name: `role-access-check-bucket-by-john`.
- Upload an object (for example, an image file).
- The upload completes successfully, confirming that the **S3 Full Access** role is functioning as expected.

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/641b35de-f536-4166-8294-09f0890eacfb" />

---

## 🔹 Step 8: Session Duration Configuration

- In the Admin account, navigate to: **IAM → Roles → s3-Full-Access-Role**.
- The default **Maximum session duration** is **1 hour**.

<img width="1352" height="645" alt="image" src="https://github.com/user-attachments/assets/14bd0379-d2e3-4073-a0eb-a715d1029289" />

- Click **Edit** to modify the duration.  
- You can set any value between **1 and 12 hours**.  
  - Example: For 2.5 hours, enter `9000` seconds.  
- To extend beyond 12 hours (up to 36 hours), use **AWS CLI** or **SDK** methods programmatically.

<img width="1366" height="639" alt="image" src="https://github.com/user-attachments/assets/b6b3a6ce-6935-4404-b4e3-5067312b51d6" />

---

## 🔹 Step 9: Revoke Active Sessions

If the Admin needs to immediately terminate active sessions for this role (for example, to stop access or revoke permissions):

- Go to: **IAM → Roles → s3-Full-Access-Role**.
- Click on **Revoke active sessions**.

<img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/feae8b8e-5758-46ff-9a30-dea8964fd48c" />

This action instantly ends all active sessions for users (like `John`) who have assumed the role.

---

## ✅ Summary

- Created a test IAM user: **John**
- Created a role: **s3-Full-Access-Role** with **AmazonS3FullAccess**
- Added an inline policy to allow John to **AssumeRole**
- Switched roles to gain **temporary access** to S3
- Verified permissions and configured **session duration**
- Learned how to **revoke sessions** when needed

---
