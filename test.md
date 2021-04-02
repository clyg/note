## C语言笔记
---
### 1. 字符串的定义
一个字符数组, 最后加上一个转义字符\0, 我们称之为字符串。
在ASCII码中, \0对应的值是0。
如果在一串字符串中将中间某一个元素改为0, 那么认为字符串到这里就会结束。
### 2. 字符串的初始化
字符串在使用前必须进行初始化。
字符串的初始化推荐使用memset来初始化。
```c
#include <string.h>
char strname[21];
memset(strname, 0, sizeof(strname)); // 将strname中的所有元素置为0
```
### 3. 字符串使用printf输出
```c
printf("%10s\n", strname) // 输出10个字符宽度, 右对齐
printf(“%-10s\n”, strname) // 输出10个字符宽度, 左对齐
```
如果字符串的长度超过了格式化输出的长度, 并不会发生截断, 而是会输出原来的值。
如果小于, 则会使用空格补上需要的字符串数之后输出。
### 4. 字符串常用函数
使用字符串函数前, 一般都要包含string.h头文件。
- strcpy(): 将一串字符串复制另一个字符串strname。
复制结束后, 会在字符串最后加上一个0。
```c
strcpy(strname, src);
```
- strlen(): 计算字符串的实际长度, 不包含0
```c
strlen(strname);
```
- strncpy(): 将一个字符串的前n个字符赋值到另一个字符窜strname。
如果src的长度小于n, 则复制完字符串之后会在后面追加0, 直到n个。
如果src的长度大于n, 就截取src的前n个字符, 不会在后面追加上0。
```c
strncpy(strname, src, n);
```
- strcat(): 将一个字符串连接到另一个字符串尾部。
strname最后的0会被覆盖掉, 连接完成之后会在字符串的尾部加上一个0。
```c
strcat(strname, src);
```
- strncat(): 将一个字符串的前n个字符连接到另一个字符串的尾部。
必须要保证strname有足够的空间来存放连接后的字符串
strname最后的0会被覆盖掉, 连接完成之后会在字符串的尾部追加上一个0。
如果n大于等于src的长度, 那么会将src全部追加到strname的尾部。
如果小于, 则只会追加n个字符。
但是无论如何, strname尾部的0将会被替换, 连接完成后会在尾部加上一个0。
```c
strncat(strname, src, n);
```
- strcmp(): 比较两个字符串的大小
如果两个字符串相等, 返回0。
如果str1大于str2, 返回1。
如果str1小于str2, 返回-1。
```c
strcmp(str1, str2);
```
- strncmp(): 比较一个字符串的前n个字符和另一个字符串是否相等
规则和strcmp()相同
```c
strncmp(str1, str2, n);
```
- strchr(): 检索一个字符串中是否有指定的字符, 一般用于判断该字符是否存在于字符串中
返回字符串中第一次出现字符的位置, 如果找不到, 则返回0。
```c
char strname[21];
memset(strname, 0, sizeof(strname));
strcpy(strname, "www.freecplus.net");
char *pos = 0;
pos = strchr(strname, 'e');
if(pos == NULL)
{
    printf("未找到\n")
}
```
- strrchr(): 检索一个字符在一个字符串中最后一次出现的位置
规则同strcht();
```c
char strname[21];
memset(strname, 0, sizeof(strname));
strcpy(strname, "www.freecplus.net");
char *pos = 0;
pos = strrchr(strname, 'e');
if(pos == NULL)
{
    printf("未找到\n")
}
```
- strstr(): 检索子字符串在另一个字符串中出现的位置
返回子字符串首次出现在字符串中的地址, 如果没有找到, 则返回0。
```c
char strname[21];
memset(strname, 0, sizeof(strname));
strcpy(strname, "www.freecplus.net");
char *pos = 0;
pos = strrchr(strname, "plus");
if(pos == NULL)
{
    printf("未找到\n")
}
```
### 5. 字符串使用经验
1. 对于strcpy, strncpy, strcat, strncat而言, 必须要保证strname的空间足够大
否则可能会造成缓冲区溢出。
这时要使用一定的内存空间来换取程序的稳定性。
2. 字符串在每次使用前, 都要进行一次初始化, 以防发生意外。
3. strncpy()与字符串的截取
```c
char str1[21];
char str2[21];

memset(str1, 0, sizeof(str1));
memset(str2, 0, sizeof(str2));

strcpy(str1, "bcdefgh");
strcpy(str2, "abcdefgh");

strncpy(str1 + 2, str2 + 2, 3); // 此时str1是bccdegh
```
4. 不要在子函数内使用sizeof()
对于下面的代码, 在fun()中, str是一个指针, 大小是8
但是在main()中, strname是一个数组, 大小是21
```c
#include <string.h>
void fun(char *str);

int main(void)
{
    char strname[21];
    memset(strname, 0, sizeof(strname)); // 此时的sizeof(strname)结果是21
    return 0;
}
void fun(char *str)
{
    memset(str, 0, sizeof(str)); // 此时sizeof(str)的结果是8
}
```