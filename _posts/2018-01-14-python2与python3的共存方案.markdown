---
layout: post
title: python2与python3的共存方案
date: 2018-01-03 21:02:50 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: python.jpg # Add image post (optional)
tags: [Python, Programming]
---
>各自独立安装好两个版本（python 2和python 3），添加相应的环境变量，安装顺序不做要求。

* 控制台中指定python版本

  * 调用python 2

    ```python
    py -2
    ```

  * 调用python 3

    ```python
    py -3
    ```

* 在python文件中指定版本号

  * 在文件中调用python 2

    ```python
    #!python2
    ```

  * 在文件中调用python 3

    ```python
    #!python3
    ```

* 调用不同版本下的`pip`命令（以安装`numpy`模块为例）

  * 方案一

    * python 2

      ```python
      pip2.7 install numpy
      ```

    * python 3

      ```python
      pip3.6 install numpy
      ```

  * 方案二

    * python 2

      ```python
      py -2 -m pip install numpy
      ```

    * python 3

      ```python
      py -2 -m pip install numpy
      ```

  【已经过实际验证】


