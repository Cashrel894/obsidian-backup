#CTF 
https://overthewire.org/wargames/bandit/
## Level 0
```sh
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```
密码直接输入 `bandit0`

## Level 1
```sh
cat readme
```
密码为 `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`

## Level 2
```sh
cat ./-
```
密码为 `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`

## Level 3
```sh
cat ./'--spaces in this filename--'
```
密码为 `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`

## Level 4
```sh
ls -a
cat ...Hiding-From-You
```
密码为 `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`

## Level 5
```sh
bandit4@bandit:~/inhere$ find . -type f | xargs file -i
./-file06: application/octet-stream; charset=binary
./-file09: application/octet-stream; charset=binary
./-file01: application/octet-stream; charset=binary
./-file08: application/octet-stream; charset=binary
./-file00: application/octet-stream; charset=binary
./-file03: application/octet-stream; charset=binary
./-file07: text/plain; charset=us-ascii
./-file02: application/octet-stream; charset=binary
./-file05: application/octet-stream; charset=binary
./-file04: application/octet-stream; charset=binary
bandit4@bandit:~/inhere$ cat ./-file07 
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```
密码为 `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`

## Level 6
```sh
bandit5@bandit:~/inhere$ find . -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
```
这里发现只有一个匹配项，所以另外两个条件是多余的。

密码为 `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`

完整版可见 https://www.reddit.com/r/linuxquestions/comments/ku3agh/question_about_find_comand_and_finding_humand/

## Level 7
```sh
bandit6@bandit:~$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```
密码为 `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`

## Level 8 
```sh
andit7@bandit:~$ cat data.txt | grep "millionth"
millionth	VR1ljMayciFxbnUokuQmJFw6QC9VKtub
```
密码为 `VR1ljMayciFxbnUokuQmJFw6QC9VKtub`

## Level 9
```sh
bandit8@bandit:~$ cat data.txt | sort | uniq -c | sort -n | head -n 1
      1 EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
```
密码为 `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`

## Level 10
```sh
bandit9@bandit:~$ strings data.txt | grep -E "==+"   
cL0========== the
========== password
>========== is
R========== B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
```
密码为 `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`

## Level 11
```sh
bandit10@bandit:~$ base64 -d data.txt 
The password is pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
```
密码为 `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`

## Level 12
```sh
bandit11@bandit:~$ tr < data.txt 'A-Za-z' 'N-ZA-Mn-za-m'
The password is GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```
密码为 `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN` 

## Level 13
```sh
bandit12@bandit:~$ mktemp -d
/tmp/tmp.tTsXzwBJZC
bandit12@bandit:~$ cd /tmp/tmp.tTsXzwBJZC
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cp ~/data.txt .
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ xxd -r data.txt > d0   
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d0
d0: gzip compressed data, was "data2.bin", last modified: Wed Jun 24 14:58:58 2026, max compression, from Unix, original size modulo 2^32 578
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat d0 | gzip -d > d1
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d1
d1: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat d1 | bzip2 -d > d2
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d2
d2: gzip compressed data, was "data4.bin", last modified: Wed Jun 24 14:58:58 2026, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat d2 | gzip -d > d3
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d3
d3: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ tar xvf d3
data5.bin
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ tar xvf data5.bin
data6.bin
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat data6.bin | bzip2 -d > d4
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d4
d4: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ tar xvf d4
data8.bin
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jun 24 14:58:58 2026, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat data8.bin | gzip -d > d5
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ file d5
d5: ASCII text
bandit12@bandit:/tmp/tmp.tTsXzwBJZC$ cat d5
The password is qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```
密码为 `qQYQiHOBPR8zR61qxYqX45quvihF2uzk`

## Level 14
```sh
❯ scp -P 2220 bandit13@bandit.labs.overthewire.org:"~/sshkey.private" .
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit13@bandit.labs.overthewire.org's password: 
sshkey.private                                        100% 2602     5.6KB/s   00:00    
```

```sh
❯ chmod 600 sshkey.private
❯ ssh bandit14@bandit.labs.overthewire.org -p 2220 -i sshkey.private
```

```sh
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```
密码为 `aaWecNkG4FhxJQxz07uiwzVP6bJiYS65`

## Level 15
```sh
bandit14@bandit:~$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
Correct!
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7

Connection closed by foreign host.
```
答案为 `pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7`

## Level 16
```sh
bandit15@bandit:~$ openssl s_client -connect localhost:30001
...
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
Correct!
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V

closed
```
答案为 `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`

## Level 17
```sh
bandit16@bandit:~$ nmap localhost -p 31000-32000                                       
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-30 13:16 +0000
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00022s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

bandit16@bandit:~$ echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | openssl s_client -connect localhost:31790 --quiet
Connecting to 127.0.0.1
Can't use SSL_get_servername
depth=0 CN=SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN=SnakeOil
verify return:1
Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

利用私钥登陆 Level 17，
```sh
cat /etc/bandit_pass/bandit17
```
密码为 `pWXMAZoxGC8JmDMfmT5MGEsobMM3vnj2`

## Level 18
```sh
bandit17@bandit:~$ diff passwords.new passwords.old 
42c42
< OQxXZjELndr90zuhOTDYBEomI0SZITXI
---
> icUh23IUytZLIYhcCaXL18agiSIqymBc
```
密码为 `OQxXZjELndr90zuhOTDYBEomI0SZITXI`

## Level 19
```sh
❯ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t cat readme
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit18@bandit.labs.overthewire.org's password: 
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
Connection to bandit.labs.overthewire.org closed.
```
密码为 `KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI`

## Level 20
```sh
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```
密码为 `4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA`

## Level 21
```sh
❯ echo "4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA" | nc -l 6894 &
[1] 373527 373528
❯ ssh -R 7894:localhost:6894 bandit20@bandit.labs.overthewire.org -p 2220
```

```sh
bandit20@bandit:~$ ./suconnect 7894
Read: 4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
Password matches, sending next password
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```
密码为 `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`

## Level 22
```sh
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz
```
密码为 `RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz`

## Level 23
```sh
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23 
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh 
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

```sh
❯ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

```sh
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw
```

答案为 `gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw`

## Level 24
```sh
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit 
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
```

在临时目录 `/tmp/tmp.QoI9PoOmYr` 创建一个脚本 `steal.sh`：
```sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.QoI9PoOmYr/pass
```

```sh
bandit23@bandit:/tmp/tmp.QoI9PoOmYr$ chmod 777 .
bandit23@bandit:/tmp/tmp.QoI9PoOmYr$ chmod 777 steal.sh
bandit23@bandit:/tmp/tmp.QoI9PoOmYr$ cp steal.sh /var/spool/bandit24/foo
```

等待定时任务触发后，临时目录下的 `pass` 中的字符串即为密码。

密码为 `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv`

## Level 25
```sh
bandit24@bandit:~$ mktemp -d
/tmp/tmp.nyPzsZmdaF
bandit24@bandit:~$ cd /tmp/tmp.nyPzsZmdaF
bandit24@bandit:/tmp/tmp.nyPzsZmdaF$ echo {0000..9999} > pincodes
```

随后执行以下脚本：
```sh
#!/bin/bash

bandit24="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"

for i in $(cat pincodes); do
	echo "$bandit24 $i" >> brute
done
```

得到形如 `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 1573` 的暴力枚举序列。

```sh
bandit24@bandit:/tmp/tmp.nyPzsZmdaF$ cat brute | nc localhost 30002 > response
...
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Correct!
The password of user bandit25 is SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P
```
密码为 `SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P`