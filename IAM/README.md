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
Answer: It forces the user to set their own password on first login, improving security.
Example: If any illegal or unauthorized activity happens from that account, the user cannot deny it because only they knew the new password — ensures accountability and integrity.


