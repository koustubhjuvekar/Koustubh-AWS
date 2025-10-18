## 🚀 AWS Cross-Account IAM Role (Vendor Developer Access)

## 🧠 Question
Your company **StarCloud (Admin account: Koustubh-Admin)** hires an external vendor company **WeDevelop**.  
WeDevelop assigns their developer **Monica** to work temporarily in your AWS account to:

- Launch and configure an EC2 instance  
- Develop a PHP-based web page and upload files to S3  
- Access S3 to verify the uploaded data  

You want **Monica** to have this access for only **5 hours**, after which her access should automatically expire.

---

## 🏢 Scenario Explanation (Professional Context)

This is a real-world **cross-account collaboration model**.  
In cloud-based projects, companies often invite external vendors or freelancers to develop, configure, or test applications within their AWS environment.  

To do this securely, instead of sharing credentials, the company creates a **temporary cross-account IAM role**.

| Role | Description |
|------|--------------|
| **Your account (StarCloud)** | Acts as the *Resource Owner / Admin Account* |
| **Vendor account (WeDevelop)** | Acts as the *Trusted Account* |
| **Developer Monica** | IAM user in WeDevelop who assumes the role you provide |
| **Temporary access** | Automatically expires after the role session ends (5 hours) |

### ✅ This ensures:
- No permanent credentials are shared  
- You control what Monica can do (e.g., only EC2 + S3)  
- Access auto-expires after 5 hours (session duration)

---

## ⚙️ Implementation Steps

### **A. Steps in Your Account — StarCloud (Admin Account)**

#### 1. Create a Role
- Console → **IAM → Roles → Create role**
- Select **Another AWS account**
- Enter **WeDevelop’s AWS Account ID** (Monica’s account ID)
- Click **Next**

<img width="5488" height="2600" alt="image" src="https://github.com/user-attachments/assets/b0362796-a77a-46d4-8ffe-43650a9c95b2" />
<br>


#### 2. Attach Policies
Attach the following AWS managed policies:
- `AmazonEC2FullAccess`
- `AmazonS3FullAccess`

#### 3. Set Session Duration
Under **Maximum session duration**, choose **5 hours (18,000 seconds)**.

#### 4. Review and Create Role
- **Role name:** `WeDevelopMonicaRole`  
- Note the **Role ARN**, for example:

  -  `arn:aws:iam::111122223333:role/WeDevelopMonicaRole`


#### 5. (Optional) Add Condition for Expiry Based on Date/Time
To make sure access is valid only for 5 hours from now (e.g., till `2025-10-18T17:00:00Z`):

```json
"Condition": {
  "DateLessThan": {
      "aws:CurrentTime": "2025-10-18T17:00:00Z"
  }
}
```

This ensures that after this time, even if Monica re-assumes the role, it won’t work.

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

#### 2. Attach Policy to Allow AssumeRole
Attach a **custom policy** allowing `sts:AssumeRole` on your role ARN:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::111122223333:role/WeDevelopMonicaRole"
        }
    ]
}
```
