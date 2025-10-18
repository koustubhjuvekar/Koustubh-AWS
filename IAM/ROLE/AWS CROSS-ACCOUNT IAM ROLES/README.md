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

  -  Another AWS account - Account ID - `982081044537`

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
- **Description** - `This role is created for development, for Monica at WeDevelop Company.`

<img width="1350" height="640" alt="image" src="https://github.com/user-attachments/assets/e865c899-0f22-40dc-b409-eda86fd42de5" />
<br>

- Go to IAM → Roles →  `WeDevelopMonicaRole`
- Note the **Role ARN**, for example:

  -  `arn:aws:iam::494341429801:role/WeDevelopMonicaRole`

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

