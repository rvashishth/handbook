# Linux

- [Linux](#linux)
  - [Concepts](#concepts)
  - [Commands and Concepts](#commands-and-concepts)
    - [Pipeline](#pipeline)
    - [I/O redirection](#io-redirection)
    - [variables](#variables)
    - [Globbing/regex](#globbingregex)
    - [Networking](#networking)
    - [security](#security)
    - [user management](#user-management)
  - [FAQ](#faq)
  - [TODOs](#todos)
  - [Reference Docs](#reference-docs)

## Concepts

**who to find the list of commands available** - command utilities are installed either in `/bin` or in `/usr/bin`

Its better to use `certificates` instead of passwords, no manual intervention is required

`aws ec2` instances run `aws linux` forked from `redhat linux`

`LAMU02Y80D8JG5J:~ rvashish$` **~** means we are on home directory

`$` on terminal is for normal user. `#` sign is for root user.

each command on terminal provides output to either `stdout` or `stderr`

**fingerprint** with `ssh` ensure that with a given ip we always connects with the same host. so if the host for a ip changes in background, fingerprint provides a security in such case.

`xen`,`vmware` and `virtualbox` are two popular **virtualization** tools.

multiple commands at once - </br>
`$ls; pwd` or `$ls && pwd`

one command in multiple line

```bash
$ls \
-l
```

How to check if a tcp connection is reachable </br>
```shell
nc -zv 52.158.209.216 6650
nc -zv pulsar-perf-proxy.centralus.cloudapp.azure.com 6650
```

## Commands and Concepts

use `man`(**manual**) command before any utility command to get the description i.e. `man ssh` will print `ssh` utility's description. </br>
We can also search in manual using `/` followed by search string </br>
We can also **search in all manual pages** using `man -k mysearch` or `man -k "search multi string"` </br>
use `info` instead of `man` for more detailed description. Info either provide more details or content of `man` </br>
use `--help` if no `man` or `info` page is available

use `yum` to install rpm packages </br>
use `apt-get` to install debian packages

`cd` **change directory** just `cd` takes to home directory. `cd /` changes to root directory. `cd ~` take to home directory. `cd -` takes to last location/previous directory from where we came to current dir </br>
**Trivia** on each changed directory, linux creates set the value of `PWD` env variable

`hostname` prints the **computer name**

`ssh` is secure shell

`tar` tap archive

`df`, `df -h --total` display free disk space, total storage, disk size

`wc` word count

`top` and `htop` - process

`ps` process status

`head` `tail` print first/last n lines of a file

`lshw` list hardware
`lscpu` list cpu

`dig` like `ping`, get the list of name servers, a records for a domain name

Create base64 string

`echo -n "s string to encode" | base64`

### Pipeline

`cat myfile | grep Apple | wc -l` this will print the total number of line which has the word Apple in it

to re-run the last command or previous command again use bang and command name
`!cat`

### I/O redirection

[Shell Input/Oput Redirection](https://www.tutorialspoint.com/unix/unix-io-redirections.htm)

Output redirection

Generally output of a command is written on terminal, to write this on a file we can use output redirection

```bash
echo -e "app=google\nenv=dev" > configmap.txt
```

use `>>` to append content after existing content. `>` will override the previous file content

Above will create a configmap.txt file with following content

```bash
app=google
env=dev
```

Use of **Here Document** to build a docker image. `<<` it redirects input into an interactive shell script.

```bash
COMMAND] <<[-] 'DELIMITER'
  HERE-DOCUMENT
DELIMITER
```

```bash
cat <<- Dockerfile
first line
    second line
Dockerfile
```

```bash
docker build <<- EOF
 FROM busybox
 RUN echo "hello world"
EOF
```

### variables

don't use space between variable, `=` sign and value. We can also do inplace execution of shell commands and assign output to a variable

```
$ var1=$(ls)
$ echo $var1
```

`w` who is logged in and what they are doing

`>` write output to given file. override the file content. use `>>` to append on existing content in new line

`env` to **list the environment variable.**

### Globbing/regex
`? question mark` - any value for a character, `* Asterisk` any number of character any value, `[] angle bracket` match character from a rang i.e. `[0-9]` or `[[:digit:]]`, `^ caret` match first character, `$ dollar` match last character, `{} curly brace` multiple patterns

`cp -r ~/Practice/Test ~/Report` this will copy the `Test` dir inside `Report` dir. To copy just content of Test dir use `cp -r ~/Practice/Test/. ~/Report/`

### Networking

`nslookup www.domain.com` will return the IP address for this domain
`traceroute` </br>
`tcpdump` </br>
`ifconfig` </br>
`ipconfig` </br>
`is addr show` or `ip a s` </br>
`ip route show` or `ip r s` </br>
`netstat` </br>
`ss`, `ss -lntp` on which port our service are listening to </br>

`/etc/host` - host ip to dns mapping </br>
`/etc/resolve.conf` - name server </br>
`/etc/services` - list out running services. i.e. see which service is running on port 22 `cat /etc/services | grep 22` </br>

### security

`md5`

`shasum`

`openssl`

### user management

root user always has uid 0 and gid 0

all password are stored at `/etc/shadow` and users are stored at `/etc/passwd`

system users are same as service accounts

## FAQ

**To check exit status of any command** run `echo $?` after the execution of actual command is completed

**Add new path in profile** - below will add content of new PATH env variable in `.profile` file under home directory

```
$ echo PATH=$PATH:$HOME/mydir/ >> ~/.profile
```

**How to check the linux distribution** using terminal? use `cat /etc/*release*` or `cat /etc/*issue*` or `hostnamectl` or `lsb_release -a`

**Count the number of process run by a user** </br>
``

## TODOs

- [x] read `tar`
- [ ] read `make`
- [ ] read `scp`
- [ ] `curl`
- [ ] why do we need `-e` with `echo`.
- [ ] when to use `./` with files/scripts or with commands?

## Reference Docs
