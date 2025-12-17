## Write-Up
<img width="474" height="108" alt="image" src="https://github.com/user-attachments/assets/c22e66c9-89bc-4869-9a92-610f0b19f64b" />  

When we input '?id=admin' at URL, it is wrong.  
But the final value of ID we need is admin.  
So, we should avoid typing 'admin' directly, and then the value should be admin when it is URL decoded.  
So, value of id before URL decoding, should be %61dmin, and that before, % character is must be %25 when we input the value.  
Final process is %2561dmin -> %61dmin -> admin  

## Cheatsheet
```php
echo urldecode()
echo urlencode()
```
## Vulnerability
Double Encoding Attack
