python changle issue
===
提示，查看前一关解决方案将url中的pc->pcc

    要解决这里面的问题需要有一下蛋疼的精神
        1. 随时查看源代码
        2. 熟悉英语，例如里面给个图是个拉链要坑爹的联想到zip
        3. 熟悉python模块，例如有peak提示让你发音,
           熟悉python的人才知道pickle

0
---
    http://www.pythonchallenge.com/pc/def/0.html
    这个题没什么说的 套入计算即可
    2 ** 38 = 274877906944

    Hint: try to change the URL address
    -->
    http://www.pythonchallenge.com/pc/def/274877906944.html

1
---
   everybody thinks twice before solving this.
   thick twice，如果直接按照这三个字母对应的东西解密的话接出来的依然是
   乱码,回头一想找到规律，每个小写字母移动了两位，这就是think twice

   k -> m
   o -> q
   e -> g
   给出的提示又都是小写
   可以看出规律，
   s = '''g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq
   ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr
   gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc
   spj.'''
   import string
   from = string.ascii_lowercase
   to = from[2:] + from[:2]
   table = string.maketrans(from, to)
   print string.translate(s, table)
   -->
   Out[21]: "i hope you didnt translate it by hand. thats what computers
   are for. doing it in by hand is inefficient and that's why this text
   is so long. using string.maketrans() is recommended. now apply on the
   url."

   In [22]: string.translate('map', table)
   Out[22]: 'ocr'

   http://www.pythonchallenge.com/pc/def/ocr.html

2
---
    recognize the characters. maybe they are in the book, 
    but MAYBE they are in the page source.
    page source不是书上的玩意，是源代码
    源代码里面有很大一段的注释

    hint:find rare characters in the mess below:

    from collections import Counter

    In [55]: Counter(s)
    Out[55]: Counter({')': 6186, '@': 6157, '(': 6154, ']': 6152, '#':
    6115, '_': 6112, '[': 6108, '}': 6105, '%': 6104, '!': 6079, '+':
    6066, '$': 6046, '{': 6046, '&': 6043, '*': 6034, '^': 6030, '\n':
    1219, 'a': 1, 'e': 1, 'i': 1, 'l': 1, 'q': 1, 'u': 1, 't': 1, 'y':
    1})

    rare --> aeilquty --> equality
    http://www.pythonchallenge.com/pc/def/equality.html

3
---
    标题是re，明显是正则,hit又提示一个小写字母在三个大写中间
    继续看源码，里面又有一堆注释
    import re
    p = re.compile('[^A-Z][A-Z]{3}[a-z][A-Z]{3}[^A-Z]')
    m = p.search(s)
    res = p.findall(s)
    text = ''.join(t[4] for t in res)
    -->
    linkedlist

    http://www.pythonchallenge.com/pc/def/linkedlist.html
    http://www.pythonchallenge.com/pc/def/linkedlist.php

4
---
    linkedlist 居然是一个也面一个页面的找，输入俩页面之后会发现提示你
    是不是手累了，确实需要写个脚本的，不过里面往下走还有坑。

    import requests
    import re
    p = re.compile('\d+')
    url = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing='
    next_num = '12345'
    with open('pych', 'aw') as f:
        while 1:
            r = requests.get(url + next_num)
            f.write(r.text)
            next_num = p.search(r.text).group()
            print r.text
            print url + next_num

    坑
            16044
            Yes. Divide by two and keep going.
            82682
            There maybe misleading numbers in the text. One example is 82683.
            Look only for the next nothing and the next nothing is 63579
            82683
            You've been misleaded to here. Go to previous one and check.
            66831
            peak.html

    http://www.pythonchallenge.com/pc/def/peak.html

