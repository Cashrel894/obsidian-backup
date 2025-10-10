基本结构：\[command] \[flags] \[arguments]

通过 "man (command)"获取该指令的使用手册（**man**ual）

常见 Shell 指令：
cd 
ls 用-a 参数显示所有文件（包括隐藏）；用-l 显示文件详细信息。
cat 将多个文件首尾相连并输出。
head 读取文件的前 10 行。 
less 以 Vim 形式读取文件。
mv
cp
rm
file 识别文件类型
grep 查找文件中符合指定模式的行。用-r 参数可递归搜索文件夹。
wget 下载指定 URL 的文件。
pwd
chmod 更改文件权限。权限分为三部分：所有者（u），组用户（g），其他人（o）。每部分的权限值：r=4（可读），w=2（可写），e=1（可执行）。使用例：![[../../附件/Pasted image 20250731091852.png]]