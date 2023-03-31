# main函数的形参

```c
int main(int argc, char *argv[])
```

在命令行中运行程序时，用于将命令行中附带的参数传入程序内

> argc：传入的参数个数（文件名本身也算1个参数）  
> *argv[]：传入的参数内容（以一个指针数组的形式）

假设编译后的可执行性文件的文件名为“test.exe”，存储于D盘根目录下，则当执行如下命令行代码后：

```cmd
D:\test.exe Microsoft Google Apple
```

argc的值为4，argv数组中：
* argv[0]为指向字符串“D:\test.exe\0”的指针
* argv[1]为指向字符串“Microsoft\0”的指针
* argv[2]为指向字符串“Google\0”的指针
* argv[3]为指向字符串“Apple\0”的指针

测试代码如下：

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    for (int i = 0; i < argc; i++)
        printf("%s\n", argv[i]);

    return 0;
}
```
