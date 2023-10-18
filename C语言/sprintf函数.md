# sprintf函数

sprintf函数和printf类似，只是它不将格式化输出打印到标准输出，而是存储在指定的字符串中  
使用sprintf函数时，相比printf的参数，只需在最前面加一个参数指向对应字符串

```c
#include <stdio.h>

int main(void)
{
    char str[20];
    char *ch_ptr = str;
    float pi = 3.14;

    sprintf(str, "pi=%f", pi);
    while (*ch_ptr != '\0')
        putchar(*ch_ptr++);

    return 0;
}
```
