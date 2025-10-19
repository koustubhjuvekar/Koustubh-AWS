# 🚀 `AWS Cross-Account IAM Role`

## Task

Your company **StarCloud (Admin account: Koustubh-Admin)** hires an external vendor company **WeDevelop**.  
WeDevelop assigns their developer **Monica** to work temporarily in your AWS account to:

- Launch and configure an EC2 instance  
- Develop a PHP-based web page and upload files to S3  
- Access S3 to verify the uploaded data  

You want **Monica** to have this access for only **5 hours**, after which her access should automatically expire.

---

## Scenario Explanation 

This is a real-world **cross-account collaboration model**.  
In cloud-based projects, companies often invite external vendors or freelancers to develop, configure, or test applications within their AWS environment.  

To do this securely, instead of sharing credentials, the company creates a **temporary cross-account IAM role**.

| Role | Description |
|------|--------------|
| **Your account (StarCloud)** | Acts as the *Resource Owner / Admin Account* |
| **Vendor account (WeDevelop)** | Acts as the *Trusted Account* |
| **Developer Monica** | IAM user in WeDevelop who assumes the role you provide |
| **Temporary access** | Automatically expires after the role session ends (5 hours) |

### This ensures:
- No permanent credentials are shared  
- You control what Monica can do (e.g., only EC2 + S3)  
- Access auto-expires after 5 hours (session duration)

---

## `Steps`

### **A. Steps in Your Account — StarCloud (Admin Account)**

### **1. Create a Role**

- Console → **IAM → Roles → Create role**
- Select **Another AWS account**
- Enter **WeDevelop’s AWS Account ID** (Monica’s account ID)

  -  Another AWS account - Account ID - `982081044537`

- Click **Next**

<img width="5488" height="2600" alt="image" src="https://github.com/user-attachments/assets/b0362796-a77a-46d4-8ffe-43650a9c95b2" />
<br>


### **2. Attach Policies**

Attach the following AWS managed policies:
- `AmazonEC2FullAccess`
- `AmazonS3FullAccess`

### **3. Set Session Duration**

Under **Maximum session duration**, choose **5 hours (18,000 seconds)**.

#### 4. Review and Create Role

- **Role name:** `WeDevelopMonicaRole`
- **Description** - `This role is created for development, for Monica at WeDevelop Company.`

Create Role!

<img width="1350" height="640" alt="image" src="https://github.com/user-attachments/assets/e865c899-0f22-40dc-b409-eda86fd42de5" />
<br>
<br>

- Now Go to IAM → Roles →  `WeDevelopMonicaRole`
- Note the **Role ARN**, for example:

  -  `arn:aws:iam::494341429801:role/WeDevelopMonicaRole`

-  Now Scroll down → Click on `Permissions` → Add Permission → Create inline policy → JSON →
-  Paste below policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreatePolicy",
        "iam:GetPolicy",
        "iam:ListPolicies",
        "s3:*"
      ],
      "Resource": "*"
    }
  ]
}
```

<img width="1352" height="641" alt="image" src="https://github.com/user-attachments/assets/5f564d1b-824e-4ed9-959a-ea61ae83cb6a" />
<br>
<br>

-  Name - `EditPolicyPermissionForMonica`

<img width="1352" height="644" alt="image" src="https://github.com/user-attachments/assets/947e1897-884c-4780-8beb-b779057fbe3e" />
<br>

-  Create Policy!



So this policy gives Monica full access to all S3 buckets and objects in your AWS account, and also allows her to create, view, and list IAM policies. Because while working with s3, she needs to create policy.

- Now Scroll down → Go to **`Trust relationships`** → Edit trust policy → 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "TrustWeDevelopMonica",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::982081044537:user/Monica.developer"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "DateGreaterThan": {
            "aws:CurrentTime": "2025-10-18T16:00:00Z"
            },
        "DateLessThan": {
            "aws:CurrentTime": "2025-10-18T21:00:00Z"
            }
        }
    }
  ]
}


```

<img width="1351" height="640" alt="image" src="https://github.com/user-attachments/assets/82e99ac3-ebc0-47de-a871-94b7c5834c27" />
<br>

So this means,

