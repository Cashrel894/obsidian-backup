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