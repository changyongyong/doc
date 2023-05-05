# 选择器

## 常用选择器
```
属性选择器，选取具有name属性的元素
[name]
选取具有某个属性的元素:
[属性名] 

属性选择器，选取属性target值为_blank的元素
[target=_blank]
[属性名=属性值] 
选取某个属性等于某个值的元素	等于= 

属性选择器，请写出name属性的值中包含"user"的元素
[name~=user]
[属性名~=某某内容]
选取某属性的值中包含某某内容的元素。约等于~   

属性选择器，选取属性name的值以"user"开头的元素
[name^=user]
以某某开头^ 
[属性名^=值]
代表选择某某属性以某某值开头的元素

属性选择器，选取属性src的值中以png结尾的元素
[src$=png]
以某某结尾$
[属性名$=值]
代表选择某某属性以某某值结尾的元素

属性选择器，选取属性value值中包含un的元素
[value*=un]
包含*
[属性名*=值]
代表选择某某属性包含某某值的元素

属性选择器，选取p标签中具有name属性的元素，让他的背景颜色变红
p[name]{
    background-color: red;
}

属性选择器，找到属性名为name，属性值为noname的标签，让它背景颜色变蓝
    [name=noname]{
        background-color: blue;
    }

属性选择器，找到属性名为name，属性值为noname的span标签，让它背景颜色变蓝
    span[name=noname]{
        background-color: blue;
    }

属性选择器，找到属性class值中包含c2样式的标签，让字号变为50px
    [class~=c2]{
    font-size:50px;
    }

并集选择器的符号
，

层级选择器的符号
空格

子选择器的符号
>

畔邻选择器的符号
+

nth选择器，的使用格式
选择器:nth-child(序号)
备注，选择器所在层级的成员会编号，第一个为一号
如果序号所对应的标签对象也正好符合选择器，则选择成功，否则选择失败
额外扩展 nth-last-child(n)，倒着数


:nth-of-type的用法
选择器：nth-of-type（序号）
备注，选择器所在的层，先会找到符合选择器的全部成员，再编号，起点是1号。
然后根据填入的序号拿到对应的元素

:nth-last-of-type的用法
选择器：nth-last-of-type（序号）

:nth选择器，获取偶数行
p:nth-child(2n)
```

## 选择器的回顾
标签选择器 。。。div *
类选择器 。。。 .inner{}
ID选择器 。。。#inner{}
层级选择器。。。拿p标签下的id为name的标签 p #name{}
并级选择器。。。 同时设置div标签以及p标签的样式 div,p{}
属性选择器

## 主要标识
```
选取具有某个属性的元素
[属性名] {样式} 

等于= 选取某个属性等于某个值的元素
[属性名=属性值] {样式}

约等于~   选取属性中包含某某值的元素
[属性名~=属值名]

以某某开头^ 
[属性名^=值]
代表选择某某属性以某某值开头的元素

以某某结尾$
[属性名$=值]
代表选择某某属性以某某值结尾的元素

包含*
[属性名*=值]
代表选择某某属性包含某某值的元素

选取p标签中具有name属性的元素
    p[name]{
        background-color: red;
    }

找到属性名为name，属性值为noname的标签，让它变蓝
    [name=noname]{
        background-color: blue;
    }

找到属性名为name，属性值为noname的span标签，让它变蓝
    span[name=noname]{
        background-color: blue;
    }

找到属性class值中包含c2样式的标签，让字号变为50px
    [class~=c2]{
    font-size:50px;
    }
```

```
<style>
    [name]{
        background-color: red;
    }

    
</style>


<div class="c1">c1</div>

<div class="c2">c2</div>

<div name="username">username</div>
```

## 组合选择器

逗号，并级选择器

空格，层级选择器

> ，子选择器
加号  畔邻选择器


## 后代选择器 vs 子元素选择器
后代选择器，影响所有的后代。   
子元素选择器，只会影响儿子一级，不会对孙子一级造成影响

## :nth-child(n)
th有着第几个的意思
n代表一个数量
第若干个-子元素

```
p:nth-child(3)

代表的意思是，先找到p标签
拿到p标签的父一级
然后再根据这个父级，查找他的子元素
第一个子元素，序号为1
第2个为2
第n个为n

根据这个样的计数原则
我们可以找到对应的标签

根据计数找到了对应的元素后
该元素还得符合选择器的条件才可以
```

## :nth-last-child(n)


## :nth-of-type(n)
```
选择器：nth-of-type(序号）

寻找方式
先找到选择器
根据选择器找到父级
再从父级的角度，向下数儿子
只有符合选择器条件的儿子，才是有序号的儿子
再对儿子进行编号，第一个是1，第二个是2，第n个是n
最终得到的标签对象，可以获得样式
```

## :nth-last-of-type(n）
```
<style>
p:nth-last-of-type(1)
{
	background:#ff0000;
}
</style>
</head>
<body>

<h1>This is a heading</h1>
<p>The 1 paragraph.</p>
<p>The 2 paragraph.</p>
<p>The 3 paragraph.</p>
<p>The 4 paragraph.</p>

</body>
```
## nth系列中n的应用
我们可以把()中写成这种形式
2n
2n+1
















