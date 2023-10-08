# 对typedef语法的理解

`typedef` 的语法可以与定义变量的语法对应：定义变量时，变量名的位置对应成使用 `typedef` 时别名的位置，其他所有语法都按照定义变量的语法来

如：

```c
typedef int *int_ptr;
int_ptr p;
```

`int_ptr` 就成了 `int*` 的别名， `p` 是 `int*` 类型的变量

再如结构体的使用中：

```c
typedef struct fruit
{
    char *name;
    int color;
} fruit, *fruit_ptr;

fruit apple;
fruit_ptr p;
```

此处 `fruit_ptr` 就是 `struct fruit*` 的别名
