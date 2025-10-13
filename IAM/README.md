### **IAM**

- IAM is the only service by aws which is fully free.
- Go to IAM console.
- Here remember one thing, before IAM learning we were using root user to log in AWS console. In real life scenario, that root user account would be a company, then owner or founder may open account using email, log in AWS using root id.

- Then he/she create 1 (more if need) Admin account which will have full access of everyting, a highly authorized person.
- And the will sign out from root account. That root account would never be used again to log into AWS cosole. Everything will be worked or managed by Admin.
- Then root id would be used only and only if,  only needed for specific tasks like:
    -  Changing the account email or password
    -  Closing the AWS account
    -  Managing payment methods & tax settings
    -  Restoring IAM user permissions if accidentally locked out <br>

<img width="1351" height="640" alt="image" src="https://github.com/user-attachments/assets/f0576b82-78c9-444b-a5e3-6d733322ca46" /><br>

- Go to **Users** → <kbd>Create user</kbd>

**User details**

-  **User name:** You’re creating a new IAM user called Koustubh_Admin. (This will be the account name for login/programmatic access).

-  Provide user access to the AWS Management Console – _optional_:
   -  If you tick this, **the user will be able to log in to the AWS Console (web login).**
   -  If unchecked → **the user can only access AWS using access keys (CLI/SDK).**


**Are you providing console access to a person?**

Here AWS asks how you want to manage console login (web login).

**Specify a user in Identity Center – Recommended ✅**

   -  Best practice (especially for companies).
   -  Instead of creating many IAM users, AWS recommends using IAM Identity Center (previously AWS SSO).
   -  Advantage: Centralized login, easier user management, better security.
Example: In a company, all employees log in using their company email via Identity Center, not separate IAM users. <br>

**I want to create an IAM user**

   -  Traditional way of creating a user.
   -  You choose this if you specifically need an IAM user (for programmatic access, admin user, testing, or emergency access). <br>
Example: You are making Koustubh_Admin as an IAM admin user.


<img width="1351" height="640" alt="image" src="https://github.com/user-attachments/assets/dd6f449b-15e8-44dc-b9a4-c1291b4f7645" />

Here, First we want to create an ADMIN account and then sign out from root. So select `**I want to create IAM user.**`

Set password, here we set `Password@123`

<img width="1352" height="638" alt="image" src="https://github.com/user-attachments/assets/dcf0ac0f-21f9-461d-98ab-97aa68ff7795" />

??? Users must create a new password at next sign-in - Recommended . WHY?

Answer: It forces the user to set their own password on first login, improving security. <br>
Example: If any illegal or unauthorized activity happens from that account, the user cannot deny it because only they knew the new password — ensures accountability and integrity.
<br>

Click on <kbd>**Next**</kbd>

<img width="1351" height="644" alt="image" src="https://github.com/user-attachments/assets/418ce9ac-73dd-4fb0-9db8-ba81621b5432" />

We will create user group later but select this option.
OR there you can see option to create group, you can click on that also and give group name and add permission.

_Following image is just to show if you click on create group, how it will look like. Close the window and continue to create IAM user_
<img width="1352" height="642" alt="image" src="https://github.com/user-attachments/assets/583b97c3-af66-4400-8ba2-4de1b4857082" />

Here I will create user group later.

??? Set permissions boundary - optional ==> It sets the maximum limit of permissions an IAM user or role can have — even if other policies give more access, they can’t exceed this boundary.

Click on <kbd>**Next**</kbd>

<img width="1352" height="641" alt="image" src="https://github.com/user-attachments/assets/45738cdd-32d7-4c68-9f95-1afe26451baf" />

Click on <kbd>**Create user**</kbd>

<img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/d1beee37-34fc-4a9f-a367-d5385d7181eb" />

Here, <br>
username and first default password, for login will be displayed.
Also login link is there. You can pin or bookmark this link to log in directly to your account on daily basis, then you don't need to click here and there.
Copy link and save or bookmark OR you can find this link later, no worries.

If you see carefully this link contains AWS account id there. 
`https://494341429801.signin.aws.amazon.com/console`
See in image.

<img width="1366" height="43" alt="image" src="https://github.com/user-attachments/assets/c9ce74e3-804b-4d57-a870-384b231717a0" />