For Monica, The role can only be assumed between 16:00Z and 21:00Z (UTC)
i.e. 21:30 IST – 02:30 IST.
After that time window, AWS STS will automatically deny the AssumeRole request.

Click on <kbd> Update policy</kbd>
<br>

_#### 5. (Optional) Add Condition for Expiry Based on Date/Time
To make sure access is valid only for 5 hours from now (e.g., till `2025-10-18T17:00:00Z`):_

```json
"Condition": {
  "DateGreaterThan": {
    "aws:CurrentTime": "2025-10-18T16:00:00Z"
  },
  "DateLessThan": {
    "aws:CurrentTime": "2025-10-18T21:00:00Z"
  }
}

```

This ensures that after this time, even if Monica re-assumes the role, it won’t work.

---

## 🧩 Trust Policy in IAM Role 

---

### 🧠 What is a Trust Policy?

A **trust policy** defines **who is allowed to assume a role**.

It establishes a **trust relationship** between the **role owner account** (your AWS account) and the **trusted entity** (another AWS account, IAM user, or service).

In a **cross-account scenario**, it allows users from another AWS account to **access your resources temporarily** via `sts:AssumeRole`.

---

### 💡 Why Do We Add a Trust Policy?

When you create a role, **no one can assume it by default**.

To enable a specific **external account or IAM user** (like *Monica* from *WeDevelop*), you must **explicitly define that trust** in the role’s trust policy.

This ensures that access is:
- ✅ Secure  
- ✅ Controlled  
- ✅ Limited only to the entities you specify

---

### 🧾 What Does the Trust Policy Contain?

A trust policy includes these key fields:

| **Field** | **Purpose** |
|------------|-------------|
| `"Effect": "Allow"` | Grants permission to assume the role |
| `"Principal"` | Defines *who* can assume the role (user, account, or service) |
| `"Action": "sts:AssumeRole"` | Allows the principal to perform the `AssumeRole` API call |
| `"Condition"` | *(Optional)* Adds extra security, like time-based expiry or MFA requirement |

---

### 👤 Why Are We Using `"Principal"`?

`"Principal"` identifies the **trusted entity** that is allowed to assume the role.

In this case, the trusted entity is **Monica** — an IAM user in the **WeDevelop** account.

**Example:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::982081044537:user/Monica.developer"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### 🧩 Why Are We Adding Monica’s ARN Here?

Monica’s **ARN** uniquely identifies her IAM user across AWS.  

By using her ARN, we ensure that **only she** (not the entire WeDevelop account or other users) can assume the role.  

This is a **fine-grained security approach** — access is tied to a **single trusted identity**, not a whole account.  

---

### ⏳ Why Add the Time-Based Condition?

To automatically **expire the trust after a specific UTC time**.  

This ensures **temporary access** — even if Monica tries to re-assume the role later, AWS denies it.  

**Example condition:**
```json
"Condition": {
  "DateLessThan": {
    "aws:CurrentTime": "2025-10-18T17:00:00Z"
  }
}
```

---


---

### **B. Steps in WeDevelop Account (Monica’s Account)**

#### 1. Create IAM User Monica

- Go to **IAM → Users → Add user**
- **Username:** `Monica`
- Provide **Console access** (or **Programmatic access** if using CLI)
- Keep remainin as it is, simply create user.

<img width="1350" height="633" alt="image" src="https://github.com/user-attachments/assets/a02ebc86-9466-4007-a719-3b0860e673b3" />
<br>

<img width="1351" height="600" alt="image" src="https://github.com/user-attachments/assets/b5fb694c-db27-4b56-8682-d612868981c2" />
<br>

#### 2. Attach Policy to Allow AssumeRole

Here Monica has to assume that role.

So go to IAM → Users → `Monica.developer` → Click on `Permissions` → Add Permissions → Create inline policy → JSON

