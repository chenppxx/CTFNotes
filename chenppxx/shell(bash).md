# shell(bash)

## 创建脚本程序

在终端创建一个`.sh`文件,`vi test.sh`

`#!/bin/bash`

`#first shell demo`

`echo "hello world"`

第一行，#!是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，用的是哪种shell，后面的`/bin/bash`就是指明了解释器的具体位置。 第二行#是注释行，用来解释说明，当然#！是特殊的，不在此类。 第三行在是终端输出 hello，is me！

### 执行shell脚本

`./test.sh`:./表示当前目录，执行./test.sh会说明权限不够，不能执行。需要改变文件的权限：chmod 777 test.sh，就能执行

`. ./test.sh`: 在不想改变权限的时候，测试脚本是不是能够正常使用。. 临时增加权限。

## 获取字符串长度

`str="123456"`

`echo "${#str}"`

输出:6

## shell变量

在bash shell中，每一个变量的值都是字符串， 当然也可以用declare 关键字显式定义变量的类型，在赋值的时候等号两边不能有空格，如：str=1 ，str='1' ,str="1",变量名必须有字母、下划线、数字组成，开头必须字母或者下划线，不能用shell。

- **局部变量**：shell也有自定义函数，函数里面的变量为局部变量，但是它也是相当于全局变量，函数中的变量，在函数外调用也是可以的，如果要仅限函数使用，需要在函数变量前加个关键字：local
- **全局变量**：每打开一个终端就是一个shell会话，在这个shell会话（终端）定义的变量就是全局变量，它在这个shell会话有效，当你打开另一个终端就是另一个shell会话，这个变量在另一个终端就失效了。
- **环境变量**：在全局变量前加 export ，如：export a=1  那么这个变量就是环境变量了。创建这个变量的shell成为父shell，这个shell中，在创建一个shell叫做子shell，环境变量可以由父shell往下一级一级传，而不能逆转往上传递。当shell会话销毁时，这个环境变量也会随之销毁。想要永久保存就得环境变量写到启动文件中去。

### shell变量引用

使用shell变量在变量前面加一个`$`,而标准的是 `&{}`，目的是在一长串字符中可以识别出这个变量，而不会引起误会

### shell变量的赋值,修改,删除

- 在变量前加`readonly`,就是只读变量
- `unset`用于删除变量,如`unset a`

## shell特殊变量

- **$0**:当前脚本的文件名或者解释器。
- **$n(n>=1)**:传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2。
- **$#**:传递给脚本或函数的参数个数。
- **$***:传递给脚本或函数的所有参数。
- **$@**:传递给脚本或函数的所有参数。当被双引号`" "`包含时，`$@` 与 `$*` 稍有不同，$@所有参数是一个数据，而​@一个参数就是一份数据
- **$?**:上个命令的退出状态，或函数的返回值

