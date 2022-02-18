# Shell stable

## Python to spawn a PTY
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
#	        or
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
## Put the shell in to background with Ctrl-Z
```bash
Ctrl-Z
```
## Examine the current terminal and STTY info and match it
```bash
echo $TERM
stty -a
```

## The information needed is the TERM type (“xterm-256color”) and the size of the current TTY (“rows 53; columns 238")


## Set the current STTY to type raw and tell it to echo the input characters	
```bash
stty raw -echo
# or In zsh shell use
stty raw -echo; fg
#   Press enter
```
## Foreground the shell with fg and re-open the shell with reset
```bash	
fg
  reset
```

## stty size to match our current window
```bash
export SHELL=bash
export TERM=xterm-256color
stty rows 50 columns 211
```

## Set PATH TERM and SHELL if missing
```bash
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
export TERM=xterm
export SHELL=bash
cat /etc/profile; cat /etc/bashrc; cat ~/.bash_profile; cat ~/.bashrc; cat ~/.bash_logout; env; set
export PS1='[\u@\h \W]\$ '
```


# Simplest way or quick commands
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl z
stty raw -echo; fg
#		press enter 

stty rows 55 columns 238

# (Optional)
export TERM=xterm-256color
```
