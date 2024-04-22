## word隐写

文件--选项--隐藏文字勾选

## 杂项

- 不知道密码可以用ARCHPR暴力,最好给定一定范围

## zip文件

一个 ZIP 文件由三个部分组成：
    

​    压缩源文件数据区+压缩源文件目录区+压缩源文件目录结束标志

### 伪加密

压缩源文件目录区：

50 4B 01 02：目录中文件文件头标记

3F 00：压缩使用的 pkware 版本 
14 00：解压文件所需 pkware 版本 
00 00：全局方式位标记（有无加密，这个更改这里进行伪加密，改为09 00打开就会提示有密码了）

## 隐写

- 当图片无法打开时,可能是文字头出现问题.例如:png图片的八字节文件头为8950 4e47 0d0a 1a0a,可以用十六进制编辑器打开后修复文件头

## 修改图片高度宽度

将png图片拖进010中,第二行前四个代表宽,后面四个代表高

### 利用jphs处理jpg图片

- 准备一个空的txt文件,在jphs中打开图片后seek到txt文件中.
- 如果txt文件不行可以尝试转换成其他格式文件

### 音频隐写

当文件格式为flac时,可以使用audocity打开,观察有无摩斯电码或者切换成频谱图形态去看

## 隐藏文件

可以用binwalk查看图片有没有隐藏的文件

再使用foremost,语法:

foremost -i 文件 -o [打开的文件存放的位置]

### 分离文件

- foremost -i 文件 -o [打开的文件存放的位置]

- binwalk -e [文件]       分离成功会在当前目录下有含extracted文件

- 对于png图片,可以拖入tweakpng中,png文件有四个数据块IHDR，PLTE，IDAT，IEND .删除多余的就可以找到flag

- 当binwalk -e无法分离文件,可以使用dd命令:

	dd if=[输入文件] of=[输出文件] skip=[位置] bs=1

	**解释**:解释：if 指定输入文件，of 指定输出文件，skip 指定从输入文件开头跳过多少个块后开始复制，bs设置每次读写块的大小为1字节

- **zsteg**:`zsteg -e extradata:0 misc17.png > 1.txt`

	提取后如果没有flag,可以再用binwalk对1.txt进行提取

## 判断文件格式是否篡改

### png文件

检查png的文件头和文件尾，文件格式正常 PNG文件头(hex)：89 50 4e 47 0d 0a 1a 0a PNG文件尾(hex)： 00 00 00 00 49 45 4E 44 AE 42 60 82 

## exiftool

`exiftool misc20.jpg`

查看详细信息,flag可能隐藏在里面

## qr-research

用来扫描二维码

## stegsolve

- 把图片拖进去后,analyse>>data extracted
- 选择rgb,把red,gree,blue设置为0
- preview
- save bin

## wireshark

抓包

通过过滤找到想要的信息,例如http.request.method==POST

## steghide

**使用steghide查看文件隐写的内容,比stegsolve更好用**

steghide extract -sf 文件名 -p 密码

没有密码可以不要-p



## 二维码补全

当二维码缺角时,可以随便找个网站生成二维码,再把定位角弄到残缺的二维码上



## snow隐写

碰到有多余的空格可能就是snow隐写

把文件放在snow工具目录下面,执行.\snow.exe -C [文件名]



## AAencode加密

类似`颜文字`的形式,直接找在线工具破解



## 查看zip注释

用7z打开zip后,

点击`信息`,可以看到zip文件的注释



## 百家姓编码

实例:`奚郎昌任云窦葛奚韦福`

直接找在线工具



## stegdetect

`.\stegdetect.exe -tjopi -s 10.0 [文件名]`

文件要放在同一目录下



## F5隐写

进入E:\ctf_tools\F5-steganography-master

`java Extract [文件地址]`



## unicode

`\u4e00\u95ea\u4e00\u95ea\u4eae\u6676\u6676 \u6ee1\u5929\u90fd\u662f\u5c0f\u661f\u661f`

类似以上这种格式就是unicode

