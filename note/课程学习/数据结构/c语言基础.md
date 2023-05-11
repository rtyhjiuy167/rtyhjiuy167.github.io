### 指针

在32位系统（即x86)中，指针的大小为4字节。在64位系统（即x64)中，指针的大小为8字节。

c语言为指针变量提供一对特殊的运算符：

* `&`：取址运算符

  用于得到变量的地址

* `*`：间接寻址运算符

  用于访问指针所指向的对象

```c
#include<stdio.h>
int main()
{
	int num = 1;
	int *p; // 声明 int 型指针，*p表示指向的对象中的内容，p为指向的对象的地址（也称其为p指针）
	p = &num; // *p = num; // p = &*&num; //等价
	printf("%d", *p); // 1
    
	return 0;
}
```

由于改变`*p`即改变指针`p`指向的对象的内容，所以会改变原先对象的值：

```c
#include<stdio.h>
int main()
{
	int num = 1;
	int *p;
	p = &num; 
	*p = 2;
	printf("%d", num); // 2
	return 0;
}
```

利用`*`与`&`来进行实参传递：

```c
#include<stdio.h>
void func(int* a,int& b) {
	printf("%d %d", *a,b);
}

int main()
{
	int num1 = 1,num2 = 2;
	// &num1会形成*int指针
	// b 形参会引用 num2 实参
	func(&num1, num2);
	return 0;
}
```

### 结构体

```c
#include<stdio.h>

typedef struct{
	char char1;
	char char2;
	int  num;
}MyStruct;

int main()
{
	printf("%d", sizeof(MyStruct)); // 8
	return 0;
}
```

<img src=".\pictures\struct1.png" alt="struct1" style="zoom:80%;" />

```c
#include<stdio.h>

typedef struct{
	char char1;
	int  num;
	char char2;
}MyStruct;

int main()
{
	printf("%d", sizeof(MyStruct)); // 12
	return 0;
}
```

<img src=".\pictures\struct2.png" alt="struct2" style="zoom:80%;" />

### 输入与输出

#### 输入

使用`scanf()`，以空格作为不同字符或字符串的区分，按下回车键结束输入。

##### 字符串输入

```c
#include<stdio.h>
#include<string.h>
int main()
{
    char str[20]; 
    printf("请输入字符串：");
    scanf("%s", str); 
    printf("%d", strlen(str));
    return 0;
}
```

