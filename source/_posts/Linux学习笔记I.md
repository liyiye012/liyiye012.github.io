---
title: Linux学习笔记I
date: 2018-10-22 11:41:13  
categories: 2019年10月
tags: [Linux]

---


学习Linux命令

<!-- more -->

# cat

cat 命令用于连接文件并打印到标准输出设备上。

# 语法格式

    cat [-AbeEnstTuv] [--help] [--version] fileName

#  参数说明：

-n 或 --number：由 1 开始对所有输出的行数编号。

-b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。

-s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。

-v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。

-E 或 --show-ends : 在每行结束处显示 $。

-T 或 --show-tabs: 将 TAB 字符显示为 ^I。

-A, --show-all：等价于 -vET。

-e：等价于"-vE"选项；

-t：等价于"-vT"选项；

# 实例：

把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：

    cat -n textfile1 > textfile2
把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：

    cat -b textfile1 textfile2 >> textfile3
清空 /etc/test.txt 文档内容：

    cat /dev/null > /etc/test.txt
cat 也可以用来制作镜像文件。例如要制作软盘的镜像文件，将软盘放好后输入：

    cat /dev/fd0 > OUTFILE
相反的，如果想把 image file 写到软盘，输入：

    cat IMG_FILE > /dev/fd0

# dev/null

在类 Unix 系统中，/dev/null 称空设备，是一个特殊的设备文件，它丢弃一切写入其中的数据（但报告写入操作成功），读取它则会立即得到一个 EOF。

而使用 cat $filename > /dev/null 则不会得到任何信息，因为我们将本来该通过标准输出显示的文件信息重定向到了 /dev/null 中。

使用 cat $filename 1 > /dev/null 也会得到同样的效果，因为默认重定向的 1 就是标准输出。 如果你对 shell 脚本或者重定向比较熟悉的话，应该会联想到 2 ，也即标准错误输出。

如果我们不想看到错误输出呢？我们可以禁止标准错误 cat $badname 2 > /dev/null。
# 参考资料
【1】 https://www.runoob.com/linux/linux-comm-cat.html