<!--now this is ROOT account or Company account, so only id there.
So from now onwards, when you create IAM users or federated users, this account id will be act as domain for everyone.
for example: prajakta@494341429801
koustubh@494341429801
<anyuser@awsaccount> -->
So now you are logged in as root account, so root id or your name will be there. Later when you sign in using IAM, you will see your username below account id.

NOw here,

📘 Explaining `Email Sign-in Instructions` in IAM

Now, here I am the Admin, so I already know my username, password, and sign-in link.
But suppose I am the Company Owner and Admin, and I want to create individual IAM login IDs and passwords for my employees — each with permissions based on their specific role and requirements.

The question is:
👉 How can I inform them about their IAM login credentials?

When you create a new IAM user, you’ll notice an option called `Email sign-in instructions.`
If you click this option, your default email app will automatically open with a pre-filled email template containing the AWS sign-in link, username, and instructions for the new user.

You just need to:

-    Enter the employee’s registered email address (the one they use in your organization).
-    Review or modify the message if needed.
-    Send the mail — the employee will receive all their IAM login details.

Example:

E.g. Here I am sending mail from my one account to another account.
Suppose Main account OR ROOT account(current AWS) : <br>
```
Name : Koustubh
Account ID : 494341429801
Email : maverick******@gmail.com
```
I created a ADMIN account for CEO of the company with full authorization. I want to send him his IAM login credentials. Later I will log out from AWS as a root, Koustubh will handle all AWS as he is ADMINISTRATOR.
So his details
```
Name : Koustubh Juvekar
Account ID : <No need>
Email : koustubhjuvekar07@gmail.com
```

Then, by clicking “Email sign-in instructions”, a ready-made email opens automatically — you just send it to the CEO’s email (koustubhjuvekar07@gmail.com).
After that, you (the root user) log out, and the Admin user (Koustubh Juvekar) will handle all AWS operations from now on.

✅ In short:
The “Email sign-in instructions” option is an easy way to share IAM user login details securely via email. It helps ensure each employee receives their personalized AWS console login information directly.

See in the image what happens when I click on `Email sign-in instructions`

<img width="1366" height="726" alt="emailw" src="https://github.com/user-attachments/assets/29ac9371-6a2e-4a88-9077-63dc87ed89a5" />

Now Koustubh(New user) will receive email like this

<img width="1366" height="647" alt="emailw1" src="https://github.com/user-attachments/assets/d7003f9d-b196-443f-8e1a-fdb80bd2b22e" />

So here we created IAM ADMIN user successfully.
Now we have to create Users group for him. So that we need to give him permission. So let's create Users group.

Click on <kbd>Return to users list</kbd>

<img width="1366" height="640" alt="image" src="https://github.com/user-attachments/assets/43ec1fe9-deb8-4e45-8f62-006fcea21736" />


## USER GROUPS

Users Group (in IAM) – in short:

A group is a collection of IAM users.
You assign permissions or policies to the group, and all users in that group automatically get those permissions.

Example:

Group: `Developers` → Policy: `EC2FullAccess` <br>
All users in the Developers group can manage EC2 instances.

Here I will create `ADMINS` group, and I will apply policies and permissions to **GROUP** and then whoever I want as an ADMIN, I will add them in this group.
So all the permissions and policies will be applied to them automatically.

So 

Go to User groups → Click on <kbd>Create group</kbd>

User group name : `ADMINS` <br>
Add users to the group - Optional : Created users list will be there → select user <br>
`Koustubh-admin`

Attach permissions policies - Optional : There are thousands of permissions policies

Select those you want to apply to group.

Here we are creating group of `ADMINS` so they should have full permissions policies.

So select `AdministratorAccess` - This permissions policy have full authorization of AWS services and account.

<img width="1351" height="638" alt="image" src="https://github.com/user-attachments/assets/e2a8d5ab-f39f-4715-b466-5ee96c6d7506" />

<img width="1350" height="641" alt="image" src="https://github.com/user-attachments/assets/90b52629-88c2-41b7-98ba-3cf9b5744207" />

Click on <kbd>**Create user group**</kbd>

