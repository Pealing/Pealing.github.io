---
title: openai-atari windows 安装瞎折腾
date: 2018-03-08 10:52:35
updated: 2018-03-08 10:52:35
tags:
---
# 前言
    因为毕业设计需要，最近在学习强化学习+OPENAI的一些东西。
    OPENAI推出的ATARI游戏系列，按照官方的说法是不支持windows的
    奈何设备有限，被迫只能windows
<!--more-->

# 简述：

* 先贴上OPENAI的源码地址：
    - [openai/gym](https://github.com/openai/gym)
    - [gym/atari](https://github.com/openai/atari-py)

* 理论上来说，如果你是Ubuntu或者OS,安装以上两个包只需要：
    - pip install gym
    - pip install gym[atari]

* 当然如果很不幸，你是windows，而且必须一定只能windows的话，那么
    - 如果你只需要使用gym里面的一下画面简洁/操作简单的小游戏，就只需要：
        + pip install gym
    - 如果…嗯，一定要atari的话，来吧，请继续往下看。

# 报错T^T

* 首先，当你运行`pip install gym[atari]` 你会惊喜地发现，是可以安装的！但是！
很快错误就出现了：`CalledProcessError: Command '['make', 'build', '-C', 'atari_py/ale_interface', '-j', '3']' returned non-zero exit status 2`

* 接着，我尝试了另一种方式：`git clone https://github.com/openai/atari-py.git` & 
`python setup.py install`.这种方法下，成功倒是成功了，但是当你开心地`import atari_py`的时候，他又会告诉你`make error`

* 之后我又尝试了使用cmake+VS2015的方式编译、修改ctypes的方式，总之，都失败了。

# 结果：
感谢万能的知乎，其实在一开始查这个问题的时候我就找到了答案，但是一直没有尝试，绕了一圈之后…总之追悔莫及。
先贴上别人的解决方案：
[知乎](https://zhuanlan.zhihu.com/p/33834336)
[stackOverflow](https://stackoverflow.com/questions/42605769/openai-gym-atari-on-windows)

简单附上安装atari_py的方式：
* 首先需要`pip install numpy` `pip install six`
    - (因为我用的virtualenv,所以需要重新安装这两个，如果本来就有，自然不需要重新安装)
* `pip install gym`
* `pip install --no-index -f https://github.com/Kojoley/atari-py/releases atari_py`

大概就是以上3句话，很简单吧。我却绕了3天时间T^T
另外，据我同学所说，他用另外修改ctypes的方式成功过，可惜当时没有记录忘记了。
等他恢复记忆我再来更新吧！
