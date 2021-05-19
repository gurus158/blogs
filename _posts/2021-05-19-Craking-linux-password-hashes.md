## Hi

In this blog , I want to discuss, how to crack password hashes of Linux users? 

Like any username - password system , Linux don't save password as plain text. Instead It stores password hashes. 

Every time you entered user password to login , it generates it's hash and compare it with hash it stored.

You might wondering where these password hashes are stored. Linux stores those hashes in a special file named **shadow** at **/etc/shadow** location.

shadow file has entry of each users. An entry in shadow file(each line is an entry) is look like below:

**myuser:$6$m/6Wre3j$jxlFgso6ZbYtWCOvZh8Im3k7lSj8c2zPXmlhrisk3Rj/oyAn9oO0s9qFQPQ4TEzXrzRhPbaheI4AtkEQiJ8w4/:18766:0:99999:7:::**

above string has lot of information in itself. If you break above line by delimiter **":"** the  following substring represents:

- **'myuser'**  --> USERNAME

- **'$6$m/6Wre3j$jxlFgso6ZbYtWCOvZh8Im3k7lSj8c2zPXmlhrisk3Rj/oyAn9oO0s9qFQPQ4TEzXrzRhPbaheI4AtkEQiJ8w4/'** --> PASSWORD

- **'18766'** --> days from Jan 1, 1970 when last time password changed

- **'0'** --> minimum number of days remain when user can change password

- **'99999'** --> maximum number of days after which user is forced to change password

- **'7'** --> number of days to show warning before password has to be changed

So password is stored in second substring i.e 

$6$m/6Wre3j$jxlFgso6ZbYtWCOvZh8Im3k7lSj8c2zPXmlhrisk3Rj/oyAn9oO0s9qFQPQ4TEzXrzRhPbaheI4AtkEQiJ8w4/

this password string made of three parts i.e

**$algorithm_id$SALT$HASH_VALUE**

- **algorithm_id** is an id which represent what algorithm is used while generating hash 

different algorithms and their respective id are:

- - **$1** --> MD5

- - **$2a**  or **$2y** --> BLOWFISH

- - **$5** --> SHA-256

- - **$6** --> SHA-512

- **SALT** is a random bits which used to increase complexity hash or make hash more difficult to crack

- **HASH_VALUE** is the final hash generated by algorithm

In above case algorithm is **SHA-512** , SALT is **"m/6Wre3j"**  and hash is **jxlFgso6ZbYtWCOvZh8Im3k7lSj8c2zPXmlhrisk3Rj/oyAn9oO0s9qFQPQ4TEzXrzRhPbaheI4AtkEQiJ8w4/**

After this much information it is very easy to write a python script to crack password hash from a dictionary also known as **dictionary attack** 

Lets write our python code to crack hash value

```shell
#!/bin/bash
filename='wlist.list'
password='$6$m/6Wre3j$jxlFgso6ZbYtWCOvZh8Im3k7lSj8c2zPXmlhrisk3Rj/oyAn9oO0s9qFQPQ4TEzXrzRhPbaheI4AtkEQiJ8w4/'
salt='m/6Wre3j'
while read line; do
t_hash=`mkpasswd -m sha-512 $line $salt`
if [ $password == $t_hash ]
then
        echo "password found: ${line}"
        exit
fi
done < $filename
echo "no password matched"
```

running above code generates below output

```python
┌──(joker㉿gotham)-[~/data/.hacking/password_cracking]
└─$ ./cracker
password found: zaxscdvf

```

as you can see password come out to be **zaxscdvf** which id 8 length alpha-pass which is very very weak as per modern tech.

if you are wondering how much time it takes to crack passwords of different lengths, below is some data:

![19.png](https://github.com/gurus158/blogs/blob/gh-pages/images/19.png?raw=true)



So you must have passwords at least 12 character long for better security in todays world.