<img width="1366" height="641" alt="image" src="https://github.com/user-attachments/assets/c9a63c9b-2ac2-4e63-b37b-d21c3518cda9" />

Now if you click on `ADMINS` and go in details, check for permission and click on `+` sign. You will get even JSON format of policy link

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

<img width="1350" height="639" alt="image" src="https://github.com/user-attachments/assets/00f76922-9125-4ca9-9d9d-4cbcadf5cdc7" />

Okay Done!!!!!!!!!

So here an owner created 1 root account on AWS for company.<br>
For security and operations, owner created 1 ADMIN account using IAM user.<br>
Created 1 user group `ADMIN` for admins of the company who have full authorization of whole AWS.<br>
Added that IAM user to group. <br>
Now Owner logs out from ROOT account.<br>

So let's log in using IAM credentials received on mail.

Click on the link you received on mail or as previously said, you might have bookmarked it.
`https://494341429801.signin.aws.amazon.com/console`

Enter Username. Here `Koustubh-admin`<br>
Enter password. Here `Password@123`

<kbd>Sign in</kbd>

<img width="1350" height="646" alt="image" src="https://github.com/user-attachments/assets/95de7da5-08aa-4255-9ba7-ec3b14640da2" />
<br>

You will see `Password reset` window. Because as a ADMIN I am logging first time.

So fill the details, set new password.

<img width="1350" height="646" alt="image" src="https://github.com/user-attachments/assets/8ac4924b-1dac-4293-a814-d64af1ecad57" />

<kbd>**Confirm password change**</kbd>

<img width="1366" height="645" alt="image" src="https://github.com/user-attachments/assets/bcd21634-9f3c-475c-9ab7-0f2111074d35" />

Now you will be logged in to your AWS account as ADMIN, with full authorization.

<img width="1350" height="641" alt="image" src="https://github.com/user-attachments/assets/0508f39e-7eed-41a5-9fa2-795e01404124" />

Now as ADMIN we have full access!!

So let's create 2 more users for different roles and give them only required access.

So let's create 

```
1st user: (To handle EC2 only)
User: Virat.ec2
User Group: EC2_Team_Leaders

2nd user: (To handle s3 only)
User: Sachin.s3
User Group: s3_Team_Leaders
```

<img width="1352" height="587" alt="image" src="https://github.com/user-attachments/assets/09edc5be-0ca6-4213-a6c5-0a1468efbccc" />

<img width="1352" height="544" alt="image" src="https://github.com/user-attachments/assets/859d318d-3421-4199-8202-74039f2bc2f6" />

<img width="1351" height="589" alt="image" src="https://github.com/user-attachments/assets/ad02b774-4bdb-4802-8584-eb7c0c7994e9" />

Create user!!!

Similarly create user for s3
_(Here, we are logging in every account for testing, in real life when ADMIN mail them details, employee logs in their system with credentials, and then changes password and use it every day.)_

<img width="1348" height="586" alt="image" src="https://github.com/user-attachments/assets/5c4b4c7b-4479-43ae-a307-fbdf6ce4f603" />

<img width="1352" height="541" alt="image" src="https://github.com/user-attachments/assets/a7daf006-8196-4fc0-8f8b-21baafa5f309" />

Every time we create group later seperately like previous practicals. This time let's create here in process.<br>
So in second page after above, click on <kbd>Create user</kbd>

<img width="1366" height="592" alt="s3user" src="https://github.com/user-attachments/assets/8a6e38f5-d668-45eb-8f8d-f11bc204fb45" />

Enter here group name `s3_Team_Leaders` .<br>
He is Team leader for s3 team, so he needs to have full access of s3, so search for policy `AmazonS3FullAccess`.
When we apply only this policy, Sachin will get full access of `s3 service` only, nothing else.

Click on <kbd>Create user group</kbd>

Now select the group.

<img width="1350" height="586" alt="image" src="https://github.com/user-attachments/assets/fbb3e32d-d9c6-4447-9db5-b84b45761cd8" />

Create user!!!

SO here 2 users created, just consider we mailed them their credentials.

NOw for trial, for s3 we created user and user group too, in ongoing process, <br>
but for ec2, let's create `EC2_Team_Leaders` group and assign `Virat.ec2` to it!

