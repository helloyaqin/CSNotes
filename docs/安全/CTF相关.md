---
attachments: [书安-第二期.pdf, ctf-all-in-one.pdf, ctf-field-guide.pdf]
tags: [Notebooks/Study]
title: CTF相关
created: '2022-04-07T03:26:52.260Z'
modified: '2022-04-19T05:45:45.012Z'
---

# CTF相关

练习网站：
- 推荐：https://www.cnblogs.com/Bubgit/p/9721272.html
- 选用：http://www.wechall.net/challs/by/chall_dif/ASC/page-1

教程：F:\study\book\ctf guide

工具：

密码格式总结：https://cloud.tencent.com/developer/news/175943

## write-up 【第一批】
**Traning : Get Sourced** 0.47
```
The solution is hidden in this page
http://www.wechall.net/challenge/training/get_sourced/index.php
Use View Sourcecode to get it
```
Solution:
<kdd>C\^u</kdd>查看网页源代码，一般藏在注释中，<kdd>C\^f</kdd>搜索`<!`
找到代码段

Result:
```html
<!-- You are looking for this password: html_sourcecode --> 
```

---

**Training : Ascii** 0.55
```
In a computer, you can only work with numbers.
In this challenge you have to decode the following message, which is in ASCII.

84, 104, 101, 32, 115, 111, 108, 117, 116, 105, 111, 110, 32, 105, 115, 58, 32, 105, 99, 114, 112, 105, 97, 101, 100, 100, 99, 111, 112

--
http://www.wechall.net/challenge/training/encodings/ascii/index.php
```
Solution:
```python
import sys
from tkinter import N

def decodeAscii(number):
    return chr(int(number))

if __name__ == "__main__":
    tmp = input("输入字符，以逗号隔开: ")
    numbers = tmp.split(',')
    for number in numbers:
        print(decodeAscii(number), end="")
```
Result:
```python
# The solution is: icrpiaeddcop
```
---

**2021 Christmas Grampa** 0.67
```
Santa speaks many languages and is receiving a lot of letters this year.
But he gets old!

The problem is that he will need glasses.
Do you need glasses as well?

spaceone told us you can help!

--
http://www.wechall.net/challenge/christmas2021/grampa/index.php
```
Solution:
点进连接，solution.php中有代码
```php
return sha1("あなたはそれを見つけることができません“ . “الحل خارج نطاقك");
```
将`あなたはそれを見つけることができません“ . “الحل خارج نطاقك`的加密结果填入，即通过
（但不知道是不是正确做法，因为没有找到从challenge页面获取solution.php的方法）

Result:
b33815d66363f7bf3aaa9223e22aa0bc66a5c05c


---

**Training: Stegano I**  1.08
```
This is the most basic image stegano I can think of.

--
https://www.wechall.net/challenge/training/stegano1/index.php
```

Solution:
放到WSL/Linux系统中，用cat或者vim打开即可看见flag
因为说了是最简单的，所以没做其他的隐写处理

Result:
```
Look what the hex-edit revealed: passwd:steganoI
```

Reference：https://fareedfauzi.gitbook.io/ctf-checklist-for-beginner/steganography

---

**Encodings: URL (Training, Encoding)** 0.81
```
Your task is to decode the following:

%59%69%70%70%65%68%21%20%59%6F%75%72%20%55%52%4C%20%69%73%20%63%68%61%6C%6C%65%6E%67%65%2F%74%72%61%69%6E%69%6E%67%2F%65%6E%63%6F%64%69%6E%67%73%2F%75%72%6C%2F%73%61%77%5F%6C%6F%74%69%6F%6E%2E%70%68%70%3F%70%3D%65%64%73%6C%67%61%70%66%6D%63%70%62%26%63%69%64%3D%35%32%23%70%61%73%73%77%6F%72%64%3D%66%69%62%72%65%5F%6F%70%74%69%63%73%20%56%65%72%79%20%77%65%6C%6C%20%64%6F%6E%65%21

--
https://www.wechall.net/challenge/training/encodings/url/index.php
```
Solution:
URL编码转码，网上随便找个小工具，

Result：
解码结果：`Yippeh! Your URL is challenge/training/encodings/url/saw_lotion.php?p=edslgapfmcpb&cid=52#password=fibre_optics Very well done!`
把结果给的网址和wecall的网址拼合一下并访问：`https://www.wechall.net/challenge/training/encodings/url/saw_lotion.php?p=edslgapfmcpb&cid=52#password=fibre_optics`，过了

---

**Training: Crypto - Caesar I (Crypto, Training)** 1.2
```
As on most challenge sites, there are some beginner cryptos, and often you get started with the good old caesar cipher.
I welcome you to the WeChall style of these training challenges :)

Enjoy!

SGD PTHBJ AQNVM ENW ITLOR NUDQ SGD KZYX CNF NE BZDRZQ ZMC XNTQ TMHPTD RNKTSHNM HR ONLRMZRDECGC

--
https://www.wechall.net/challenge/training/crypto/caesar/index.php
```
Solution:
凯撒密码，暴力一点的试错就行了，偏移6位解密时，结果为：MAX JNBVD UKHPG YHQ CNFIL HOXK MAX ETSR WHZ HY VTXLTK TGW RHNK NGBJNX LHENMBHG BL IHFLGTLXYWAW
但是只有MAX对的上，其他的单词还是对不上，出于巧合，点中了偏移7位加密，此时结果为：THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG OF CAESAR AND YOUR UNIQUE SOLUTION IS POMSNASEFDHD
得到flag（每次随机出来的密文不一样，但递归解密肯定能做）

