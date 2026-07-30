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

