---
tags: [Notebooks/Study]
title: CTF相关2
created: '2022-04-19T10:06:25.660Z'
modified: '2022-04-23T12:53:19.038Z'
---

# CTF相关2

**Training: Crypto - Substitution I (Crypto, Training)** 3.06
```
Crypto - Simple Substitution I
Oh dear, I guess you have cracked the two caesar cryptos...
This one is more difficult. Although a simple substitution is easily cracked...
Again the characters are limited to A-Z... But I think I can come up with a 256 version again.

Enjoy!
--
https://www.wechall.net/challenge/training/crypto/simplesub1/index.php

JG DYM KRQEUYDG ULX GLC IKH VMKX DYEA QG FVEMHX E KQ EQNVMAAMX SMVG WMRR XLHM GLCV ALRCDELH PMG EA FHLKIIEHVMMY DYEA REDDRM IYKRRMHUM WKA HLD DLL YKVX WKA ED
```

Solution:
用之前那个caser推测JG对应为BY，https://quipqiup.com/


Result:
FNOACCINREEH

---

**Training: No DNS (Training, Network)** 1
```
Hello Hacker,

In this little training challenge you have to request https://make.love.not.war.com/challenge/training/net/nodns/etc/hosts.php.

I hope this info is enough.

- gizmore

Shouts go out to the wechall admins; dloser, tehron and livinskull :)

--
https://www.wechall.net/challenge/training/net/nodns/index.php
```

Solution:
添加DNS映射即可
首先查询wechall.net的IP地址
在hosts文件中加“5.44.104.158	make.love.not.war.com”
回浏览器访问https://make.love.not.war.com/challenge/training/net/nodns/etc/hosts.php，通过
一定要关代理！！！！！

Result：
同solution


---

**Training: Crypto - Caesar II (Crypto, Training)** 2.96
```
I guess you are done with Caesar I, aren't you?
The big problem with caesar is that it does not allow digits or other characters.
I have fixed this, and now I can use any ascii character in the plaintext.
The keyspace has increased from 26 to 128 too. \o/

Enjoy!
13 3B 3B 30 20 36 3B 2E 78 20 45 3B 41 20 3F 3B
38 42 31 30 20 3B 3A 31 20 39 3B 3E 31 20 2F 34
2D 38 38 31 3A 33 31 20 35 3A 20 45 3B 41 3E 20
36 3B 41 3E 3A 31 45 7A 20 20 34 35 3F 20 3B 3A
31 20 43 2D 3F 20 32 2D 35 3E 38 45 20 31 2D 3F
45 20 40 3B 20 2F 3E 2D 2F 37 7A 20 23 2D 3F 3A
73 40 20 35 40 0B 20 7D 7E 04 20 37 31 45 3F 20
35 3F 20 2D 20 3D 41 35 40 31 20 3F 39 2D 38 38
20 37 31 45 3F 3C 2D 2F 31 78 20 3F 3B 20 35 40
20 3F 34 3B 41 38 30 3A 73 40 20 34 2D 42 31 20
40 2D 37 31 3A 20 45 3B 41 20 40 3B 3B 20 38 3B
3A 33 20 40 3B 20 30 31 2F 3E 45 3C 40 20 40 34
35 3F 20 39 31 3F 3F 2D 33 31 7A 20 23 31 38 38
20 30 3B 3A 31 78 20 45 3B 41 3E 20 3F 3B 38 41
40 35 3B 3A 20 35 3F 20 34 32 3F 3B 34 2D 2D 31
3C 2F 38 39 7A 

--
https://www.wechall.net/challenge/training/crypto/caesar2/index.php
```
Solution:
```python
import sys

def caeser_128_decode(num, ciphertext):
    result = ""
    list = ciphertext.split(" ")
    for code in list:
        tmp = int(code,16) + num
        if tmp > 126:
            tmp = tmp%126 + 32
        
        result += chr(tmp)
    print(result)


if __name__ == "__main__":
    code = input("ciphertext: ")
    for i in range(1,128):
        caeser_128_decode(i,code)
        print('\n')
```

Result:
Good'job,'you'solved'one'more'challenge'in'your'journey.'This'one'was'fairly'easy'to'crack.'Wasn't'it?'128'keys'is'a'quite'small'keyspace,'so'it'shouldn't'have'taken'you'too'long'to'decrypt'this'message.'Well'done,'your'solution'is'pdldmglcfrop.  

---

**Training: Crypto - Digraphs (Crypto, Training)** 4.27
```
This time I am using a digraph crypto scheme to encrypt one letter into two characters.
With only 26 different letters I am able to encrypt up to 26*26 different characters.
The big problem again is sharing the key, but the cipher is easily broken anyway.
The message is in the current language, is written with correct case and punctuation. There are no line breaks.

Good luck!

--
https://www.wechall.net/challenge/training/crypto/digraph/index.php
```
Solution:


Result:



