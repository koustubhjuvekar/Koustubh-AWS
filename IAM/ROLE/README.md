### `ASSUME ROLE - User from same AWS account - ROle assign`

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
