Attach a **custom policy** as given below, allowing `sts:AssumeRole` on your(Monica's) role ARN:

Now we noted Role ARN in ADMIN Account of StarCloud Company, paste that here for **Resource**

-  Resource - `arn:aws:iam::494341429801:role/WeDevelopMonicaRole`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::494341429801:role/WeDevelopMonicaRole"
        }
    ]
}
```
<img width="1366" height="725" alt="image" src="https://github.com/user-attachments/assets/ebda7aa2-6685-4c02-b208-311366b3014a" />
<br>

Give suitable policy name such as `DeveloperPolicyForStarCloud`

Click on <kbd> Create Policy </kbd>

So here we set!

### Monica will assume the cross-account IAM role after 21:30 IST to work in StarCloud’s (my) AWS account. Once she successfully assumes the role, she will temporarily gain the permissions defined in the role’s policy, allowing her to launch EC2 instances, develop the application, and use S3 as required.

### The role session is limited to 5 hours, which is the maximum session duration configured in the role settings, and we’ve also enforced a time-based condition (aws:CurrentTime) in the trust policy to automatically expire access after 5 hours.

### Therefore, after 02:30 IST, Monica’s session will automatically expire, and she will lose access to StarCloud’s AWS account — ensuring temporary, secure, and time-bound access for external vendor users.

## `Testing`

See when you log in with Monica's account, i.e when Monica will log in to her account all access is denied, even before assuming role

<img width="1366" height="723" alt="image" src="https://github.com/user-attachments/assets/779c00c4-8cdb-4833-85e2-837d933aa181" />

As a Monica...

Now it's 21:29IST, let's try to assume role using link or filling details.

You can see access denied, she is not able to ASSUME role. WHY? Because we set starting time 21:30IST (2025-10-18T16:00:00Z).

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8f772c6d-1289-4968-922b-cf256ae1fa7d" />
<br>

Now after 21:30 IST (2025-10-18T16:00:00Z), let's try again to ASSUME role.

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/83e91db7-78c9-43f5-a9a0-01f72b77daca" />
<br>

### See, Monica's 
<img width="1366" height="768" alt="Developer Monica" src="https://github.com/user-attachments/assets/578a92f6-a298-4f5a-8550-85b0869e0992" />
<br>

---

Now as Monica, let's start application development, after she assumes role given by StarCloud.

### Step 1 — Launch an EC2 Instance

-  Name: `image-upload-app`
-  AMI: `Amazon Linux 2023 (or Ubuntu 22.04)`
-  Instance type: `t2.micro`
-  Security group:
   -  `SSH (22) — Your IP`
   -  `HTTP (80) — Anywhere (0.0.0.0/0)`
-  IAM Role: the one Monica assumed (has S3FullAccess and EC2FullAccess) + EC2AccessRole
-  Key pair: select one (e.g., MonicaKeyPair)

<img width="1366" height="725" alt="image" src="https://github.com/user-attachments/assets/37c660e3-4bdf-4e97-a01b-5364d5d966cc" />
<br>


-  Copy EC2 ip
-  Open powershell or client SSH

Now connect to EC2 for web application development! 

```bash
ssh -i "C:\Users\koust\Downloads\MonicaKeyPair.pem" ec2-user@13.51.166.62
```

```bash
sudo yum update -y
```

### Creating s3 bucket

As a Monica, you can create bucket using command or from website console or use an existing bucket.

-  To create bucket using CLI, run this command in terminal of EC2,
  
```bash
aws s3 mb s3://monica-image-upload-bucket --region eu-north-1
```

## May be you can see...

# ERROr of s3

**WHY?** Because now Monica has full access to s3 and EC2 services in StarCloud Company's AWS account. So she launched EC2 also and get connected. But now she wants to create s3 bucket using commands, that means from EC2. That means **This EC2 must have permission to run command and access s3.**

So let's go back to StarCloud(My ADMIN) Koustubh-admin account, and create role for `EC2` to access `AmazonEC2FullAccess` and `AmazonS3FullAccess`, So we can create s3 bucket from this EC2.

***
Also we need to create custom policy to Give some more actions for s3.

_In real case Monica can't do, she has to request ADMIN of StarCloud Company to create that role for EC2 and attach_ 

_OR else she can create bucket manually from website_

---

So for while in ADMIN account,

### Attach IAM Role to EC2 for S3 & Application Access

Why?

-  Allow the EC2 instance to directly access AWS services like Amazon S3 (for image upload and retrieval) and Amazon EC2 APIs (for management or automation tasks), without using any Access Keys.
-  This ensures secure and temporary credentials are automatically provided to the instance via the Instance Metadata Service (IMDS).

0. Create Policy (1 custom policy needed to allow s3 actions, and the we attach policy to role, so firstly...)

   -  Go to IAM → Policies →  Create policy →  JSON
   -  Copy and Paste JSON code given below as it is

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3Upload",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::starcloud-photo-storage",
        "arn:aws:s3:::starcloud-photo-storage/*"
      ]
    }
  ]
}
```
   -  Set policy name: **`PhotoAppS3Policy`**

