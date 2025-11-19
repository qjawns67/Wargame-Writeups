## Username
natas17

## Password
EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC

## Write-Up
<img width="596" height="173" alt="image" src="https://github.com/user-attachments/assets/c7e40e0a-5182-49ac-b848-1dd39f07f69f" />

I submit username 'test' for testing the logic.  
There was no output result.  
<img width="586" height="483" alt="image" src="https://github.com/user-attachments/assets/a96ec4ba-c1d8-41d8-b1eb-e0c5b671bd01" />

These strings that indicate whether user exists are blocked by a comment character.
If there were no comment sign, we can get a password one by one for blind SQL Injection.
This problem is about Fully Blind SQL Injection.
In this situation, we can not see anything for text, so we should judge true or false for server response time using SLEEP() methods.
When we submit natas18" and sleep(10) and substring(~~~) , if this expression is true, the result screen is indicated after 10 seconds.

## Method of solve
