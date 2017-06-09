# 模板引擎

## 目录
  1. [简介](#1)
  2. [基本语法](#2)
     1. [开始与结束标记](#2.1)
     2. [变量（Variables）](#2.2)
     3. [过滤器（Filters)](#2.3)
     4. [注释（Comments）](#2.4)
     5. [流程控制](#2.5)
     6. [表达式（Expressions）](#2.6)
     7. [运算符](#2.7)
     8. [函数](#2.8)
     9. [校验方法](#2.9)
     10. [~~宏定义（禁用/结合片段使用）~~](#2.10)
    

   
<h2 id="1">简介</h2>
   
   Volt 是一个用C编写的超快的并且对设计师友好的模板语言。Volt 提供一组辅助工具有助于你以一种更简单的的方式编写页面模板。
   
   五星网站的页面及模板，都是基于volt模板引擎开发，它是网站页面模板构建的基础，熟练掌握模板引擎的语法结构，可以让你快速地构建自己的个性化网站。
   
   
<h2 id="2">基本用法</h2>

   下面介绍一些基础用法:
   
<h3 id="2.1">开始与结束标记</h3>

   Volt模板在不同的场景使用不同的开始和结束标记: 

   {% 和 %} 用于流程控制语句如if判断、for循环及赋值处理等等  

   {{ 和 }} 用于在模板中输出变量或表达式的执行结果。

   {# 和 #} 用于输出注释内容

<h3 id="2.2">变量（Variables）</h3>

变量有三种：字符变量、对象变量、数组变量

```
字符变量赋值：
{% set title = '我是标题' %}
访问字符变量：{{ title }}

```

```
对象变量赋值：
{% set product = Object %}
通过foo.bar的形式访问变量属性:
{{ product.name }}

```
数组的几种形式：

```
{# 创建简单数组 #}
{% set array = ['Apple', 1, 2.5, false, null] %}

{# 创建多维数组 #}
{% set array = [[1, 2], [3, 4], [5, 6]] %}

{# 创建关联数组 #}
{% set array = ['first': 1, 'second': 4/2, 'third': '3'] %}

通过foo['bar']的形式访问数组变量：

{{ array[0] }} 

{{ array['first'] }} 

```

<h3 id="2.3">过滤器（Filters)</h3>
输出变量的时候可以通过为变量添加过滤器来过滤/格式化变量

操作符 | 用于为变量添加过滤器

{{ product.name|e }}

{{ product.name|striptags }}

{{ product.name|capitalize|trim }}

以下是Volt模板内置的过滤器列表:

|过滤器	|	描述|
|:--:|:--:|
|trim| 删除左右两侧多余的空格|
|left_trim| 删除左侧多余的空格|
|right_trim| 删除右侧多余的空格|
|striptags| 删除变量中的html标记|
|slashes| 	在字符串中的单引号（'）、双引号（"）、反斜线（\）与 NUL字符前加上反斜线|
|stripslashes| 	去除字符串中的转义反斜线|
|capitalize| 	将字符串中每个单词的首字母转换为大写|
|lower| 将变量中的字符转换为小写|
|upper| 将变量中的字符转换为大写|
|length| 	计算字符长度或数组与对象的数量|
|nl2br| 在字符串所有新行之前插入 HTML 换行标记|
|sort| 	对数组进行排序并保持索引关系|
|keys| 	返回数组的所有键值|
|join |	将一个数组分割为字符串|
|format| 	把格式化的字符串写入变量中（sprintf）|
|json_encode| 	对变量进行 JSON 编码|
|json_decode|  对 JSON 格式的字符串进行解码|
|abs| 	取绝对值|
|url_encode| 	编码 URL 字符串|
|default 	| 为变量设置一个默认值（如果变量为空或未设置）|
|convert_encoding| 	转换字符编码|

`trim` 

输入：

    {% set name=' 小明 ' %}
    {{ name|trim }}

输出：

    小明

`left_trim` 

输入：

    {% set name=' 小明 ' %}
    {{ name|left_trim }}

输出：

    小明 

`right_trim` 

输入：

    {% set name=' 小明 ' %}
    {{ name|right_trim }}

输出：
  
     小明

`striptags`

  输入：

    {% set name='<div>小明</div>' %}
    {{ name|striptags }}

  输出：

    小明

`slashes`

输入：

    {% set name='"小明"' %}
    {{ name|slashes }}

输出：

    \"小明\"


`capitalize`

输入：

    {% set name='my name' %}
    {{ name|capitalize }}

输出：

    My Name

`lower`

输入：

    {% set name='MY Name' %}
    {{ name|lower }}

输出：

    my name

`upper`

输入：

    {% set name='my name 你好' %}
    {{ name|upper }}

输出：

    MY NAME 你好

`length`

输入：

    {% set data=['小明','小红','小强'] %}
    {{ data|length }}
    
    {% set data='小强' %}
    {{ data|length }}
    
    {% set data='abcd' %}
    {{ data|length }}

输出：

    3
    2
    4

`nl2br`

输入：

    {% set name='a
    b
    c' %}
    {{ name|nl2br }}

输出：

    a<br />
    b<br />
    c

`sort`

输入：

    {% set data=['a':3,'b':1,'c':2] %}
    {% set data=data|sort %}
    {{ dump(data) }}

输出：

    array(3) { ["b"]=> int(1) ["c"]=> int(2) ["a"]=> int(3) } 


`keys`

输入：

    {% set data=['a':3,'b':1,'c':2] %}
    {% set data=data|keys %}
    {{ dump(data) }}

输出：

    array(3) { [0]=> string(1) "a" [1]=> string(1) "b" [2]=> string(1) "c" } 

`join`

输入：

    {% set data=['a':3,'b':1,'c':2] %}
    {{ data|join(",") }}

输出：

    3,1,2


`format`

输入：
    
    {% set myname='xiaoming' %}
    {{ "My real name is %s"|format(myname) }}

输出：

    My real name is xiaoming

`json_encode`

用法示例：

    {% set encoded = robots|json_encode %}


`json_decode`

用法示例：

    {% set decoded = '{"one":1,"two":2,"three":3}'|json_decode %}

`abs`

用法示例：

输入：

    {% set a = 2 %}
    {{ a|abs }}
    {% set a = -3 %}
    {{ a|abs }}

输出：

    2
    3

`url_encode`

用法示例：

输入：

    {% set url = 'http://www.baidu.com' %}
    {{ url|url_encode }}

输出：

    http%3A%2F%2Fwww.baidu.com

`default`

用法示例：

输入：

    {% set name = '小红' %}
    {{ name|default('小明') }}
    {% set name = '' %}
    {{ name|default('小明') }}

输出：

    小红
    小明


`convert_encoding`

用法示例：

    {# 从EUC-JP编码转换为UTF-7 #}
    {{ "abasds"|convert_encoding("UTF-7", "EUC-JP") }}


<h3 id="2.4">注释（Comments）</h3>

在{# 和 #} 之间的内容在输出的时候将被忽略，只作为注释内容在源码中展示

在源码中输入以下内容：

输入：

    {# 这里是注释 #}
    <div>abcd</div>

输出：

    <div>abcd</div>

<h3 id="2.5">流程控制</h3>
<h4>循环语句for</h4>

输入：

    {% set data = ['one': 1, 'two': 2, 'three': 3] %}
    {% for value in data %}
        Value: {{ value }}<br>    
    {% endfor %}

输出：

    Value:1
    Value:2
    Value:3

输入：

    {% for name, value in data %}
        Name: {{ name }} Value: {{ value }}<br>
    {% endfor %}

输出：

    Name: one Value:1
    Name: two Value:2
    Name: three Value:3

<h4>条件判断语句if</h4>

`if...else`

    {# 判断data变量是否为非空 #}
    {% if data is not empty %}
	     ...     
    {% else %}
	     {% break %}
    {% endif %}

`if`

    {# 判断index的值是否等于7 #}
    {% if index is 7 %}
        {% break %}
    {% endif %}

<h4>循环上下文（Loop Context）</h4>

循环上下文关键字 loop 在for循环中可用，使用它，你可以方便的进行一些判断和计数操作。
我们可以把它看做是一个循环计数器，用它来记录元素在当前循环中的位置。
loop中主要包含以下可用属性：

|变量	|	描述|
|:--:|:--:|
|loop.index| 	   当前元素在从1开始计数的循环计数器中的位置|
|loop.index0| 	 当前元素在从0开始计数的循环计数器中的位置|
|loop.revindex|  当前元素在逆向从1开始计数的循环计数器中的位置|
|loop.revindex0| 当前元素在逆向从0开始计数的循环计数器中的位置|
|loop.first| 	是否是循环中的第一个元素|
|loop.last| 	是否是循环中的最后一个元素|
|loop.length| 	循环中的元素个数|

用法示例：

`loop.index`

输入：

    {% set  robots= ['a','b','c'] %}
    {% for robot in robots %}
        {{ loop.index }} 
    {% endfor %}
    <br />
    {% for robot in robots %}
        {{ loop.index0 }} 
    {% endfor %}
    <br />
    {% for robot in robots %}
        {{ loop.revindex }} 
    {% endfor %}
    <br />
    {% for robot in robots %}
        {{ loop.revindex0 }} 
    {% endfor %}

输出

    1 2 3
    0 1 2
    3 2 1
    2 1 0 

    {% set robots = [['id':0,'name':'a'],['id':1,'name':'b'],['id':2,'name':'c']] %}
    {% for robot in robots %}
      {% if loop.first %}
          <table>
              <tr>
                  <th>#</th>
                  <th>Id</th>
                  <th>Name</th>
              </tr>
      {% endif %}
              <tr>
                  <td>{{ loop.index }}</td>
                  <td>{{ robot['id'] }}</td>
                  <td>{{ robot['name'] }}</td>
              </tr>
      {% if loop.last %}
          </table>
      {% endif %}
    {% endfor %}

<h3 id="2.6">表达式（Expressions）</h3>

volt模板引擎提供表达式支持，包括文字和常见操作符

输入: 

    {{ (1 + 1) * 2 }}

输出：

    4

如果表达式结果无需输出显示，则可以在前面加上do操作符

输入：

    {% do (1 + 1) * 2 %}

输出：

    此时只会执行表达式，不会有任何输出

<h3 id="2.7">运算符</h3>
运算符主要包括算术运算符、比较运算符、逻辑运算符合其它运算符

<h4>算术运算符</h4>

|操作符	|	作用|示例
|:--:|:--:|:--:|
|+| 	加法操作|{% a+b %}|
|-|   减法操作|{% a-b %}|
|*| 	乘法操作|{% a*b %}|
|/|   除法操作|{% a/b %}|
|%| 	取余操作|{% a%b %}|

<h4>比较运算符</h4>

|操作符	|	作用|
|:--:|:--:|
|==| 	等于（元素值相同）|
|!=|    不等于|
|<>| 	不等于|
|>|     大于|
|<| 	小于|
|<=|    小于等于|
|>=| 	大于等于|
|===|   全等于（元素值和类型都相同）|
|!==| 	不全等于（元素值和类型至少有一样不同）|

<h4>逻辑运算符</h4>

|操作符	|	作用|
|:--:|:--:|
|or|     或|
|and|    且|
|not| 	 非|
|(expr)|  表达式|

<h4>其它运算符</h4>

|操作符	|	作用|
|:--:|:--:|
|~| 	连接两个操作数|
|&#124;|  为最左侧变量增加过滤器|
|..| 	创建一个包含指定范围单元的数组|
|is|     等于|
|in| 	检查表达式是否包含在其他表达式中|
|is not|   不等于|
|'a' ? 'b' : 'c'|   三目运算|
|++| 	自增|
|--| 	自减|

<h3 id="2.8">函数</h3>

|方法名	|	作用|
|:--:|:--:|
|content| 	将在此之前输出的内容包含进来（重写？）|
|get_content| content方法的别名（重写？）|
|partial| 	在当前模板中动态调取一个代码片段（重写？）|
|super| 获取（渲染）父模板中的内容（重写？）|
|time| 	返回当前的 Unix 时间戳|
|date| 	格式化输出一个本地时间／日期 |
|dump| 	输出一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。|
|version| 	获取框架的当前版本（禁用）|
|constant| 	读取php的静态变量（禁用）|
|url| 	生成url（禁用）|

`super`

用法示例：

      {# main.volt #}
      <!DOCTYPE html>
      <html>
          <head>
              <title>Title</title>
          </head>
          <body>
              {% block content %}<h1>Table of contents</h1>{% endblock %}
          </body>
      </html>


      {# layout.volt #}
      {% extends "main.volt" %}
      {% block content %}
          {{ super() }}
          <h2>contents 2</h2>
      {% endblock %}

渲染后输出：

      <!DOCTYPE html>
      <html>
          <head>
              <title>Title</title>
          </head>
          <body>
              <h1>Table of contents</h1>
              <h2>contents 2</h2>
          </body>
      </html>

`time`

用法示例：

    {% set now = time() %}

`date`

用法示例：

输入：

    {{ date('Y年m月d日 H:i:s',1496222729) }}
输出：

    2017年5月31日 17:25:29

`dump`

用法示例：

输入：

    {% set data = ['one': 1, 'two': 2, 'three': 3] %}
    {{ dump(data) }}
输出：

    array(3) { ["one"]=> int(1) ["two"]=> int(2) ["three"]=> int(3) } 

<h3 id="2.9">校验方法</h3>

|方法名	|	作用|
|:--:|:--:|
|defined| 	检查变量是否被定义|
|empty| 	检查变量是否为空（未定义、空字符串、空数组、0、null、false等均认为为空）|
|even| 	检查变量值是否是偶数|
|odd| 	 检查变量值是否是奇数|
|numeric| 	检查变量值是否是数字|
|scalar| 	检测变量是否是一个标量（标量变量是指那些包含了 integer、float、string 或 boolean的变量，而 array、object 和 resource 则不是标量。） |
|iterable| 	检查变量值是否可被迭代生成|
|divisibleby| 	检查变量值是否可被整除|
|sameas| 	检查变量值是否相同|
|type| 	检查变量值是否是给定类型|

`defined`

用法示例：

    {% if robot is defined %}
        The robot variable is defined
    {% endif %}

`empty`

用法示例：

    {% if robot is empty %}
        The robot is null or isn't defined
    {% endif %}

`even`

用法示例：

    {% for key, name in ['Voltron', 'Astroy Boy', 'Bender'] %}
        {% if key is even %}
            {{ name }}
        {% endif %}
    {% endfor %}

`odd`

用法示例：

    {% for key, name in ['Voltron', 'Astroy Boy', 'Bender'] %}
        {% if key is odd %}
            {{ name }}
        {% endif %}
    {% endfor %}

`numeric`

用法示例：

    {% for key, name in ['Voltron', 'Astroy Boy', 'third': 'Bender'] %}
        {% if key is numeric %}
            {{ name }}
        {% endif %}
    {% endfor %}

`scalar`

用法示例：
输入：

    {% set robots = 'a' %}
    {% if robots is scalar %}
        {{ 'True' }}
    {% endif %}

    {% set robots = ['a','b'] %}
    {% if robots is not scalar %}
        {{ 'True' }}
    {% endif %}
输出：

    True
    True

`iterable`

用法示例：

    {% set robots = [1: 'Voltron', 2: 'Astroy Boy'] %}
    {% if robots is iterable %}
        {% for robot in robots %}
            {{ robot }}
        {% endfor %}
    {% endif %}

`divisibleby`

用法示例：

    {% set robots = 10 %}
    {% if robots is divisibleby(5) %}
            10 can divisible by 5
    {% endif %}


`sameas`

用法示例：

    {% set world = "hello" %}
    {% if world is sameas("hello") %}
        {{ "it's hello" }}
    {% endif %}

`type`

用法示例：

    {% set external = false %}
    {% if external is type('boolean') %}
        {{ "external is false or true" }}
    {% endif %}

<h3>宏定义（禁用/结合片段使用）</h3>

宏定义可以重用模板逻辑，和其它方法一样可以接收参数和返回值
```
{# Macro "display a list of links to related topics" #}
{%- macro related_bar(related_links) %}
    <ul>
        {%- for link in related_links %}
            <li>
                <a href="{{ url(link.url) }}" title="{{ link.title|striptags }}">
                    {{ link.text }}
                </a>
            </li>
        {%- endfor %}
    </ul>
{%- endmacro %}

{# Print related links #}
{{ related_bar(links) }}
{# Print related links again #}

{{ related_bar(links) }}
```
<h3>标签助手，phalcol\tag方法（禁用）</h3>

<h3>包含（include）&&继承(extends、block)&&多重继承（禁用/用于片段）</h3>

<h3>注入服务到模版(禁用)</h3>

<h3>独立组件&外部资源&扩展（开发）</h3>

<h3>缓存视图片段（考虑结合动态片段更新）</h3>


## links
   * [目录](<index.md>)
   * 下一节: [变量引用说明](<变量引用说明.md>)