5
---
    hit:pronounce it --> pickle
    源码里面有个链接，banner.p打开一看是被pickle过的东西，
    解压出来的东西比接特别,十一个二维数组，看一眼每一行
    数字加和为95，很自然的想到把这玩儿打印出来看看。

    import requests
    import pickle
    url = 'http://www.pythonchallenge.com/pc/def/banner.p'
    r = requests.get(url)
    text = pickle.loads(r.text)
    with open('f', 'wa') as f:
        for line in text:
            for symbol, num in line:
                f.write(symbol * int(num))
            f.write('\n')

    打开文件一看是channel
                                                                                                   
                  #####                                                                      ##### 
                   ####                                                                       #### 
                   ####                                                                       #### 
                   ####                                                                       #### 
                   ####                                                                       #### 
                   ####                                                                       #### 
                   ####                                                                       #### 
                   ####                                                                       #### 
          ###      ####   ###         ###       #####   ###    #####   ###          ###       #### 
       ###   ##    #### #######     ##  ###      #### #######   #### #######     ###  ###     #### 
      ###     ###  #####    ####   ###   ####    #####    ####  #####    ####   ###     ###   #### 
     ###           ####     ####   ###    ###    ####     ####  ####     ####  ###      ####  #### 
     ###           ####     ####          ###    ####     ####  ####     ####  ###       ###  #### 
    ####           ####     ####     ##   ###    ####     ####  ####     #### ####       ###  #### 
    ####           ####     ####   ##########    ####     ####  ####     #### ##############  #### 
    ####           ####     ####  ###    ####    ####     ####  ####     #### ####            #### 
    ####           ####     #### ####     ###    ####     ####  ####     #### ####            #### 
     ###           ####     #### ####     ###    ####     ####  ####     ####  ###            #### 
      ###      ##  ####     ####  ###    ####    ####     ####  ####     ####   ###      ##   #### 
       ###    ##   ####     ####   ###########   ####     ####  ####     ####    ###    ##    #### 
          ###     ######    #####    ##    #### ######    ###########    #####      ###      ######
                                                                                                   

    http://www.pythonchallenge.com/pc/def/channel.html

6
---
    打开一看是拉链 --> zip
    然后看标题是channel, python面貌似没有channel + zip有关的东西啊
    源代码里有一段捐赠的注释，想了好久实在没有办法
    http://www.pythonchallenge.com/pc/def/channel.zip
    google下发现居然把url后缀改为.zip
    下载下来解压出来就是,linkedlist里面的那些东西
    有个readme
        welcome to my zipped list.

        hint1: start from 90052
        hint2: answer is inside the zip
    应该用python的zipfile
    看了下里面有个ZipFile, ZipInfo
    试了下用ZipFile channel.zip后有个 NameToInfo
    显示了一个dict = {
        '99254.txt': <zipfile.ZipInfo at 0x374b050>,
        '99460.txt': <zipfile.ZipInfo at 0x374b118>,
        ...
    }
    去了90052.txt查看了下，属性里面有个comment，貌似这里会有信息
    是个'*'

    import zipfile
    import re

    p = re.compile('\d+')
    next = '90052'
    suffix = '.txt'
    z = zipfile.ZipFile('channel.zip')
    infos = z.NameToInfo
    result = []
    try:
        while 1:
            next_file = next + suffix
            comment = infos[next_file].comment
            result.append(comment)
            print next_file, comment
            with z.open(next_file) as zf:
                next = p.search(zf.read()).group()
    except:
        with open('out', 'wa') as out:
            out.write(''.join(result))

    打开文件查看是
    hockey
    http://www.pythonchallenge.com/pc/def/hockey.html

    it's in the air. look at the letters.

    在天上，重新组合这个单词么？
    妹的，英语不好行了吧，用python的全排列看看
    from itertools import permutations
    with open('word', 'wa') as f:
    f.write('\n'.join(''.join(word) for word in permutations('HCOKEY')))

    我甚至找了这俩网站来查字母
    http://www.scrabblefinder.com/word/
    http://www.allscrabblewords.com/unscramble/

    我擦，妹的，最后居然不是,而是用里面的字母oxygey

    ****************************************************************
    ****************************************************************
    **                                                            **  
    **   OO    OO    XX      YYYY    GG    GG  EEEEEE NN      NN  **  
    **   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE  NN    NN   **  
    **   OO    OO XXX  XXX YYY   YY  GG GG     EE       NN  NN    **  
    **   OOOOOOOO XX    XX YY        GGG       EEEEE     NNNN     **  
    **   OOOOOOOO XX    XX YY        GGG       EEEEE      NN      **  
    **   OO    OO XXX  XXX YYY   YY  GG GG     EE         NN      **  
    **   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE     NN      **  
    **   OO    OO    XX      YYYY    GG    GG  EEEEEE     NN      **  
    **                                                            **  
    ****************************************************************
     **************************************************************

    http://www.pythonchallenge.com/pc/def/oxygen.html

7
---
    这一关是用pil
    学了下pil的库,不过不想做这个题怎么办
    http://danmcewan.com/python-challenge-7
    http://holger.thoelking.name/python-challenge/7

    http://www.pythonchallenge.com/pc/def/integrity.html

