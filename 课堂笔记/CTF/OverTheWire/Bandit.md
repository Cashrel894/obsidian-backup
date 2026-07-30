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
/tmp/tmp.CsPOtwmHVw
bandit12@bandit:/tmp/tmp.CsPOtwmHVw$ cp ~/data.txt .
bandit12@bandit:/tmp/tmp.CsPOtwmHVw$ cat data.txt
00000000: 1f8b 0808 b2f0 3b6a 0203 6461 7461 322e  ......;j..data2.
...
```
这里 `1f8b` 是 `gzip` 压缩文件的魔数，应该先反 `hexdump` 后解压缩。
```sh
bandit12@bandit:/tmp/tmp.CsPOtwmHVw$ xxd -r data.txt | gzip -d | xxd    
00000000: 425a 6839 3141 5926 5359 dc09 6683 0000  BZh91AY&SY..f...
...
```
解压缩出来又是一个二进制文件，`xxd` 后发现开头为 `425a`，是 `bzip2` 压缩文件的魔数。
```sh
bandit12@bandit:/tmp/tmp.CsPOtwmHVw$ xxd -r data.txt | gzip -d > gz
bandit12@bandit:/tmp/tmp.CsPOtwmHVw$ cat gz | bzip2 -d | xxd
00000000: 1f8b 0808 b2f0 3b6a 0203 6461 7461 342e  ......;j..data4.
...
```
依旧是 `gzip` 魔数。
```sh

```