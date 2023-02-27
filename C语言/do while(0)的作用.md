# do{xxxx}while(0)的作用

## 1. 宏定义中使用

如果这样写：  
 `#define FUN() fun1(); fun2();`

那么，如果遇到下述代码：

```c
if (a > 0)
	FUN();
```

则宏定义展开后，实际代码会变成：

```c
if (a > 0)
	fun1();
	fun2();
```

会发现，fun2不在if语句的范围内，因此可以用do while(0)来包装两个函数，以此避免这样的问题，形式如下：  
 `#define FUN() do { fun1(); fun2(); } while(0)`

一个实际例子：

```c
#define handle_error_en(en, msg) \
    do { errno = en; perror(msg); exit(EXIT_FAILURE); } while (0)
```

或

```c
#define handle_error_en(en, msg) \
	do                           \
	{                            \
		errno = en;              \
		perror(msg);             \
		exit(EXIT_FAILURE);      \
	} while (0)
```

其中：反斜杠为宏定义的继续符，表示本行与下一行连接起来

## 2. 需要用break时使用

示例如下：

```c
int fun()
{
    int *p = malloc(100);
	char error = false;

    do
    {
        //一些操作
        if(error)
            break;

        //一些操作
        if(error)
            break;

        //一些操作
    }
    while(0);

    free(p);
    return 0;
}
```