Create policy!
<br>

1. Create an IAM Role
      
   -  In the AWS Management Console:
      -  Navigate to IAM → Roles → Create Role
   -  Trusted Entity Type: Select `AWS Service`
   -  Choose `EC2` → Click Next

2. Now Attach the Following Policies to the role:

|Policy Name	|  Purpose  |
|------|--------------|
|`AmazonS3FullAccess`	|  Gives full S3 access for uploading, downloading, and managing objects — useful during development/testing. |
|`AmazonEC2FullAccess`	|  Enables EC2 management actions (optional, used if application needs to manage EC2 instances). |
|`PhotoAppS3Policy` (custom)	|  Restricts access to a specific bucket used by the app (starcloud-photo-storage). Provides only upload, read, and list permissions. |



<img width="1350" height="640" alt="image" src="https://github.com/user-attachments/assets/6ced2046-cd5a-471a-9654-858367dd2eb9" />
<br>

<img width="1351" height="640" alt="image" src="https://github.com/user-attachments/assets/44e133c8-710b-4a64-8068-807dd03f49b2" />
<br>


✅ Explanation:

This custom policy grants:
  
  -  `PutObject` → to upload images to the S3 bucket.
  -  `GetObject` → to retrieve or view uploaded images.
  -  `ListBucket` → to list the contents of the bucket.

It’s limited to one specific bucket, ensuring principle of least privilege (security best practice).

3. Name & Create the Role

  -  Role Name: `EC2AccessRole`
  -  Review → Create Role.

4. Attach Role to the EC2 Instance

   -  After the instance is launched:
   -  Go to EC2 → Instances
   -  Select the instance → Actions → Security → Modify IAM Role
   -  From dropdown, choose `EC2AccessRole`
     
Save changes.

AWS automatically attaches the role to the instance. The instance will now assume this role every time it runs, and get temporary credentials for S3/EC2 access.

---

### Now As a Monica, again go back to your console, refresh it!


### STEP 4 — Install Required Packages

```bash
sudo yum update -y
sudo yum install -y nodejs git
```
Verify Node:

```bash
node -v
npm -v
```

### STEP 5 — Create Project Folder

```bash
mkdir image-upload
cd image-upload
npm init -y
npm install express aws-sdk multer cors
```

STEP 6 — Backend (index.js)

Create file:
```bash
nano index.js
```

Paste below code 👇

Just replace 2 fields as per your situation:
-  your region
-  your bucket name

```js
const express = require('express');
const multer = require('multer');
const AWS = require('aws-sdk');
const cors = require('cors');
const app = express();

app.use(cors());
const upload = multer({ storage: multer.memoryStorage() });

AWS.config.update({ region: 'ap-south-1' }); // change region
const s3 = new AWS.S3();
const BUCKET = 'monica-image-upload-bucket'; // change to your bucket name

app.post('/upload', upload.single('file'), (req, res) => {
  const params = {
    Bucket: BUCKET,
    Key: Date.now() + '_' + req.file.originalname,
    Body: req.file.buffer,
    ContentType: req.file.mimetype
  };

  s3.upload(params, (err, data) => {
    if (err) {
      console.error(err);
      res.status(500).send('Error uploading file');
    } else {
      res.send({ message: 'Upload successful', url: data.Location });
    }
  });
});

app.use(express.static('public'));
app.listen(80, () => console.log('Server started on port 80'));
```
<br>

Save file!

### STEP 7 — Frontend (public/index.html)

