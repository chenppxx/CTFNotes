# reverse

## buuctf题解

### easyre

1.拿到附件,先用**exeinfo pe**打开,查一下壳,看一下是几位的程序,用对应位数的ida打开.64位程序,没有壳

2.64位ida直接载入,检索flag

## reverse1

1.用**exeinfo pe**打开,没有壳,64位程序

2.64位ida载入之后,检索flag,发现"right flag",双击跳到对应位置

3.选中代码之后,按ctrl+x找到对应程序

4.f5可以把程序转化成伪代码,方便阅读.发现if ( !strncmp(Str1, Str2, v3) )
    sub_1400111D1("this is the right flag!\n");判断sub_1400111D1是输出函数

5.找到str2,结合具体情况对str2进行转换,得到flag



## 新年快乐

1.**exeinfo pe **打开,32位程序,并且有upx壳

2.尝试手动脱壳,用od打开后按f7,在后边对变红的一行follow in dump

3.在对应位置设置断点后,f9运行程序,就可以找到程序的OEP

4.用插件ollydump进行脱壳

5.脱壳后的程序用ida打开,找到主程序,f5找到flag

## xor

1.**exeinfo pe**打开没有找到有用信息,直接用ida64打开

2.找到主函数,发现是一个简单的异或程序

3.找到global字符串,shift+f5导出

4.编写程序,还原flag



