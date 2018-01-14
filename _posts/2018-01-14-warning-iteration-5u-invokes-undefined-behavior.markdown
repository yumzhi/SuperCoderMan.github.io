---
layout: post
title: warning iteration 5u invokes undefined behavior
date: 2018-01-05 10:12:39 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: C.jpg # Add image post (optional)
tags: [C, ESP8266]
---
## 有些警告信息也是不能忽略的.
警告信息如下：

```
In file included from user_main.c:17:0:
user_main.c: In function 'devManage':
user_main.c:319:45: warning: iteration 5u invokes undefined behavior [-Waggressive-loop-optimizations]
   os_printf("]\n");
                                             ^
../../include/osapi.h:39:30: note: in definition of macro 'os_printf'
  os_printf_plus(flash_str, ##__VA_ARGS__); \
                              ^
user_main.c:317:3: note: containing loop
    os_printf("0x%02x ",devNode->dev_status[i]);
   ^
In file included from user_main.c:17:0:
user_main.c:312:46: warning: iteration 5u invokes undefined behavior [-Waggressive-loop-optimizations]
   }
                                              ^
../../include/osapi.h:39:30: note: in definition of macro 'os_printf'
  os_printf_plus(flash_str, ##__VA_ARGS__); \
                              ^
```

按照出现的先后顺序，找到最先出现警告信息的位置，即`user_main.c:319:45:`,查看附近相关代码：

```c
		os_printf("Sub Device Status =[");
		for(i=0;i<8;i++)
		{
			os_printf("0x%02x ",devNode->dev_status[i]); //317

		}
		os_printf("]\n"); //319
```

这是一段调试代码，为了打印子设备的状态信息。警告信息出现的位置均在这一部分代码，初看没有发现什么问题。在浏览器中搜索警告信息`warning: iteration 5u invokes undefined behavior`,[参考链接][reference-linked]后发现是因为数组下标越界(index is out of bound)造成的，回头检查自己的代码，结构变量`devNode`的成员变量`dev_status`的定义是一个长度为`5`的整型数组，循环打印时的循环变量`i`的上限是`8`，更正为`5`后警告信息消除。

(这是调试代码是从其他部分复制过来的，更改了要打印的目标变量而忽略循环变量，导致访问数组越界，出现上述警告信息。幸亏只是打印调试，不然后果还是比较严重的。)

[reference-linked]: https://stackoverflow.com/questions/38781770/warning-iteration-5u-invokes-undefined-behavior-waggressive-loop-optimization#