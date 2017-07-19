##HTML骨架
*   [code](https://maxiaobu1999.github.io/html5/heima/html/base.html)

        <head>
            <meta charset="UTF-8"><!--1字符集-->
            <meta name="Keywords" content="收索引擎关键字 SEO"><!--2收索引擎关键字-->
            <meta name="Description" content="收索引擎页面描述 SEO"><!--3收索引擎页面描述-->
            <title>网页的标题</title><!--4网页的标题-->
        </head>

###head
1. 字符集 
    - 中文能够使用的字符集两种*UTF-8* & *gb2312*：
    - UTF-8：全，各种鸟语都有
    - gb23112：小  只有汉语及常用其他语言

2. 收索引擎关键字

        <meta name="Keywords" content="网易,邮箱,游戏,新闻,体育,娱乐,女性,亚运,论坛,短信" />
    - 这些关键词，就是告诉搜索引擎，这个网页是干嘛的，能够提高搜索命中率。让别人能够找到你，搜索到你。
        Keywords就是“关键词”的意思。

3. 收索引擎页面描述

       <meta name="Description" content="网易是中国领先的互联网技术公司，为用户提供免费邮箱、游戏、搜索引擎服务，开设新闻、娱乐、体育等30多个内容频道，及博客、视频、论坛等互动交流，网聚人的力量。" />
    - 只要设置的Description页面面熟，那么百度搜索结果，就能够显示这些语句，这个技术叫做SEO，search engine optimization，搜索引擎优化。
![Description效果](https://maxiaobu1999.github.io/html5/resources/img/what_is_Description.png)

4. 网页的标题
    - title也是有助于SEO搜索引擎优化的
![title效果](https://maxiaobu1999.github.io/html5/resources/img/what_is_title.png)

HTML标签是分等级的，HTML将所有的标签分为两种：容器级、文本级。
顾名思义，容器级的标签，里面可以放置任何东西；文本级的标签里面，只能放置文字、图片、表单元素。

p标签是一个文本级标签。从学习p的第一天开始，就要死死记住：p里面只能放文字、图片、表单元素。其他的一律不能放。

###body
1. p标签
    - 段落，是英语paragraph“段落”缩写。
    - HTML标签是分等级的，HTML将所有的标签分为两种：容器级、文本级。
    - 顾名思义，容器级的标签，里面可以放置任何东西；文本级的标签里面，只能放置文字、图片、表单元素。
    - p标签是一个文本级标签。从学习p的第一天开始，就要死死记住：p里面只能放文字、图片、表单元素。其他的一律不能放。

2. 图片
    
        <img src="images/baby.jpg" alt="巴黎结婚照" />

3. 超级链接

    	<a href="09_img.html" title="很好看哦" target="_blank">结婚照</a>
    - blank就是“空白”的意思，就表示新建一个空白窗口。为毛有一个_ ，就是规定，没啥好解释的。
    - title 悬停文本

4. 无序列表
```
<ul>
		<li>北京</li>
		<li>上海</li>
		<li>广州</li>
		<li>铁岭</li>
	</ul>
```	
    - ul就是英语unordered list，“无序列表”的意思。
        li 就是英语list item ， “列表项”的意思。
    - “组标签”，就是要么不写，要么就要写一组。
    - 所以的li不能单独存在，必须包裹在ul里面；反过来说，ul的“儿子”不能是别的东西，只能有li
    - li是一个容器级标签，li里面什么都能放
    
5. div和span
    - div和span是非常重要的标签，div的语义是division“分割”； span的语义就是span“范围、跨度”
    - div标签是一个容器级标签，里面什么都能放，甚至可以放div自己
    - span也是表达“小区域、小跨度”的标签，但是是一个“文本级”的标签。
      就是说，span里面只能放置文字、图片、表单元素。 span里面不能放p、h、ul、dl、ol、div。
    - 总结：div换行 span不换行


