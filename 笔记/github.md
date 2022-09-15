# 记一下这次重新使用GutHub遇到的问题
>2022年09月14日 06:46:22

首先是Markdown中文内容在github.io查看会乱码  
保存成“UTF-8 with BOM”就不会乱码了  
可以通过github.dev编辑文件，修改保存编码

我一直以为应该在访问Md时自动套模板  
一直以为是我的文件有问题导致识别失败而没套模板  
调了几小时

但是\
Md生成的HTML，是通过把网址最后的.md改成.html访问的\
（也可以省略）\
而即使Md使用默认编码（UTF8）  
查看Md时乱码，但查看HTML时不会乱码  
就是说不用改编码，不需要处理，也没有实际影响  
（不用查看.md的话，实际当然不用）

# 用平板和手机编写Markdown
GitHub访问极不稳定，需要VPN  
在iPad上随便找了“Shadow X”就可以完全正常使用

Android上没有这个，试了Clash和好几个ss  
都会加载不全，在项目代码页，Git提交记录加载不出来  
可以打开GitHub的在线编辑器  
但MD的编辑环境不能加载（看不见正常应该有的行数显示）  
Preview和Commit都是灰的
不能保存，就是完全用不了  
github.dev则卡在第一个加载提示  
这个现在猜可以装Git客户端APP来解决

后来查到[Codeit](https://github.com/codeitcodes/codeit)  
这个特别方便，在iPad上不用挂VPN就能用  
因为我需要的功能也非常简单，能写能提交就行了  
所以打算暂时用这个就好  
（这个在Android上仍然不能加载  
和github.dev一样卡在第一个画面，不能加载  
不管是否用VPN）

#### 更新
安卓上的问题后来通过腾讯浏览器解决了\
不需要VPN， Codeit 使用没出现问题\
GitHub 1234和github.dev老样子，极不稳定\
能打开但不是总能打开，至少能用了

*[GitHub]: 123测试456
