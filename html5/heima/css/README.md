##选择器

-  标签选择器
```
<style type="text/css">
	body{
		background-color: yellow;
	}
	span{
		color:red;
	}
	p{
		color:blue;
	}
</style>
```
-  id选择器
```
<style type="text/css">
		#para1{
			color:green;
		}
		#para2{
			color:red;
		}
		#para3{
			color: blue;
		}
	</style>
</head>
<body>
	<p id="para1">我是段落1</p>
	<p id="para2">我是段落2</p>
	<p id="para3">我是段落3</p>
</body>
```
    - id选择器的选择符是“#”   唯一
    - 这个标签的名字，可以任取，但是：
      1） 只能有字母、数字、下划线
      2） 必须以字母开头
      3） 不能和标签同名。比如id不能叫做body、img、a
-   类选择器           
```
<style type="text/css">
		/*用.来表示类*/
		.teshu{
			color: red;
		}
		.zhongyao{
			text-decoration: underline;
		}
	</style>
</head>
<body>
	<h3 class="zhongyao">我是一个h3啊</h3>
	<!--同一个标签，可能同时属于多个类，用空格隔开-->
	<h3 class="teshu zhongyao">我是一个h3啊</h3>
	<h3>我是一个h3啊</h3>
	<p>我是一个段落啊</p>
	<p class="teshu">我是一个段落啊</p>
	<p class="teshu">我是一个段落啊</p>
</body>
```
    - 总结：
      1） class可以重复，也就是说，同一个页面上可能有多个标签同时属于某一个类；
      2） 同一个标签可以同时携带多个类。
      3) js用id   css 用class
- 后代选择器               就是空格
```      
<style type="text/css">
			.div1 p{
				color:red;
			}
		</style>
```
    - 空格就表示后代，.div1 p 就是.div1的后代所有的p。（选择的是后代，不一定是儿子）
- 交集选择器               就是.
```      
<style type="text/css">
	h3.special{
		color:red;
	}
</style>
``` 	
	- 选择的元素是同时满足两个条件：必须是h3标签，然后必须是special标签。（special是类名）
      交集选择器没有空格。
- 并集选择器（分组选择器）    就是,
``` 
<style type="text/css">
        h3,li{
    	color:red;
        }
</style>	
``` 	
- 通配符*
```css
*{
    color:red;
}
```
    - *就表示所有元素
    效率不高，如果页面上的标签越多，效率越低，所以页面上不能出现这个选择器。
- 儿子选择器              就是>
```
	<style type="text/css">
		div>p{
			color:red;
		}
	</style>
```	
    - div的儿子p。和div的后代p的截然不同
    - IE7开始兼容，IE6不兼容。
- 序选择器	
```
ul li:first-child{
	color:red;
}
ul li:last-child{
	color:blue;
}	
```	
    - IE8开始兼容；IE6、7都不兼容
- 下一个兄弟选择器 就是+
```	
<style type="text/css">
	h3+p{
		color:red;
	}
</style>	
```
	- +表示选择下一个兄弟
	
##CSS的继承性和层叠性	
###继承性
- 有一些属性，当给自己设置的时候，自己的后代都继承上了，这个就是继承性。
- 哪些属性能继承？
    color、 text-开头的、line-开头的、font-开头的
    这些关于文字样式的，都能够继承； 所有关于盒子的、定位的、布局的属性都不能继承。
	
###层叠性
- 就是css处理冲突的能力。 所有的权重计算，没有任何兼容问题！
- 权重问题大总结：
  1） 先看有没有选中，如果选中了，那么以（id数，类数，标签数）来计权重。谁大听谁的。如果都一样，听后写的为准。
  2） 如果没有选中，那么权重是0。如果大家都是0，就近原则。
-  !important标记
    - 一个属性提高权重。这个属性的权重就是无穷大
    - font-size:60px !important;     写在;之前
    - 注意 
    !important提升的是一个属性，而不是一个选择器
	!important无法提升继承的权重，该是0还是0
	!important不影响就近原则
	
##盒模型
####padding
- 清除默认的padding：
```
<style type="text/css">
	    body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
		margin:0;
		padding:0;
	}
</style>
```	

####border	“边框”
```
border: 1px dashed red;
```
- 三个要素：粗细、线型、颜色
	![border样式](https://maxiaobu1999.github.io/html5/resources/img/borderStyle.png)

##标准文档流
####块级元素和行内元素	

- 标签分为两种等级：
    1） 块级元素
    ● 霸占一行，不能与其他任何元素并列
    ● 能接受宽、高
    ● 如果不设置宽度，那么宽度将默认变为父亲的100%。
    2） 行内元素
    ● 与其他行内元素并排
    ● 不能设置宽、高。默认的宽度，就是文字的宽度。
- 块级元素和行内元素的相互转换
```
div{
	display: inline;
	background-color: pink;
	width: 500px;
	height: 500px;
}
```
    - display: inline;
      那么，这个标签将立即变为行内元素。此时它和一个span无异：
      ● 此时这个div不能设置宽度、高度；
      ● 此时这个div可以和别人并排了
```
span{
	display: block;
	width: 200px;
	height: 200px;
	background-color: pink;
}
```	
	- “block”是“块”的意思
      让标签变为块级元素。此时这个标签，和一个div无异：
      ● 此时这个span能够设置宽度、高度
      ● 此时这个span必须霸占一行了，别人无法和他并排
      ● 如果不设置宽度，将撑满父亲	
      
##脱离标准文档流
####浮动
```	
.box1{
	float: left;
	width: 300px;
	height: 400px;
	background-color: yellowgreen;
}
.box2{
	float: left;
	width: 400px;
	height: 400px;
	background-color: skyblue;
}
```	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	