Create a folder and file:
```bash
mkdir public
nano public/index.html
```
Paste this code 👇
```html
<!DOCTYPE html>
<html>
<head>
  <title>Upload to S3</title>
</head>
<body>
  <h2>Upload Image to S3</h2>
  <input type="file" id="fileInput">
  <button onclick="uploadFile()">Upload</button>
  <p id="msg"></p>

<script>
async function uploadFile() {
  const file = document.getElementById('fileInput').files[0];
  if (!file) return alert('Select a file first!');

  const formData = new FormData();
  formData.append('file', file);

  const response = await fetch('/upload', { method: 'POST', body: formData });
  const result = await response.json();
  document.getElementById('msg').innerHTML =
    result.url ? `✅ Uploaded! <a href="${result.url}" target="_blank">View</a>` : '❌ Failed';
}
</script>
</body>
</html>
```
<br>

Now run:

```bash
sudo node index.js
```
<img width="1366" height="729" alt="image" src="https://github.com/user-attachments/assets/18036c95-f2f9-4615-8cfe-b52346bc328a" />
<br>

## STEP 9 — Test from Browser

-  Now first go to s3 → Click on your bucket (monica-image-upload-bucket) → See it's empty.

<img width="1366" height="724" alt="image" src="https://github.com/user-attachments/assets/731fd09d-c0f7-4438-98fb-28c835be7f9a" />
<br>

-  Now Visit your EC2 public IPv4:

   -  http://<EC2-Public-IP>/
     
   -  here `http://13.51.166.62/`

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/b04c8f58-71c4-4a2d-8b21-43fdbeaa46e1" />
<br>

   -  Select any image and click `Upload`.
   -  Wait 2–3 seconds — you’ll see a success link.
   -  Click link → image should open directly from S3 bucket.

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/3b911613-3bc7-442f-9742-c2fd4ff7a388" />
<br>

<img width="1366" height="725" alt="image" src="https://github.com/user-attachments/assets/fd2a1a5c-f10c-41af-a9c7-f62bccacaf51" />
<br>

Now if you click on `View` you may see access deined and output like in below image. This is because now we are acting as a public and we have blocked all publick access to s3 bucket, which is better approach for security.
You are directly trying to access image in s3, which is not possible. So only for testing purpose, let's allow public access in s3.

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/02f3b39c-b64b-4672-85a5-97da1dd5f16a" />
<br><br>

So now let's fix it.

-  Make the bucket (and files) public for testing

   -  Go to S3 Console → Your bucket (monica-image-upload-bucket)
   -  Click Permissions tab
   -  Scroll to **Block public access (bucket settings)** → Click Edit
   -  **Uncheck** `Block all public access`
   -  Confirm and Save

<img width="1366" height="723" alt="image" src="https://github.com/user-attachments/assets/03a7042d-2463-49db-838f-5727639e6d57" />
<br>

-  Scroll to Bucket Policy → Add this JSON:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::monica-image-upload-bucket/*"
    }
  ]
}
```

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/3309859b-e6f6-46be-9cb3-9a289ab517e1" />
<br><br>


-  Save changes.

Now go back to your upload page and click on `view`. — it should open in your browser ✅

<img width="1366" height="727" alt="image" src="https://github.com/user-attachments/assets/a08aa04f-1d5b-4d34-9f6f-8c2c3c2be33e" />
<br>

### STEP 10 — Verify in AWS Console

Go to:

-  S3 → your bucket → here (monica-image-upload-bucket) → check `Objects` tab → **Refresh it**!

Your uploaded file should appear!!

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/820b9c8d-672c-49e6-ae47-b7583b98e00b" />

Click on image file

### Here you will see all the details of image! Here you can download and open image!

---

## Here we done with the testing for **`AWS Cross-Account IAM Role`**

---

Now Monica's Role will expire after 5hrs and she won't be assume it again! 

So StarCloud company's application is developed by Monica who is Developer in WeDevelop company(Another).
Using IAM Role for Another AWS account, we (Koustubh-admin = StarCloud company) assigned a IAM Role to Monica for 5hrs, to develop **Photo Uploading application using EC2 and s3**.
We gave her `AmazonEC2FullAccess` and `AmazonS3FullAccess` for 5hrs. She developed application and tested successfully.

So IAM Role assigned to her will expire after 5hrs and she'll loose access or if still time left, but work is done. Then
-  She can Switch back and exit from role - From her console
-  As Admin, we can revoke session - From Koustubh-admin account.





THE END
