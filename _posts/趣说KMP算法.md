---
title: 趣说KMP算法
date: 2016-08-06 19:59:24
tags: KMP算法
categories: 算法
thumbnail: http://7xveyh.com1.z0.glb.clouddn.com/string-matching-algorithms-24-638.jpg
---
今天逛知乎时，发现了一个比较浅显易懂的KMP算法解释。故在此做个笔记 <!--more-->。作者信息如下：
作者：逍遥行
链接：https://www.zhihu.com/question/21923021/answer/37475572
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

甲：abbaabbaaba
在里面寻找
乙：abbaaba
发现第 7 个字符不匹配。
这时候甲把乙叫走了，说你先一边玩去，我自己研究下。
然后甲想，自己已经知道乙的前 6 个字符就是自己的前 6 个字符，不妨先「自己与自己匹配一番」。
然后甲先用 abbaab 这 6 个已知的字符去匹配自身，错 1 个位，发现第一个就不一样（不匹配），然后错 2 个位，还是不匹配。
当错 3 个位的时候，甲发现匹配了一个 a，但是第二个 b 不匹配。
当错 4 个位的时候，匹配了两个。错 5 个位不匹配。后面的东西甲就不知道了，因为他只知道前 6 个字符。
（注：实际的匹配个数是字符串 [0...i] 的后缀与前缀的最长公共长度）

随后，甲把乙叫了过来：
「我已经知道你下一次匹配开始的位置了，来，让你的头部对齐我的第 5 个字符，然后从你的第 3 个字符开始继续匹配我吧！」

关键的地方，在于不要让乙「前功尽弃」——已经匹配了 6 个了，还差一个就结束了，这时不匹配导致从 0 开始，多可惜啊！
现在我告诉你，在不匹配的情况下，你仍然已经匹配了 2 个（乙内心：还好不是 0），并且你可以继续从不匹配的地方开始比较，即用你的 3 个字符与我继续匹配。
那，这个 2 你是怎么算的？
我在你来之前就算好啦！
我先与自己进行匹配（预处理），对每个位置，找「当前位置往前看的最长字符串，它与我的前缀匹配」（当然这个字符串不能是前缀），这个最长字符串的长度，在学术上称作「失配函数」。
UCCU，从你的第 6 个位置往前看，恰好 [ab] 与你的前缀 [ab] 匹配，但是我的第 7 个字符并不知道你的第 3 个字符是否与我一样，所以你直接从这里开始继续匹配我。

以上为 KMP 的基本思想，关键在于失配函数的计算，网上的代码很多，这里有个很好的例子你仔细体会下：ababzababa，注意最后一个失配函数的值为 3。