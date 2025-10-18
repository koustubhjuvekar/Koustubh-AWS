# `ASSUME ROLE - User from same AWS account - ROle assign`


<img width="1076" height="728" alt="IAM Test user" src="https://github.com/user-attachments/assets/632c14a2-bf0b-49a2-8b77-b55af85f0263" />

Let's create 1 Test-user as `**John (Test-user)**

So Go to IAM → Click on `Users` → Then <kbd>**Create user**</kbd> → `John`

<img width="1366" height="640" alt="image" src="https://github.com/user-attachments/assets/97898440-0980-4aca-ba52-05ffd27c9876" />

Click on <kbd>**Next**</kbd>

Keep everyhing as it is and Go till end and simply create user!!

<img width="1366" height="642" alt="image" src="https://github.com/user-attachments/assets/72191936-7099-48a5-890c-ac68387d0636" />

Now `John` is a simple user you can see, he has no permission to do anything, nothing is associated to him, he is a simple independent user.

<img width="1350" height="637" alt="image" src="https://github.com/user-attachments/assets/014bdd4d-11b5-417e-b8b7-a2741adca0d6" />

Now for `John`, we need to give him access the console for his login so just got to `Security credentials` → Click on `Enable console access` 

<img width="1349" height="643" alt="image" src="https://github.com/user-attachments/assets/be46ce75-4f60-4bd8-a04f-ecc1804ff2d3" />

<img width="1366" height="640" alt="image" src="https://github.com/user-attachments/assets/9ff83d7f-2c09-4806-8345-eb9c25cacf14" />


*_Here we are not puting **change password after first login password** option, because we need to do testing and practicals, and o save time._


Now copy the link and best way to use another browser or new window and paste there, to log in for `John` AWS account and console.

`https://494341429801.signin.aws.amazon.com/console`

Now this user has no permission, he is just a user. So let's see and confirm that `John` has no permission to access `s3`.

So log in using `John's` credentials

<img width="1351" height="726" alt="image" src="https://github.com/user-attachments/assets/1f45a649-7845-450b-812c-530b753b6e0c" />

So go to `s3` and you can see error that permission denied.

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/f0e83a19-1a01-48c3-9726-63e412bb7115" />

So now let's go back to ADMIN account and create role for `John`. Then ask `John` to **ASSUME** the role, means use that role and perform actions.

