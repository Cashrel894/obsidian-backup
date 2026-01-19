#DS100 
**正则表达式** (**RegEx**, Regular Expression)，是一种用于指定搜索样式的记号语言。

## Basic RegEx Syntax
![[Pasted image 20260118161244.png]]
![[Pasted image 20260118161759.png]]
![[Pasted image 20260118162115.png]]

## Greediness
默认情况下，RegEx 进行**贪婪匹配**，即会在字符串中寻找尽可能长的匹配串。

因此，`*?` 的意义就在于不同于 `*`，其进行的是**懒惰匹配**，只会寻找最短的匹配串。

## RegEx in Python and Pandas
### Canonicalization
```python
import re

re.sub(pattern, rep1, text) # Returns text with all instances of pattern replaced by rep1
```

例：
```python
import re

text = "<div><td valign='top'>Moo</td></div>"
pattern = r"<[^>]+>" # 注意r表示raw string，使字符串不转义任何字符
re.sub(pattern, '', text) # 'Moo'
```

在 Pandas 中 regex 的用法也比较类似：
```python
ser.str.replace(pattern, repl, regex=True)
```

```python
data = {"HTML": ["<div><td valign='top'>Moo</td></div>", \
                 "<a href='http://ds100.org'>Link</a>", \
                 "<b>Bold text</b>"]}
html_data = pd.DataFrame(data)

pattern = r"<[^>]+>"
html_data['HTML'].str.replace(pattern, '', regex=True)
```

### Extraction
```python
re.findall(pattern, text) # 返回一个包含所有匹配串的列表
```

```python
text = "My social security number is 123-45-6789 bro, or maybe it’s 321-45-6789."
pattern = r"[0-9]{3}-[0-9]{2}-[0-9]{4}"
re.findall(pattern, text) # ['123-45-6789', '321-45-6789']
```

```python
ser.str.findall(pattern)
```

```python
data = {"SSN": ["987-65-4321", "forty", \
                "123-45-6789 bro or 321-45-6789",
               "999-99-9999"]}
ssn_data = pd.DataFrame(data)

ssn_data["SSN"].str.findall(pattern)
```

```python
pattern_cg = r"([0-9]{3})-([0-9]{2})-([0-9]{4})"
ssn_data["SSN"].str.extract(pattern_cg) # 以DataFrame的形式返回每个group的第一个匹配串
```
![[Pasted image 20260118164125.png]]

```python
ssn_data["SSN"].str.extractall(pattern_cg) # 以Multi-indexed DataFrame的形式返回每个group的所有匹配串
```
![[Pasted image 20260118164307.png]]


## Capture Groups
```python
text = "Observations: 03:04:53 - Horse awakens. \
        03:05:14 - Horse goes back to sleep."

pattern_1 = r"(\d\d):(\d\d):(\d\d)"
re.findall(pattern_1, text) # [('03', '04', '53'), ('03', '05', '14')]
```

##