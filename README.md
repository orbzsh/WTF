###**记录MD语法**

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
------------------------------------
###列表
	无序列表，前面加上如下:
	-
	+
	*

	有序列表,前面加上如下:
	1.
	2.
	3.
####有序列表
1.	文本1
2.	文本2

####无序列表
*	无序列表1
-	无序列表2
+	无序列表3

------------------------------------
###链接
```
[](),方括号里是显示的文本,圆括号里是链接的地址
![](),前面的！号表示图片链接
<>,邮件链接，<>中放入邮件地址
```
------------------------------------
###分割线

	三个或者多个***或者---或者===,中间可以有空格
	多个的话线条比较粗,三个的线条细
------------------------------------
###代码区块
```
代码块可以用`号包起来
或者:缩进一个制表符或者4个空格
```

	package main
	import "fmt"
	func main(){
		fmt.Println("Hello World")
	}
------------------------------------
###区块引用
```
使用>后面跟上引用语句，可以换行等，注意>号后面要有一个空格
引用的区块内也可以使用其他MD语法,包括标题、列表、代码区块等
```
> 引用：人生不止眼前的苟且，还要有诗和远方
>
>> 你走，我不送你。你来，无论多大风多大雨，我要去接你。
>>
> ####区块内标题
> 1.	区块内列表1
> 2.	区块内列表2
>
> echo "$ARGV";

------------------------------------
###粗体斜体
```
用两个*包含一段文本就是粗体,用一个*包含文本就是斜体
```
**粗体**

*斜体*

------------------------------------
###表格
```
代码展示：
name |age |sex
---- |----|----
Jack |21  |man
Jacki|20  |women
```
------------------------------------
###版权符号
```
&copy;
```
------------------------------------
Weibo     |[Wocin](http://weibo.com/kong110)
----------|---------------------------------
Github    |[Github](https://github.com/orbzsh)
Email     |<925511765@qq.com>
Copyright |&copy;Orbzsh

参考网站[MD官方](http://wowubuntu.com/markdown/)