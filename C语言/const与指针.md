# const与指针

## 作用

限定一个**变量**为只读

## 位置的区别

1. 定义非指针变量：没有区别（const int等效于int const）
2. 定义指针

```c
int *const p1 = &a; //指针p1不能指向其它地址
const int *p2 = &a; //指针p2只读&a，不能修改a
```

例如：

```c
p1 = &b; //非法
p2 = &b; //合法
*p1 = 7; //合法
*p2 = 7; //非法
```

可以这样理解：

```c
int* (const p1) = &a;
(const int)* p2 = &a;
```

注：这样的语法并不正确

## 通过指针修改const限定的变量

正如前面提到，const关键字只是限定一个**变量**为只读，因为它本质上还是变量，所以可以通过指针来修改其值

举例如下：

```c
const int a = 1;
int *p = &a;
*p = 2;
```

这样的操作可以通过指针p间接地修改变量a中的值，尽管它已经被const限定为只读  
但这样的代码会报一个警告：initialization discards 'const' qualifier from pointer target type [-Wdiscarded-qualifiers]  
编译器还是识别到了通过指针修改只读变量的操作，因为指针p是int基类型的，而a是const int类型的

但如果这样写，编译器就不会报警告：

```c
const int a = 1;
const int *p1 = &a;
int *p2 = (int *)p1;
*p2 = 2;
```

这里把const int基类型的指针p1强制转换为int基类型的指针p2，再由p2修改了a的值，编译器就没有报警告，毕竟你确实是拿const int接下的a的地址啊  
所以在实际使用中，千万记得用const int为基类型的指针去接下const限制的int变量的地址，并注意不要转换指针类型，以免修改const限制的变量，与初衷不符
