# Munna_Linux-and-Server-Assessment
Linux and Server Assessment

Question 1: Set Up Your DevOps Project Structure
Step 1 — Create the Directory Structure
mkdir -p /home/ec2-user/webapp/{scripts,logs,config}

<img width="827" height="151" alt="image" src="https://github.com/user-attachments/assets/5a226bdb-787d-401e-81f9-64aadbf07418" />


Verify:

tree /home/ec2-user/webapp

 output:
<img width="581" height="129" alt="image" src="https://github.com/user-attachments/assets/3dcefa33-682f-4961-9c36-4647f4bf6172" />


Step 2 — Create app.conf

Command:

cat> /home/ec2-user/webapp/config/app.conf

sudo nano app.conf

Type:

APP_NAME=WebApp
PORT=8080

Save using:

Ctrl + s

Ctrl + x

Check file:

cat /home/ec2-user/webapp/config/app.conf

Output:

<img width="773" height="112" alt="image" src="https://github.com/user-attachments/assets/ca8454d8-e02e-46bb-9b7e-76d310107304" />



Step 3 — Create Empty Log File
touch /home/ec2-user/webapp/logs/app.log

Verify size:

ls -l /home/ec2-user/webapp/logs/app.log

 output:


<img width="711" height="115" alt="image" src="https://github.com/user-attachments/assets/c7dfec64-6fe0-4bb8-bb9b-8f28595ac167" />



Step 4 — Set Permissions
Give execute permissions to scripts/
chmod 755 /home/ec2-user/webapp/scripts

<img width="749" height="156" alt="image" src="https://github.com/user-attachments/assets/47a15127-73cb-4a03-8903-4d7255f7bc71" />


Set config file permissions
chmod 644 /home/ec2-user/webapp/config/app.conf

<img width="797" height="97" alt="image" src="https://github.com/user-attachments/assets/f985116a-93b7-4fa0-a05e-e36e5d1f9b0a" />


Meaning of 755 and 644
755
rwxr-xr-x
User Type	Permission	Meaning
Owner	rwx	Can read, write, execute
Group	r-x	Can read and execute only
Others	r-x	Can read and execute only

Used mainly for directories and executable files.

644
rw-r--r--
User Type	Permission	Meaning
Owner	rw-	Can read and write
Group	r--	Read only
Others	r--	Read only

Used mainly for normal files.

Step 5 — Change Ownership
sudo chown -R root:root /home/ec2-user/webapp/

Verify:

ls -lR /home/ec2-user/webapp/

 output:


<img width="1002" height="358" alt="image" src="https://github.com/user-attachments/assets/09138032-a0d7-4813-8dd8-27f70122d7cb" />






/home/ec2-user/webapp/:
total 0
drwxr-xr-x 2 root root 22 May 20 10:15 config
drwxr-xr-x 2 root root 21 May 20 10:15 logs
drwxr-xr-x 2 root root  6 May 20 10:15 scripts

/home/ec2-user/webapp/config:
total 4
-rw-r--r-- 1 root root 30 May 20 10:15 app.conf

/home/ec2-user/webapp/logs:
total 0
-rw-r--r-- 1 root root 0 May 20 10:15 app.log

/home/ec2-user/webapp/scripts:
total 0




<img width="547" height="150" alt="image" src="https://github.com/user-attachments/assets/5b222e43-4390-4649-8eb2-93616394419b" />







Question 2: Write an Interactive Log Script

Step 1 — Navigate to Scripts Directory
cd /home/ec2-user/webapp/scripts/
Step 2 — Create Script Using Vim
vim log_user.sh

Press:

i

Paste this script:

#!/bin/bash

read -p "Enter your name: " username

cat /home/ec2-user/webapp/config/app.conf

echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log

cat /home/ec2-user/webapp/logs/app.log

Save and exit:

Esc
:wq
Step 3 — Give Execute Permission
chmod +x log_user.sh

Verify:

ls -l log_user.sh

Example:

-rwxr-xr-x 1 root root 220 May 20 10:25 log_user.sh
Step 4 — Run Script 3 Times
First Run
./log_user.sh