尝试写了python脚本：
```python
import sys

def caeser_decode(p,num):
    result = ""
    base = "A"
    for n in num:
        if n == " ":
            result += n
        else:
            result += chr((ord(n)-p + ord(base))%26 + ord(base))
    return result

if __name__ == "__main__":
    num = input("输入密文：")
    print("爆破中，，")
    for p in range(1,26):
        print(caeser_decode(p,num))
```
但是该脚本只针对了大写的凯撒，需要加以修改，同时在![网上](https://www.starky.ltd/2020/08/05/python-cryptography-caesar-cipher/)找到了用字母表的方法实现凯撒爆破的，值得学习，只不过没有跳过空格，有点可惜
```python
symbols = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890 !?.'

ciphertext = input("Please input ciphertext:\n")


for key in range(len(symbols)):
    ciphers = symbols[key:] + symbols[:key] # 字母表
    transtab = str.maketrans(ciphers, symbols)
    plaintext = ciphertext.translate(transtab)

    print(f'Key #{key}: {plaintext}')
```


Result：
THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG OF CAESAR AND YOUR UNIQUE SOLUTION IS POMSNASEFDHD

---

**Training: WWW-Robots (HTTP, Training)** 1.22

```
In this little training challenge, you are going to learn about the Robots_exclusion_standard.
The robots.txt file is used by web crawlers to check if they are allowed to crawl and index your website or only parts of it.
Sometimes these files reveal the directory structure instead protecting the content from being crawled.

Enjoy!
```

Solution:
robots.txt: 是一种存放于网站根目录下的ASCII编码的文本文件，它通常告诉网络搜索引擎的漫游器（又称网络蜘蛛），此网站中的哪些内容是不应被搜索引擎的漫游器获取的，哪些是可以被漫游器获取的。因为一些系统中的URL是大小写敏感的，所以robots.txt的文件名应统一为小写。robots.txt应放置于网站的根目录下。如果想单独定义搜索引擎的漫游器访问子目录时的行为，那么可以将自定的设置合并到根目录下的robots.txt，或者使用robots元数据（Metadata，又称元数据）。

比如，正好前阵日子被老师问到我知网论文数据怎么爬的，知网允不允许爬虫
我爬取的网站https://search.cnki.com.cn/，他的robots.txt为：
```
user-agent:*
allow:/
```
即允许所有的机器人爬取任意信息，

而正常我们访问的知网页面https://www.cnki.net/，没有robots.txt，搜索时会跳转到网址https://kns.cnki.net/，他的robots.txt为：
```
User-agent: *
Disallow: /
```
即不允许任何机器人爬取信息，故知网可能真的不允许爬虫，但search的那个页面依旧开放。

回到本题，思路如下：
https://www.wechall.net/robots.txt
看见disallow档案

Result:
https://www.wechall.net/challenge/training/www/robots/T0PS3CR3T/

---

**PHP-0817** 1.61
```
I have written another include system for my dynamic webpages, but it seems to be vulnerable to LFI.
Here is the code:
GeSHi`ed PHP code
<?php
if (isset($_GET['which']))
{
        $which = $_GET['which'];
        switch ($which)
        {
        case 0:
        case 1:
        case 2:
                require_once $which.'.php';
                break;
        default:
                echo GWF_HTML::error('PHP-0817', 'Hacker NoNoNo!', false);
                break;
        }
}
?>
Your mission is to include solution.php.
Here is the script in action: News, Forum, Guestbook.

Good Luck!
--
https://www.wechall.net/challenge/php0817/index.php
```

Solution:
LFI: Local File inclusion
PHP代码为判断which的case语句，其中，which==0/1/2/时会执行$which.'.php'，其他值时，会报错误信息
初步理解，将which=solution，是不是就能通过index.php执行solution.php了
结果确实是成功了，可为什么which=solution时通过了which==0/1/2的判断呢？？？
- 当一个非数字开头的字符串与数字0进行==比较时，结果总是true！

Result:
https://www.wechall.net/challenge/php0817/index.php?which=solution

---

**Training: PHP LFI (Exploit, PHP, Training)** 2.9
```
Your mission is to exploit this code, which has obviously an LFI vulnerability:

GeSHi`ed PHP code
$filename = 'pages/'.(isset($_GET["file"])?$_GET["file"]:"welcome").'.html';
include $filename;


There is a lot of important stuff in ../solution.php, so please include and execute this file for us.
...
--
https://www.wechall.net/challenge/training/php/lfi/up/index.php
```
Solution:
趁热打铁，看见file，明白是用路径进行LFI攻击，但是一直访问不进去
后来看攻略发现报错里显示的是
```
PHP Warning(2): include(pages/../../solution.php.html): failed to open stream: No such file or directory in /home/wechall/www/wc5/www/challenge/training/php/lfi/up/index.php(54) : eval()'d code line 1

PHP Warning(2): include(): Failed opening 'pages/../../solution.php.html' for inclusion
```
发现多了个.html，用%00截断，通过

Result:
https://www.wechall.net/challenge/training/php/lfi/up/index.php?file=../../solution.php%00


---

**Prime Factory (Training, Math)** 1.98
```
Your task is simple:
Find the first two primes above 1 million, whose separate digit sums are also prime.
As example take 23, which is a prime whose digit sum, 5, is also prime.
The solution is the concatination of the two numbers,
Example: If the first number is 1,234,567
and the second is 8,765,432,
your solution is 12345678765432
```

Solution:
```python
import sys

def is_prime(num):
    for i in range(2,num):
        if num % i == 0 and num != 2:
            return False
    return True

if __name__ == "__main__":
    print("start programming...")
    list = []
    times = 0
    vars = 1000000

    while times < 2:
        if is_prime(vars):
            tmp = sum(map(int, str(vars)))
            if is_prime(tmp):
                list.append(vars)
                times += 1
        vars += 1
        if vars > 9999999:
            break
    print(list)
```

Result:
10000331000037

---

**Training: MySQL I (MySQL, Exploit, Training)** 2.16
```
This one is the classic mysql injection challenge.
Your mission is easy: Login yourself as admin.
Again you are given the sourcecode, also as highlighted version.

Enjoy!
--
https://www.wechall.net/challenge/training/mysql/auth_bypass1/index.php
```

Solution:
SQL注入，对应源码：
```SQL
function auth1_onLogin(WC_Challenge $chall, $username, $password)
{
	$db = auth1_db();
	
	$password = md5($password);
	
	$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";    # Attention！！
	
	if (false === ($result = $db->queryFirst($query))) {      # Attention！！
		echo GWF_HTML::error('Auth1', $chall->lang('err_unknown'), false); # Unknown user
		return false;
	}

	# Welcome back!
	echo GWF_HTML::message('Auth1', $chall->lang('msg_welcome_back', htmlspecialchars($result['username'])), false);
	
	# Challenge solved?
	if (strtolower($result['username']) === 'admin') {      # Attention！！
		$chall->onChallengeSolved(GWF_Session::getUserID());
	}
	
	return true;
}
```
即需要满足的条件：
1. query能返回结果
2. 用户名为admin
3. 突破口在\$query的$username上

Result:
username: admin' or '1'='1
passowrd: *

---

**Training: Encodings I (Training, Encoding)** 3.35
```
We intercepted this message from one challenger to another, maybe you can find out what they were talking about.
To help you on your progress I coded a small java application, called JPK.
Note: The message is most likely in english.

10101001101000110100111100110100
00011101001100101111100011101000
10000011010011110011010000001101
11010110111000101101001111010001
00000110010111011101100011110111
11100100110010111001000100000110
00011110011110001111010011101001
01011100100000101100111011111110
10111100100100000111000011000011
11001111100111110111110111111100
10110010001000001101001111001101
00000110010111000011110011111100
11110011111010011000011110010111
0100110010111100100101110

--
https://www.wechall.net/challenge/training/encodings1/index.php
```
Solution:
下载JDK，运行
由于是0/1，判断为是Binary二进制，将其转换为ascii
**ASCII 码使用指定的7 位或8 位二进制数组合来表示128 或256 种可能的字符。**
默认bitsperblock是8，Binary Format之后多一位，改成7
再Binary to Ascii得到结果

Result：
This text is 7-bit encoded ascii. Your password is easystarter


---

**Training: Programming 1 (Training, Coding)**
```
When you visit this link you receive a message.
Submit the same message back to https://www.wechall.net/challenge/training/programming1/index.php?answer=the_message
Your timelimit is 1.337 seconds

--
https://www.wechall.net/challenge/training/programming1/index.php
```
Solution:
爬虫，fq需要把小火箭停掉！保证IP地址一致，否则会报错Please login by sending your cookies in the HTTP Header（我一开始还以为是我的问题，找了两小时的错= = ）
```
import requests

url1 = 'http://www.wechall.net/challenge/training/programming1/index.php?action=request'
url2 = 'http://www.wechall.net/challenge/training/programming1/index.php?answer='

header = {'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.88 Safari/537.36'}

cookie = dict(WC='16560453-62765-MFd1Gts6fM63vVbV')

re = requests.get(url1,headers=header,cookies=cookie)
print(re.status_code)
key = re.text
print(key)

url2 = url2 + key

re = requests.get(url2,headers=header,cookies=cookie)
print(re.text)
```

Result:
运行程序即可

---

**Zebra (Training, Encoding, Stegano)** 2.25
```
Help! A zebra escaped from its enclosure. But where is it now?

--
https://www.wechall.net/challenge/Hirsch/Zebra/index.php
```
Solution:
斑马上的条纹是条形码，用读取条形码的网站识别一下就行
条形码识别：https://online-barcode-reader.inliteresearch.com/


Result:
The answer is saFFari

---

**Training: Crypto - Transposition I (Crypto, Training)**  2.42
```
It seems that the simple substitution ciphers are too easy for you.
From my own experience I can tell that transposition ciphers are more difficult to attack.
However, in this training challenge you should have not much problems to reveal the plaintext.

oWdnreuf.lY uoc nar ae dht eemssga eaw yebttrew eh nht eelttre sra enic roertco drre . Ihtni koy uowlu dilekt  oes eoyrup sawsro don:wp soceiahbcp.h
--
https://www.wechall.net/challenge/training/crypto/transposition1/index.php
```

Solution:
第一个单词wonderful，所以是两个字符反转，写程序就行
```python
import sys

def reverse_code(code):
    return code[::-1]

def decode_Fun(en, num):
    tmp = 0
    result = ''
    while tmp <= len(en):
        reverse = en[tmp:tmp+num]
        result += reverse_code(reverse)
        tmp = tmp + num
    reverse = en[tmp-num:-1]
    result += reverse_code(reverse)
    print(result)
    return result

if __name__ == "__main__":
    # encrypt_code = input()
    encrypt_code = 'oWdnreuf.lY uoc nar ae dht eemssga eaw yebttrew eh nht eelttre sra enic roertco drre . Ihtni koy uowlu dilekt  oes eoyrup sawsro don:wp soceiahbcp.h'
    decode_Fun(encrypt_code, 2)
```

Result:
Wonderful. You can read the message way better when the letters are in correct order. I think you would like to see your password now: posecaibhpch.

---

**Training: Regex (Training, Regex)** 4.18
```
submit the regular expression the matches an empty string, and only an empty string.

submit a regular expression that matches only the string 'wechall' without quotes.

submit an expression that matches valid filenames for certain images.Your pattern shall match all images with the name wechall.ext or wechall4.ext and a valid image extension.Valid image extensions are .jpg, .gif, .tiff, .bmp and .png.

capture the filename, without extension, too
--
https://www.wechall.net/challenge/training/regex/index.php
```

Solution:
正则表达式去菜鸟看一下就行

这里看一下非捕获性匹配是啥：例如，要在一篇英文资料中查找"program"和"project"两个单词，正则表达式可表示为/program|project/,也可表示为/pro(gram|ject)/，但是缓存子匹配(gramject)没有意义，就可以用/pro(?:gram|ject)/进行非捕获性匹配这样既可以简洁匹配又可不缓存无实际意义的字匹配。


Result:
/^$/
/^wechall$/
/^wechall4?\.(?:tiff|jpg|gif|bmp|png)$/
/^(wechall4?)\.(?:tiff|jpg|gif|bmp|png)$/

---

**hi (Math)** 2.39
```
Hi, imagine this situation.
There is an IRC channel #wechall on irc.wechall.net.

The server sends the messages to all people in the channel, also back to the sender himself.
When every minute one person joins and says hi,
how many "hi" messages were totally sent for this channel after 0xfffbadc0ded minutes ?
No one ever leaves the channel, so there are 0xfffbadc0ded people at the end ;)

Further explanation for 3 minutes:
the channel is empty and there have been sent 0 messages 1st person joins, sends hi, the server sends hi back to 1 persons.
2nd person joins, sends hi, the server sends hi back to 2 persons.
3rd person joins, sends hi, the server sends hi back to 3 persons.

Minute 1: 2 messages sent
Minute 2: 3 messages sent
Minute 3: 4 messages sent
Adding these up means for 3 minutes are 9 messages sent.

Conversion Notes: 0xfffbadc0ded is hexadecimal which converts to 17.591.026.060.781 (Thats around 20 trillion minutes).Please submit your solution in the decimal system.

--
https://www.wechall.net/challenge/hi/index.php
```

Solution:
大数等差数列，懒得编程，用免费在线智能计算引擎 https://www.wolframalpha.com/

Result:
154722098935564539692256152

---

**Stegano Attachment (Stegano, Image, Training)** 2.55
```
Hello challenger,

You got mail and a nice attachment.
Your unique solution which is bound to your session is in there too.

Enjoy!
--
https://www.wechall.net/challenge/training/stegano/attachment/index.php
```

Solution:
放stegsolve里Format一下，ascii出现了solution.txt字样，后面跟的字符串就是个人的flag

Result:
每个人不一样
