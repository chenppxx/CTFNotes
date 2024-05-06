# sort函数

需要包含头文件`#include <algorithm>`

对一个数组进行排序:

`sort(a+1,a+50)`    #对a[1]--a[49]进行升序排序





# sizeof()和length(),strlen()

在 C++ 中，`sizeof` 运算符用于获取对象或数据类型的大小（以字节为单位），而不是获取字符串的长度。所以，使用 `sizeof(a)` 得到的结果是 `string` 类型的对象 `a` 所占用的字节数。

可以使用`a.length()`来获取字符串a的长度



`strlen(a)`不能应用于string类型的对象,因为strlen需要以`\0`为结尾



# `int f[40][40];`，如果它是全局数组的话，所有元素会被初始化为 0。



# int不能用-2147483648当最小值的问题

需要使用

`#define INT_MIN	(-2147483647 - 1) `



# 字符串函数

```cpp
extern char strcat(char dest, const char src);
```

> `strcat` 函数将 `src` 串拼接到 `dest` 串之后

```cpp
extern char strstr(char str1, const char str2);
```

> ```
> strstr` 函数在 `str1` 串内查找 `str2` 串的位置，如未找到，则返回 `NULL
> ```

```cpp
extern char strcpy(char dest, const char src);
```

> `strcpy` 函数将 `src` 串复制到 `dest` 串