Input:

Chirag

Output:

APP_NAME=WebApp
PORT=8080

Login: Chirag Date: Wed May 20 10:30:12 UTC 2026
Second Run
./log_user.sh

Input:

Priya

Output:

APP_NAME=WebApp
PORT=8080

Login: Chirag Date: Wed May 20 10:30:12 UTC 2026
Login: Priya Date: Wed May 20 10:31:45 UTC 2026
Third Run
./log_user.sh

Input:

Ravi

Output:

APP_NAME=WebApp
PORT=8080





<img width="876" height="321" alt="image" src="https://github.com/user-attachments/assets/f83b3899-ede9-4661-8dca-838e4503e278" />








Question 3: User Management and File Permission Control

Step 1 — Create Group
sudo groupadd writers
Step 2 — Create Users
sudo useradd -m devuser1
sudo useradd -m devuser2
sudo useradd -m devuser3
sudo useradd -m devuser4

<img width="879" height="366" alt="image" src="https://github.com/user-attachments/assets/4a92416e-0903-4db0-b574-be94b0730e14" />

Step 3 — Add Users to Writers Group
sudo usermod -aG writers devuser1
sudo usermod -aG writers devuser2

Verify:

groups devuser1
groups devuser2

Example:


<img width="868" height="103" alt="image" src="https://github.com/user-attachments/assets/964c2d52-584c-4c64-8206-c363f987b98b" />





Step 4 — Change Group Ownership of Script
sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh


<img width="1169" height="44" alt="image" src="https://github.com/user-attachments/assets/83c2c4b3-ba70-47dc-8095-c2ae501180c9" />



Step 5 — Set Permissions
sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh
Permission Breakdown (664)
rw-rw-r--
User Type	Permission
Owner (root)	Read + Write
Group (writers)	Read + Write
Others	Read Only




<img width="1096" height="65" alt="image" src="https://github.com/user-attachments/assets/c36d8cc2-c0b6-4369-a771-afd5be488e5b" />




Step 6 — Verify Permissions
ls -l /home/ec2-user/webapp/scripts/log_user.sh

Expected output:

<img width="1009" height="59" alt="image" src="https://github.com/user-attachments/assets/bdf8a6ff-db96-43f4-82d0-11cde0fb020d" />


-rw-rw-r-- 1 root writers 220 May 20 10:25 log_user.sh
Testing Access
Test Write Access — devuser1
su - devuser1







<img width="788" height="401" alt="image" src="https://github.com/user-attachments/assets/3266261d-19c4-4af9-96e8-ccc3faddea48" />









Append text:

echo '# test by devuser1' >> /home/ec2-user/webapp/scripts/log_user.sh

<img width="685" height="46" alt="image" src="https://github.com/user-attachments/assets/923c46c5-938d-43f0-89f2-1defce570bab" />



This should work successfully.

Test Write Access — devuser2
su - devuser2

Command:

echo '# test by devuser2' >> /home/ec2-user/webapp/scripts/log_user.sh




<img width="805" height="392" alt="image" src="https://github.com/user-attachments/assets/629e86f8-ca54-46d2-99b9-ca3c08bec4ee" />







This should also work.

Test Read-Only Access — devuser3
su - devuser3

Read file:

cat /home/ec2-user/webapp/scripts/log_user.sh

Works fine.

Try writing:

echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh






<img width="726" height="226" alt="image" src="https://github.com/user-attachments/assets/e1004a5a-41f4-4f42-b6c8-4a1c9c2e234d" />















Test Read-Only Access — devuser4
su - devuser4

Read:

cat /home/ec2-user/webapp/scripts/log_user.sh

Write attempt:

echo 'test' >> /home/ec2-user/webapp/scripts/log_user.sh

<img width="762" height="599" alt="image" src="https://github.com/user-attachments/assets/be0d0d93-2840-4c8b-b6c3-2692e9f2156b" />








<img width="1348" height="682" alt="image" src="https://github.com/user-attachments/assets/4ccb7c52-e77b-4e07-8f1e-7faf81d26835" />
