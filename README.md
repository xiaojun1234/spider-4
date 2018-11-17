# spider-4
# BeautifulSoup基本使用
使用lxml解析库，如此出现复杂和乱的网站，可以使用html.parser解析库
<pre>
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters sisters
<a href="http://example.com" class="sister" id="link1"><!-- Eisie--></a>,
<a href="http://example.com" class="sister" id="link2">Lacle</a>and
<a href="http://example.com" class="sister" id="link3">Tillie</a>;
and they lived at.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.prettify())
print(soup.title.string)
</pre>
# 标签选择器
## 选择元素
<pre>
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters sisters
<a href="http://example.com" class="sister" id="link1"><!-- Eisie--></a>,
<a href="http://example.com" class="sister" id="link2">Lacle</a>and
<a href="http://example.com" class="sister" id="link3">Tillie</a>;
and they lived at.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.title)
print(type(soup.title))
print(soup.head)
print(soup.p)
</pre>
## 获得名称
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.title.name)
</pre>
## 获得属性
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.p.attrs['name'])     /*p标签的属性*/
print(soup.p['name'])
</pre>
## 获得内容
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.p.string)
print(soup.head.string)
</pre>
## 嵌套选择
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.head.title.string)           /*head里包含了title,所以可以使用迭代来选择*/
</pre>
# 子节点和子孙节点
## 子节点
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.p.contents)
另一种子节点(通过children迭代器)
from bs4 import BeautifulSoup
soup = BeautifulSoup(html.'lxml')
print(soup.p.children)
for i,child in enumerate(soup.p.children):
    print(i,child)
</pre>
## 子孙节点
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.p.descendants)
for i,child in enumerate(soup.p.descendants):
    print(i,child)
</pre>
# 父节点和祖先节点
## 父节点
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.a.parent)                      /*a标签的父节点*/
</pre>
## 祖先节点
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(list(enumerate(soup.a.parents)))
</pre>
## 兄弟节点
<pre>
from bs4 import BeautifulSoup 
soup = BeautifulSoup(html,'lxml')
print(list(enumerate(soup.a.next_siblings)))
print(list(enumerate(soup.a.previous_siblings)))
</pre>
# 标准选择器(find_all(name,attrs,recursive,text,**kwargs))
<pre>
name:
html='''
<div class="panel">
    <div>
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.find_all('ul'))
print(type(soup.find_all('ul')[0]))
</pre>
## 加强版(进行迭代)
<pre>
from bs4 import beautifuSoup
soup = BeautifulSoup(html,'lxml')
for i in soup.find_all('ul'):
    print(soup.find_all('li'))
</pre>
# attrs:
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.find_all(attrs={'id':'list-1'}))
print(soup.find_all(attrs={'name','elements'}))
</pre>
## 也可以简化：
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.find_all(id='list-1'))
print(soup.find_all(class_='element'))     /*class得加下划线*/
</pre>
# text:
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.find_all(text='Foo'))      /*返回的只是匹配的内容*/
</pre>
# find() 用法和find_all()一样，只是返回的只有一个元素
<pre>
find_parents()                 /*返回所有祖先节点*/
find_parent()                 /*返回直接父节点*/
find_next_siblings()     /*返回后面所有兄弟节点*/
find_next_sibling()       /*返回后面一个兄弟节点*/
find_previous_siblings()      /*返回前面所有兄弟节点*/
find_previous_sibling()        /*返回前面一个兄弟节点*/
find_all_next()              /*返回节点后所有符合条件的节点*/
find_next()             /*返回节点后符合条件的第一个节点*/
find_all_prevous()          /*返回节点前所有符合条件的节点*/
find_previous()          /*返回节点前符合条件的第一个节点*/
</pre>

# css选择器(用这个更简单)
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.select('.panel .panel-heading'))  /*class参数得在前面加‘.’*/
print(soup.select('ul li'))       /*这里也算遍历，先输出ul,再输出li*/      /*标签，直接输入*/
print(soup.select('#list-2 .element'))        /*id参数得在前面加'#'*/
print(type(soup.select('ul')[0]))
</pre>
# 遍历演示
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
for ul in soup.select('ul'):
    print(soup.select('li'))
</pre>
# 获得属性
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
for ul in soup.select('ul')
    print(ul['id'])               /*直接 [ 想查的属性  ] */
    print(ul.attrs['id'])
</pre>
# 获得内容
<pre>
from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
for li in soup.select('li')
    print(li.get_text())              /*使用li的get_text函数*/
</pre>