<img width="1358" height="589" alt="image" src="https://github.com/user-attachments/assets/b2ae0438-9b46-4f1e-bfa5-6421c76ded1e" />

Similary like Sachin, here Virat is Team leader for EC2 team, so he needs to have full access of EC2, so search for policy `AmazonEC2FullAccess`.
When we apply only this policy, Virat will get full access of `EC2 service` only, nothing else.

<img width="1351" height="588" alt="image" src="https://github.com/user-attachments/assets/b7118b91-52f9-4d8a-998f-60d2ebfc4c49" />

<img width="1351" height="590" alt="image" src="https://github.com/user-attachments/assets/acbeec92-42ff-41c8-be22-2e34632adf5d" />

Create user group!!

Time for testing, so log out from ADMIN account and first let's try to log in as `Virat`.

Remember, account id will be same for all the accounts, because it is root account or you can say company's account. All the accounts comes under this account. So for login link will be same.

`https://494341429801.signin.aws.amazon.com/console`

**EC2_Team_Leader**
```
username : Virat.ec2
default password : Virat@123
```
<img width="1350" height="645" alt="image" src="https://github.com/user-attachments/assets/e56d870e-9713-48c3-a0ab-5e35ee53092e" />

First time login, change password and click on next, get logged in successfully!

<img width="1348" height="646" alt="image" src="https://github.com/user-attachments/assets/43a9f672-3c67-4739-b994-ac7e2d22463a" />

<img width="1352" height="638" alt="image" src="https://github.com/user-attachments/assets/4f199dcb-8c9d-49fc-a964-afc993e93285" />

see, in right up corner logged in as IAM user we created for EC2 i.e. `Virat.ec2`

So we tried to launch an EC2 and it is launched successfully! All other operations related EC2, can be done!

Let's go to s3 service, from `Virat.ec2` account to check whether is it available for Virat or not.

So go to s3 console → tried to create bucket, you will see following error that is `access denied`

<img width="1349" height="637" alt="image" src="https://github.com/user-attachments/assets/831aa718-450e-495b-b3d5-4ca0d9bd1b54" />

<img width="1353" height="639" alt="image" src="https://github.com/user-attachments/assets/e65aba8c-674a-4bf3-bd99-ac1ecbec1641" />

So here we proved that, Virat has only access to EC2 services, other accesses are denied, we did it using IAM.

Log out from Virat's account and log in using `Sachin`'s Account. Create 1 s3 bucket. That will be successfully created. 

Then go to EC2 from same login and try to launch EC2, you won't be able to launch EC2. Try other services also, you won't be able to use anything besides `s3` only.

See, for Sachin all accesses are denied.

<img width="1350" height="639" alt="image" src="https://github.com/user-attachments/assets/68d27003-4bcc-480b-a922-939a0d97841d" />

<br>

<img width="1354" height="641" alt="image" src="https://github.com/user-attachments/assets/95a63fd3-a95e-4d6c-87da-c57911f973f7" />

Now go to `s3` console and create 1 bucket. It's successfully created!

<img width="1350" height="636" alt="image" src="https://github.com/user-attachments/assets/5ffa17f1-9c47-47bc-8756-a75a7f4da977" />

---

Now

1️⃣ AWS Managed Policies

 -    These are predefined policies created and maintained by AWS.
 -    They cover common use cases like AmazonS3FullAccess, AdministratorAccess, etc.
 -    You can directly attach them to users, groups, or roles — no need to create your own.
 -    Benefit: Automatically updated by AWS when new permissions or services are added.<br>
Example:
```
Policy Name: AmazonEC2FullAccess
Purpose: Grants full access to EC2 resources.
```

🟩 In short:

Ready-made policies managed and updated by AWS for common tasks.
<br>

2️⃣ Customer Managed Policies (Custom Managed Policies)

 -    These are policies you create and manage yourself in your AWS account.
 -    Useful when AWS Managed Policies don’t exactly fit your organization’s needs.
 -    You define permissions in JSON format and can reuse them across multiple users, groups, or roles.
Example:
```
Policy Name: DevEC2LimitedAccess
Purpose: Allows EC2 start/stop only for development instances.
```
🟩 In short:

User-created policies tailored to specific needs — fully managed by you.
<br>