- **$$**:当前 Shell 进程 ID。对于 [Shell 脚本](http://c.biancheng.net/shell/)，就是这些脚本所在的进程 ID。

**脚本：**如果参数个数太多，达到或者超过了 10 个，那么就得用`${n}`的形式来接收了，例如 `${10}`、`${11}`

##  字符串拼接

n1=ab

n2=cd
temp1=${n1}${n2}  *#中间不能有空格* 

temp2="${n1}${n2}"

temp3="${n1}cdefg"

## 字符串的截取

- `${string: start :length}`：字符串从左边start开始的位置截取length个字符，如果没有length，就从左边start开始到结束
- `${string: 0-start :length}`：字符串从右边start开始的位置截取length个字符，如果没有length，就从右边数start开始到结束
- `${string#*chars}`：字符串从左边开始的第一个chars开始的位置截取到右边结束，chars本身不在截取范围内
- `${string##*chars}`：字符串从左边开始的最后一个chars开始的位置截取到右边结束，chars本身不在截取范围内
- `${string%*chars}`：字符串从右边边开始的第一个chars开始的位置截取到左边结束，chars本身不在截取范围内
- `${string%%*chars}`：字符串从右边开始的最后一个chars开始的位置截取到左边结束，chars本身不在截取范围内


## shell数组

bash shell 中只支持一维，而不支持二维，定义的形式：array=(n1 n2 n3) ,数组名等号两边不能有空格，数组变量的值用空格隔开表示不同的值，一个数组变量的值可以用数字或者字符串不同的形式组成：array=(1 2 ab) 。

**调用数组**:

- `${array[0]}`
- `a=${array[0]}`
- `${array[*]} ` //表示数组的所有元素
- `${array[@]} ` //表示数组的所有元素

**可以使用unset array2删除整个数组**

## shell中条件判断if

     -eq 等于,如:if [ "$a" -eq "$b" ]
    
    -ne 不等于,如:if [ "$a" -ne "$b" ]
    
    -gt 大于,如:if [ "$a" -gt "$b" ]
    
    -ge 大于等于,如:if [ "$a" -ge "$b" ]
    
    -lt 小于,如:if [ "$a" -lt "$b" ]
    
    -le 小于等于,如:if [ "$a" -le "$b" ]
### if单分支

#!/bin/bash
#shell中if：
#if单分支第一中形式：if []
if [ 1 -eq 1 ] #[]里面的数据和中括号必须一个空格 
then
        echo "first if:真"
fi

#if单分支第二中形式：if []; then
if [ 1 -eq 1 ]; then #then和if写在同一行必须要用; 
        echo "second if:真"
fi

#if单分支第三中形式：if (())
if ((1==1))
then
        echo "third if:真"
fi

 ### if双分支

#!/bin/bash
#shell中if：
#if双分支
if ((1==0)) #判断1和o是不是相等
then
        echo "我是真的"
else
        echo "我是假的"
fi

#................................
if [ 1 -eq 0 ] #判断1和o是不是相等,
then
        echo "我是真的"
else
        echo "我是假的"
fi

### if多分枝

#!/bin/bash
#shell中if：
#if多分支
if ((1==0)) #判断1和o是不是相等
then
        echo "1=0；是真的"
elif ((1>0))
then
        echo "1>0;是真的"
else
        echo "1>0;是假的"
fi

#..............................
if [ 1 -eq 0 ] #判断1和o是不是相等
then
        echo "1=0；是真的"
elif [ 1 -gt 0 ]
then
        echo "1>0;是真的"
else
        echo "1>0;是假的"
fi

## shell中的case语句

#!/bin/bash
#shell中的case

read VAILE #在屏幕中输入一个数,1或者2，如果不是这两个数就代表其他；即 *），
case ${VAILE} in
        "1")
                echo "我是1"
        ;;

​		"2")

​				echo "我是2"

​		;;

​		*)

​				echo "我不是1,也不是2"

​		;;

esac

## shell中的for循环

### for循环形式1:

for n in 1 3 5

do

​	echo "......"

done

### for循环形式2:

与c语言类似

for ((n=0;n<3;n++))

do 

​	echo "......"

done

## while循环

while [ ${n} -lt 2 ] *#n<2;就循环，否则退出循环*

do

​        n=$((${n}+1))

​        echo "我是n，我的值是：${n}"

done

​        echo "我退出循环了"

## until循环

1. n=-2
2. until ((n>1)) *#不满足条件就循环，满足条件则退出循环*
3. do
4. ​        n=$((${n}+1))
5. ​        echo "我是n，我的值是：${n}"
6. done
7. ​        echo "我退出循环了"

## shell函数

语法:

function 函数名(){

............

}

#!/bin/bash
#shell中的函数
function fun(){
        if(($1 > 1)); then
                echo "条件成立"
        fi

​		if (($2 > 1)); then

​				echo "条件成立"

​		else

​				echo "    ....."

​		fi

}

fun 2 1   #这边传参数给fun()函数
