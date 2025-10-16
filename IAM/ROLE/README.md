### 🧩 Task — Cross-Account Temporary Access Using IAM Role

**`Scenario:`**

   -  Koustubh (AWS Account A) wants to allow an external employee, EllysePerry, from another company’s AWS account (Account B) to temporarily access specific services in his AWS account for system installation.

**`Objective:`**
  
   -  Give EllysePerry access to:

      1.  Launch and configure EC2 instances
      2.  Install required applications (like Nginx/PHP)
      3.  Access S3 buckets (read/write)
   
   -  Access should be:

      1.  Temporary — valid for only 1 hour
      2.  Secure — using an IAM Role and STS AssumeRole mechanism
      3.  Least Privilege — limited to EC2 and S3 only

**`Implementation Steps`**

  -  In Account B (Ellyse’s company):

     1.  Create IAM user EllysePerry.
     2.  Attach policy to allow her to assume the cross-account role in Koustubh’s account.
  
  -  In Account A (Koustubh’s account):

     1.  Create a Role named PracticalAccessRole.
     2.  In the trust policy, allow only the specific user ARN from Account B:arn:aws:iam::AccountB-ID:user/EllysePerry.
     3.  Attach permissions for EC2FullAccess and S3FullAccess.
     4.  Set Maximum Session Duration = 3600 seconds (1 hour).

  -  In Account B (Ellyse’s side):

     1.  Assume the role using AWS CLI or Console Switch Role.
     2.  Perform installation tasks (launch EC2, install application, use S3).

  -  After 1 hour: Session expires automatically (ExpiredToken error).

  -  Ellyse loses access to Koustubh’s account.

**`Proof to Capture (Screenshots)`**

  -  Role creation with trust policy.
  -  Permissions attached (EC2 + S3).
  -  Role duration set to 1 hour.
  -  Ellyse’s AssumeRole success (aws sts get-caller-identity).
  -  EC2 creation / S3 access proof.
  -  Expired token error after 1 hour.

**`Concepts Covered`**

  -  Cross-account access
  -  IAM Role trust & permission policies
  -  STS temporary credentials
  -  Role session expiration
  -  Least privilege principle

###**Account B - IAM User - EllysePerry**

Root user created here an IAM user `EllysePerry`
## 🅰️ Steps in Ellyse’s Account (Account B) — Do This First

### 🎯 Goal  
Create an **IAM user** `EllysePerry` that is allowed to call `sts:AssumeRole` on your role.

---

### 🧭 Step 1 — Sign in
- Sign in to **Account B (Ellyse’s admin)**.

---

### 🧩 Step 2 — Create IAM User
- **Console Path:** `IAM → Users → Add users`
- **Username:** `EllysePerry`
- **Access type:**  
  - ✅ *AWS Management Console access*  
  - *(Optionally)* *Programmatic access* — if she will use CLI.
- **Password setup:**  
  - Choose *Auto-generate* or *Custom password*  
  - (Optional) Check **User must create a new password at next sign-in**.
- Click **Create user**

---

### 📸 Screenshot #1  
- Capture the **final “Success” page** showing the new user details.  
- Copy and save the **User ARN** for reference.

<img width="1351" height="638" alt="image" src="https://github.com/user-attachments/assets/764f153d-3058-4a68-ab76-2828049ae9fa" />

