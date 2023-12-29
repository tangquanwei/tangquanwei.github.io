---
title: Linux 三剑客
date: 2023-06-01 07:07:51
tags: Linux
cover:
---

## 正则表达式

[菜鸟](https://www.runoob.com/regexp/regexp-metachar.html)

[MDN](https://learn.microsoft.com/zh-cn/dotnet/standard/base-types/regular-expression-language-quick-reference)

| re    | 含义                                                                               |
| ----- | ---------------------------------------------------------------------------------- |
| *     | 匹配前面的子表达式零次或多次                                                       |
| .     | 匹配除换行符（\n、\r）之外的任何单个字符                                           |
| ?     | 匹配前面的子表达式零次或一次                                                       |
| +     | 匹配前面的子表达式一次或多次                                                       |
| ^     | 匹配输入字符串的开始位置                                                           |
| $     | 匹配输入字符串的结束位置                                                           |
| []    | 匹配所包含的任意一个字符                                                           |
| [^]   | 匹配未包含的任意字符                                                               |
| \\    | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个向后引用、或一个八进制转义符 |
| {n}   | n 是一个非负整数。匹配确定的 n 次                                                  |
| {n,m} | m 和 n 均为非负整数，其中n <= m。匹配 [n, m] 次                                    |
| x\|y  | 匹配 x 或 y                                                                        |



| 修饰符 | 含义                           |
| ------ | ------------------------------ |
| i      | ignore - 不区分大小写          |
| g      | global - 全局匹配              |
| m      | multi line - 多行匹配          |
| s      | 特殊字符圆点 . 中包含换行符 \n |
## grep

    grep [OPTION...] PATTERNS [FILE...]


## awk

[菜鸟](https://www.runoob.com/linux/linux-comm-awk.html)

    awk — pattern scanning and processing language

    awk '{[pattern] action}' {filenames} 
    
    2 this is a test
    3 Do you like awk
    This's a test
    10 There are orange,apple,mongo

    awk '{print $1,$4}' log.txt
    awk -F, '{print $1,$2}'   log.txt # 使用","分割
    awk -F[ ,] '{print $1,$2}'   log.txt # 使用多个分隔符.先使用空格分割，然后对分割结果再使用","分割
    awk 'BEGIN{}END{}' log.txt

## sed

    sed [option][action] filename
    
    -n 输出处理过的
    -e 多条命令
    -i 修改源文件

    a 行后追加
    c 行替换
    i 行前插入
    d 删除
    p 打印
    s 替换
    n,m 行选择


## 另外

### cut

    -f 列
    -d 分隔符
    -c 字符

### sort 

    -f 忽略大小写
    -n 数值
    -r 反向
    -t 分隔符(\t)
    -k 字符范围