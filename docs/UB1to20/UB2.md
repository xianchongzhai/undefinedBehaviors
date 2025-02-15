# UB2

Template author: Xiangzhi Liu.

##  Definition

**A nonempty source file does not end in a new-line character which is not immediately preceded by a backslash character or ends in a partial preprocessing token or comment (5.1.1.2).**

## Description

该 UB 描述了三种不同的源代码不正常结束的情况。

第一种：以反斜杠和换行作为文件结尾。正常情况下反斜杠后紧跟换行意味着当前行要和下一行拼接成一个逻辑行，但如果当前行是最后一行将没有下一行进行拼接。

第二种：以不完全的块注释结尾。

第三种：以不完全的预处理 token（编译技术中的专有名称，可以简单看作标识符）结尾。（该部分存疑）

## Code

```c title="UB2.1.c"
// end with '\\ \n'
int main()
{
    return 0;
}
\

```

```c title="UB2.2.c"
int main()
{
    return 0;
}
/*
    partial block comment
```

```c title="UB2.3.c"
// partial preprocessing token
// IN DOUBT 
#define A(x) x
int main()
{
    return 0;
}
A
```
## Configurations

=== "Configuration 1."

    gcc version 4.9.2 

    `Target: x86_64-w64-mingw32`

## Behaviors

=== "Configuration 1."

    UB2.1.c: Compiled and run successfully with a warning info: `backslash-newline at end of file`

    UB2.2.c: Compile error: `unterminated comment`

    UB2.3.c: Compile error: `expected '=', ',', ';', 'asm' or '__attribute__' at end of input`

## Advice

避免出现上述三种不正常且不常遇到的情况

