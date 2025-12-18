## Username
leviathan0

## Password
leviathan0

## Write-Up
<img width="824" height="26" alt="image" src="https://github.com/user-attachments/assets/7e7a624a-fd12-44b1-8d15-e8589cd0ff5e" />

The problem information of leviathan is not given to us intentionally.  
We just find the vulnerability for being in the home directory.  


<img width="543" height="138" alt="image" src="https://github.com/user-attachments/assets/ccf1fa05-9d3d-4d74-b0df-aa600a948b79" />  

There is a various readable text file, but they didn't useful for me except for .backup directory.  


<img width="563" height="83" alt="image" src="https://github.com/user-attachments/assets/e67463fb-816d-4813-9edb-febcc9f86c10" />
<img width="1260" height="684" alt="image" src="https://github.com/user-attachments/assets/c924c258-c8a0-4e2e-980f-efe04479358f" />  

There is a bookmarks.html file, so i opened, and it printed tons of line of text.  


<img width="1262" height="50" alt="image" src="https://github.com/user-attachments/assets/8190dae9-6e75-40ac-9857-997bb0680cbd" />  

So i used PIPE for grep command, and i can find a password for next level.  

## Cheatsheet
ls -la  
|  
grep ""  