8
---
    看了下源代码，里面有个链接，打开需要用户名密码，
    源代码下面有用户名密码的东西，不过貌似是加密的，
    不过都有BZh91AY&SY这个东西，应该是某种算法的格式
    google了下原来是python的bz2模块

    import bz2
    un = 'BZh91AY&SYA\xaf\x82\r\x00\x00\x01\x01\x80\x02\xc0\x02\x00 \x00!\x9ah3M\x07<]\xc9\x14\xe1BA\x06\xbe\x084'
    pw = 'BZh91AY&SY\x94$|\x0e\x00\x00\x00\x81\x00\x03$ \x00!\x9ah3M\x13<]\xc9\x14\xe1BBP\x91\xf08'
    un = bz2.decompress(un)
    pw = bz2.decompress(pw)
    print un, pw --> huge file
    登录进去之后是
    http://www.pythonchallenge.com/pc/return/good.html

9
---
    依然是pil的题目
    http://holger.thoelking.name/python-challenge/9

    http://www.pythonchallenge.com/pc/return/bull.html

10
---
    读出来的数字

    1 11 21

    1个1 --> 11
    两个1 --> 21
    一个2一个1 --> 1211

    ch n
    [(n,ch),....]

    from itertools import groupby

    next = '1'
    result = ['1']
    for i in xrange(30):
        _generator = groupby(next)
        stat = [str(len(list(_iter))) + key for key, _iter in _generator]
        next = ''.join(stat)
        result.append(next)
    print len(result[30])

    http://www.pythonchallenge.com/pc/return/5808.html

11
---
    import Image

    # download the image from: http://www.pythonchallenge.com/pc/return/cave.jpg

    image = Image.open("cave.jpg")
    nsize = tuple([x / 2 for x in image.size])
    odd = even = Image.new(image.mode, nsize)

    for x in range(image.size[0]):
        for y in range(image.size[1]):
            if x % 2 == 0 and y % 2 == 0:
                even.putpixel((x / 2, y / 2), image.getpixel((x, y)))
            elif x % 2 == 0 and y % 2 == 1:
                odd.putpixel((x / 2, (y - 1) / 2), image.getpixel((x, y)))
            elif x % 2 == 1 and y % 2 == 0:
                even.putpixel(((x - 1) / 2, y / 2), image.getpixel((x, y)))
            else:
                odd.putpixel(((x - 1) / 2, (y - 1) / 2), image.getpixel((x, y)))

    even.save("even.jpg")
    odd.save("odd.jpg")

    http://www.pythonchallenge.com/pc/return/evil.html

12
---

    查看源代码，发现里面图片为evil1.jpg说明还有2,3,4
    第二张里面说not jpg --> gfx,将url后缀改为gfx

    下面是网上的解决方案，不知道为毛是5
    evil = open('evil2.gfx','r').read()
    img1 = open('img1.jpg','w')
    img2 = open('img2.jpg','w')
    img3 = open('img3.jpg','w')
    img4 = open('img4.jpg','w')
    img5 = open('img5.jpg','w')

    for b in range(0,len(evil),5):
        img1.write(evil[b])
        img2.write(evil[b+1])
        img3.write(evil[b+2])
        img4.write(evil[b+3])
        img5.write(evil[b+4])

    img1.close()
    img2.close()
    img3.close()
    img4.close()
    img5.close()

    图片需要用火狐打开
    dis pro port ional ity
    disproportionality
    ity被一条线画掉了
    disproportional

13
---
    #! /usr/bin/env python
    '''python challenge level 13
    question url: http://www.pythonchallenge.com/pc/return/disproportional.html
    answer url: http://www.pythonchallenge.com/pcc/return/italy.html
    '''

    import xmlrpclib
    xmlrpc = xmlrpclib.ServerProxy("http://www.pythonchallenge.com/pc/phonebook.php")
    print xmlrpc.system.listMethods()
    print xmlrpc.system.methodHelp('phone')
    print xmlrpc.phone('Bert')

    http://www.pythonchallenge.com/pc/return/italy.html

14
---
    import Image, math

    def get_pixel(t):
        t = (100 * 100 - 1) - t
        shell = int((math.sqrt(t) + 1) / 2)
        leg = 0 if (shell == 0) else int((t - (2 * shell - 1) ** 2) / 2 / shell)
        elt = t - (2 * shell - 1) ** 2 - 2 * shell * leg - shell + 1

        if leg == 0:
            x, y = shell, elt
        elif leg == 1:
            x, y = -elt, shell
        elif leg == 2:
            x, y = -shell, -elt
        else:
            x, y = elt, -shell

        return (49 + x, 50 - y)

    # Download the image from: http://www.pythonchallenge.com/pc/return/wire.png

    input = Image.open("wire.png")
    output = Image.new("RGB", (100, 100))

    for i, px in enumerate(input.getdata()):
        output.putpixel(get_pixel(i), px)

    output.save("new.jpg")

    http://www.pythonchallenge.com/pc/return/cat.html
    cat