Go to `Roles` →  Click on <kbd>Create role</kbd>  → Select `AWS Account` → This account(494341429801) _(We created an account for user John under same AWS account. Suppose under a company's AWS account. )_

<img width="1349" height="639" alt="image" src="https://github.com/user-attachments/assets/b4a48ad6-1421-44ee-82d2-a4958be1935b" />
<br>

 → Select permission `AmazonS3FullAccess`

<img width="1366" height="639" alt="image" src="https://github.com/user-attachments/assets/a93490c3-a45a-4c59-926a-5b988658385c" />

 → Give a
 
   -  Role name : `s3-Full-Access-Role`
   -  Remaining keep as it is, create role!
   
<img width="1349" height="640" alt="image" src="https://github.com/user-attachments/assets/75dfbe9c-d999-44b3-ac80-ff76bb9f77fa" />
<br>

Now go to **Roles** --> Make sure there is role created `s3-Full-Access-Role`

Now we have to assume this role to John, that means assign to John and John has to assume this role, we can say.

So go to IAM Console --> Click on **Users** --> Click on `John`--> Add Permissions --> Create inline policy --> Click on JSON

Now stay here..., 

in new window go to your IAM console --> Roles --> click `s3-Full-Access-Role` --> copy ARN of role

<img width="1350" height="638" alt="image" src="https://github.com/user-attachments/assets/f0d7f227-9cd0-4248-8672-264428c8d98d" />

Now come back to **attach policy page** and add that ARN in front of Resource like,

`"Resource": "arn:aws:iam::494341429801:role/s3-Full-Access-Role"`

and 

`"Action": "sts:AssumeRole",`

So it will be like, 

```JSON
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

Now name the policy and create policy!

-  Name : `s3-Full-Access-Policy`

Now here we attached **Role** to `John`.

<img width="1352" height="639" alt="image" src="https://github.com/user-attachments/assets/b45ce3ac-891f-40dc-9345-254d4613d380" />

Now `John` has to accept it, means `Assume` it.

Now go to `John` login. Refresh it, still John has no access! So click on `John` profile name on the right corner.

Click on `Switch role`

<img width="1366" height="727" alt="image" src="https://github.com/user-attachments/assets/8489f107-d4b3-4d50-8212-cae646328fad" />

Now you will see this window, where you have to fill details

-  Account ID : _(AWS Account ID of ROOT or Main AWS Account who assigned you a role)_ = `494341429801`
-  IAM role name : _(Role name given to you)_ = `s3-Full-Access-Role`
-  Remaining settings are optional

-  Then click on **Switch Role**
-  Then you will be logged in as a `John` and will be working in Root or Admin AWS account for given particular time and as per policy `John` has full acces to **Only s3 services**

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/cc2706be-5102-46d7-ba91-418dd39869a7" />
<br>


Now Till `click on Switch Role`, this is 1 way to switch role 

OR

Another way is Got to your admin account

Go to Roles --> Click on your created role `s3-Full-Access-Role` 

There will be a link to `switch role` you can share it with John, it's just prefilled information. In last way we entered Account ID and ROle name manually, here it will be prefilled.

`https://signin.aws.amazon.com/switchrole?roleName=s3-Full-Access-Role&account=494341429801`

Paste this link for John's login in his window, you will see like this ...

<img width="1366" height="727" alt="image" src="https://github.com/user-attachments/assets/1df73029-8300-4b50-ba95-f57eb3a4e34c" />

Remaining options are optional, but I select it as 

-  Name : `s3-man`
-  Display color : `Red`

Click on **Switch role** 

<img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/ab2bae76-5542-4fe5-bf78-37e1437a1284" />

Now you can see in the right corner display name as `s3-man`, if you click on it you can see

-  Currently active as `s3-Full-Access-Role`
-  Account ID `494341429801` _(here account ID will be same because ROOT ADMIN and even John is under 1 AWS account)_

<img width="1366" height="730" alt="image" src="https://github.com/user-attachments/assets/9e6dfd24-b432-418a-8b19-885d53b6dc28" />
<br>

Now in `John's` login, go to `s3`, you can see that you got s3 access fully!

<img width="1366" height="635" alt="image" src="https://github.com/user-attachments/assets/6664f9f9-9914-43fd-8746-49f38cfba6de" />

Now to test, create a bucket here, I created as `role-access-check-bucket-by-john`

<img width="1366" height="724" alt="image" src="https://github.com/user-attachments/assets/30262c6c-806f-4fa8-9430-b11966141890" />
<br>

I tried to upload `mypic` a picture as an object in the bucket, it's uploaded successfully!

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/641b35de-f536-4166-8294-09f0890eacfb" />

So now John can do anything related to only s3, but for only `1hr`. After `1hr` that Role will be expired and John will loose access to ROOT or ADMIN AWS account and all the services assigned to him, i.e. `s3` here.

---

### How 1hr?

So let's go back to **`ADMIN`** Account.

-	Go to Roles --> `s3-Full-Access-Role`
-	You can see there **Maximum session duration** - `1 hour`

<img width="1352" height="645" alt="image" src="https://github.com/user-attachments/assets/14bd0379-d2e3-4073-a0eb-a715d1029289" />

If you want to change duration then --> Click on **Edit**

Now you will see window to change the `Session duration` of that role. 

As we know we can set maximum 12hrs by clicking in the list, 1-12 hrs. OR any time between this range.<br>
-	For example: if you want to set 2.5 hrs then set it in seconds `9000`

<img width="1366" height="639" alt="image" src="https://github.com/user-attachments/assets/b6b3a6ce-6935-4404-b4e3-5067312b51d6" />
<br>

If you want to set it more that 12hrs, then you have to set it by coding and **you can set that up to maximum 36hrs.**

---

### Revoke sessions

As an ADMIN if you have assigned role to `John` and given any duration. And you think that you have to stop  John working on that role or work and for any reason you want to stop the working under this role, then there is an option **`Revoke sessions`** 

<img width="1366" height="643" alt="image" src="https://github.com/user-attachments/assets/feae8b8e-5758-46ff-9a30-dea8964fd48c" />

When you click on  <kbd>Revoke active sessions</kbd>, all the active session will be expire and access will be denied to all who have assumed this role, in this case it's `John`.

---

### `switch back`

This is for `John`.

If his work is done before Role allowed duration or for any reason he wants to stop working and exit from the role.

Then click on `John` profile name in the right corner or on `s3-man` display name if you set.

Then there is a option <kbd>**Switch back**</kbd>. If `John` click on that option, he will exit from Role `s3-Full-Access-Role` and go back to normal login. i.e like a simple user before assigning role, here!

<img width="1366" height="726" alt="image" src="https://github.com/user-attachments/assets/c88c620f-7efa-4fc0-b0ef-84d0173b273f" />

---


Now after 1hr `John`'s session will be expire. for access to s3, that means using this role. 

But if he continues and not leaving, or if he is able to ASSUME role again and again, then Log in ADMIN and check policy, add condition there.

Set perticular time and date, (UTC) and save policy.

-	Any new “**Switch Role**” attempts will fail immediately.
-	Existing sessions already assumed will not be forcibly terminated,
but they’ll expire automatically when their STS token ends (within 1 hour max).

👉 So if you want instant cutoff, also:
-	Go to IAM → Users → John → Revoke active sessions
-	or delete his sts:AssumeRole permission in his account.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::494341429801:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
                "DateLessThan": {
                    "aws:CurrentTime": "2025-10-18T06:23:00Z"
                }
            }
        }
    ]
}
```

---
### 🧩 Q2. What happens if the Condition is not added and only the role’s “Maximum session duration” is set to 1 hour?

Answer (Part 1 — Behavior):
-	If only the 1-hour maximum session duration is set, John’s individual session (temporary credentials from sts:AssumeRole) will automatically expire after 1 hour.
-	However, after expiry, John can simply assume the role again — creating a new 1-hour session.
-	AWS does not prevent re-assuming unless a time-based Condition or permission restriction is added.

Answer (Part 2 — Result for John):
-	John will lose access only when his current session token expires,
-	but he can immediately switch role again or run sts:AssumeRole again to regain full access — effectively having unlimited total access, just in 1-hour blocks.

---

### 🧩 Q1. Why after 1 hour John was still able to assume or use the role even though role session duration was 1 hour?

Answer (Part 1 — Reason):

-	The “Maximum session duration” in AWS only limits how long a single STS session lasts (e.g., 1 hour).
-	It doesn’t stop the user from re-assuming the role again after expiry.
-	If the trust policy is open (e.g., "Principal": {"AWS": "arn:aws:iam::<account-id>:root"}), John can keep assuming the role repeatedly.
-	AWS doesn’t automatically revoke or block new AssumeRole calls after 1 hour.

Answer (Part 2 — Fix):

-	To enforce strict 1-hour total access:

 	-	Add a time-based condition in the trust policy (e.g., DateLessThan with UTC time).
 	-	Optionally remove John’s sts:AssumeRole permission or revoke active sessions after 1 hour.
  	-	Keep maximum session duration = 1 hour to limit individual token validity.
