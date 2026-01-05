## Basic Bash Commands
```bash
date
pwd
ls
echo
man
cd
mkdir
touch
rm
cp
mv
cat
grep
chmod
sudo
df
history
ps

```

## Create and Execute Bash Scripts
一般而言，bash 脚本都有拓展名 `.sh`，但并非必要。

而必要的是在脚本开头添加 ***shebang***，即以 `#!` 开头的语句，用于指定执行这一脚本的 shell。
```bash
#!/bin/bash
#!/bin/zsh
```

编写完脚本后，为脚本文件赋予运行权限：
```bash
chmod u+x run_all.sh 
```

接着便可以运行：
```bash
./run_all.sh
bash ./run_all.sh
sh ./run_all.sh
```

## Bash Scripting Basics
## Comments
```bash
# This is a comment
```

### Variables and Data types
在 Bash 中，变量没有数据类型，可以是数值、字符或是字符串等各种类型。

设置一个变量：
```bash
x=1
```

访问一个现有变量：
```bash
y=$x
```

变量命名规则与 C 语言一致。

### Input and Output
### Inputs
1. Reads the user inputs
```bash
#!/bin/bash 

echo "What's your name?" 

read entered_name 

echo -e "\nWelcome to bash tutorial" $entered_name # -e flag allows interpretation of special characters like '\n'
```

2. Reads from a file
```bash
while read line
do
  echo $line
done < input.txt
```

3. Reads from command line arguments
```bash
$0 # 脚本名
$1 # 第1个参数
$2 # 第2个参数
$# # 参数个数
$@ # 所有参数（每个参数相互独立）
$* # 所有参数（合并为一个数据）
```

### Outputs
1. Printing to the terminal
```bash
echo "Hello, World!"
```

2. Writing to a file
```bash
echo "This is some text." > output.txt
```

3. Appending to a file
```bash
echo "More text." >> output.txt
```

4. Redirecting output
```bash
ls > files.txt
```

## Conditional statements
## if ... elif .. else
```bash
if [[ condition ]];
then
    statement
elif [[ condition ]]; then
    statement 
else
    do this by default
fi
```

```bash
# -gt: Greater than
# -lt: Less than
# -a: And
# -o: Or
if [ $a -gt 60 -a $b -lt 100 ]
```

```bash
#!/bin/bash

echo "Please enter a number: "
read num

if [ $num -gt 0 ]; then
  echo "$num is positive"
elif [ $num -lt 0 ]; then
  echo "$num is negative"
else
  echo "$num is zero"
fi
```

### Case statements
```bash
case expression in
    pattern1)
        # code to execute if expression matches pattern1
        ;;
    pattern2)
        # code to execute if expression matches pattern2
        ;;
    pattern3)
        # code to execute if expression matches pattern3
        ;;
    *)
        # code to execute if none of the above patterns match expression
        ;;
esac
```
## Looping
### While loop
```bash
#!/bin/bash
i=1
while [[ $i -le 10 ]] ; do
   echo "$i"
  (( i += 1 ))
done
```

### For loop
```bash
#!/bin/bash

for i in {1..5}
do
    echo $i
done
```

## Schedule Scripts
***Cron*** 是一个用于工作调度的强大工具，语法如下：
```bash
# Cron job example
* * * * * sh /path/to/script.sh
```
这些星号分别代表分、时、日、月、星期几，用于指定脚本执行的时间或间隔。
![[Pasted image 20260105201215.png]]

```bash
crontab -l # 显示当前已安排的任务
crontab -e # 添加和编辑任务
```

## Debugging
开启 Debug 模式：
```shell 
set -x
```
开启后，Bash 会将执行的所有指令都打印出来，用于检查错误。

此外，还可以用 `$?` 检查最后一条指令的返回代码，若结果为 `0`，则指令运行成功，否则执行异常。
```bash
#!/bin/bash

# Your script goes here

if [ $? -ne 0 ]; then
    echo "Error occurred."
fi
```

`echo` 指令也很常用，用于打印中间变量。
```bash
#!/bin/bash

# Your script goes here

echo "Value of variable x is: $x"

# More code goes here
```

也可以 `set -e`，使得当任何指令出现异常时，脚本都会立即停止，中断后续执行。

`cron` 任务的日志通常在 `/var/log/syslog` 中，出现问题时可以查看。

