# 模板引擎

## 目录
   * 1. [简介](1.1.md)
   * 2. [volt 基本语法](1.2.md)
   * 3. [volt 内置方法](1.3.md)
   * 4. [五星网站主题对象](1.4.md)
   * 5. [小结](1.5.md)



## links
   * [目录](<index.md>)
   * 下一节: [变量引用说明](<变量引用说明.md>)
   
   
   
##简介
   
   Volt 是一个用C编写的超快的并且对设计师友好的模板语言。Volt 提供一组辅助工具有助于你以一种更简单的的方式编写页面模板。
   
   五星网站的页面及模板，都是基于volt模板引擎开发，它是网站页面模板构建的基础，熟练掌握模板引擎的语法结构，可以让你快速地构建自己的个性化网站。
   
   
##基本用法

   下面介绍一些基础用法:
   
###开始与结束标记

   Volt模板在不同的场景使用不同的开始和结束标记: 
   {% 和 %} 用于流程控制语句如if判断、for循环及赋值处理等等    
   {{ 和 }} 用于在模板中输出变量或表达式的执行结果。
   {# 和 #} 用于输出注释内容

###变量（Variables）

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

###过滤器（Filters)
输出变量的时候可以通过为变量添加过滤器来过滤/格式化变量
操作符 | 用于为变量添加过滤器
{{ product.name|e }}
{{ product.name|striptags }}
{{ product.name|capitalize|trim }}

以下是Volt模板内置的过滤器列表:

|过滤器	|	描述|
|:--:|:--:|:--:|:--:|:--:|
|e| 	Applies Phalcon\Escaper->escapeHtml() to the value|
|escape |	Applies Phalcon\Escaper->escapeHtml() to the value|
|escape_css| 	Applies Phalcon\Escaper->escapeCss() to the value|
|escape_js| 	Applies Phalcon\Escaper->escapeJs() to the value|
|escape_attr| 	Applies Phalcon\Escaper->escapeHtmlAttr() to the value|
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
|join |	将一个字符串分割为数组|
|format| 	把格式化的字符串写入变量中（sprintf）|
|json_encode| 	对变量进行 JSON 编码|
|json_decode|  对 JSON 格式的字符串进行解码|
|abs| 	取绝对值|
|url_encode| 	编码 URL 字符串|
|default 	| 为变量设置一个默认值（如果变量为空且不等于false）|
|convert_encoding| 	转换字符编码|

扩展 escasper的方法   扩展length方法

###注释（Comments）

在{# 和 #} 之间的内容在输出的时候将被忽略，只作为注释内容在源码中展示

例如:
输入：

`{# 这里是注释 #}`

输出：

` `


###流程控制
####循环语句for

```
{% set numbers = ['one': 1, 'two': 2, 'three': 3] %}

输入：

{% for value in numbers %}
    Value: {{ value }}<br>    
{% endfor %}

输出：

Value:1
Value:2
Value:3

输入：

{% for name, value in numbers %}
    Name: {{ name }} Value: {{ value }}<br>
{% endfor %}

输出：
Name: one Value:1
Name: two Value:2
Name: three Value:3


```

####条件判断语句if

```
if...else

{# 判断data变量是否为非空 #}
{% if data is not empty %}
	...     
{% else %}
	{% break %}
{% endif %}

if

{# 判断index的值是否等于7 #}
{% if index is 7 %}
     {% break %}
{% endif %}

```
####循环上下文（Loop Context）

循环上下文关键字 loop 在for循环中可用，它就是一个迭代生成器，使用它，你可以方便的进行一些判断和复位操作。
loop中主要包含以下可用变量：

|变量	|	描述|
|:--:|:--:|:--:|:--:|:--:|
|loop.index| 	迭代生成器的当前索引值|
|loop.index0| 	迭代生成器的第一个索引值|
|loop.revindex| 	indexed)|
|loop.revindex0| 	 indexed)|
|loop.first| 	是否是迭代生成器中的第一个元素|
|loop.last| 	是否是迭代生成器中的最后一个元素|
|loop.length| 	迭代生成器中元素个数|

```
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
                <td>{{ robot.id }}</td>
                <td>{{ robot.name }}</td>
            </tr>
    {% if loop.last %}
        </table>
    {% endif %}
{% endfor %}
```

###表达式（Expressions）

volt模板引擎提供表达式支持，包括文字和常见操作符

eg: {{ (1 + 1) * 2 }}

如果表达式结果无需输出显示，则可以在前面加上do操作符

eg：{% do (1 + 1) * 2 %}

###运算符
运算符主要包括算术运算符、比较运算符、逻辑运算符合其它运算符

####算术运算符
|操作符	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|+| 	加法操作|
|-|     减法操作|
|*| 	乘法操作|
|/|     除法操作|
|%| 	取余操作|

####比较运算符
|操作符	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|==| 	等于（数值相同）|
|!=|    不等于|
|<>| 	不等于|
|>|     大于|
|<| 	小于|
|<=|    小于等于|
|>=| 	大于等于|
|===|   全等于（数值和类型都相同）|
|!==| 	不全等于（数值和类型至少有一样不同）|

####逻辑运算符
|操作符	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|or|     或|
|and|    且|
|not| 	 非|
|(expr)|  表达式|

####其它运算符
|操作符	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|~| 	连接两个操作数|
|\||     为最左侧变量增加过滤器|
|..| 	创建一个包含指定范围单元的数组|
|is|     等于|
|in| 	检查表达式是否包含在其他表达式中|
|is not|   不等于|
|'a' ? 'b' : 'c'|   三目运算|
|++| 	自增|
|--| 	自减|

###函数

|方法名	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|content| 	将在此之前输出的内容包含进来|
|get_content| content方法的别名|
|partial| 	在当前模板中动态调取一个代码片段|
|super| 渲染父节点中的内容|
|time| 	返回当前的 Unix 时间戳|
|date| 	格式化输出一个本地时间／日期 |
|dump| 	输出一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。|
|version| 	获取框架的当前版本（禁用）|
|constant| 	读取php的静态变量（禁用）|
|url| 	生成url|




###校验方法

|方法名	|	作用|
|:--:|:--:|:--:|:--:|:--:|
|defined| 	检查变量是否被定义|
|empty| 	检查变量是否为空|
|even| 	检查变量值是否是偶数|
|odd| 	 检查变量值是否是奇数|
|numeric| 	检查变量值是否是数字|
|scalar| 	检测变量是否是一个标量（标量变量是指那些包含了 integer、float、string 或 boolean的变量，而 array、object 和 resource 则不是标量。） |
|iterable| 	检查变量值是否可被迭代生成|
|divisibleby| 	检查变量值是否可被分割|
|sameas| 	检查变量值是否相同|
|type| 	检查变量值是否是给定类型|

eg:

```
{% if robot is defined %}
    The robot variable is defined
{% endif %}

{% if robot is empty %}
    The robot is null or isn't defined
{% endif %}

{% for key, name in [1: 'Voltron', 2: 'Astroy Boy', 3: 'Bender'] %}
    {% if key is even %}
        {{ name }}
    {% endif %}
{% endfor %}

{% for key, name in [1: 'Voltron', 2: 'Astroy Boy', 3: 'Bender'] %}
    {% if key is odd %}
        {{ name }}
    {% endif %}
{% endfor %}

{% for key, name in [1: 'Voltron', 2: 'Astroy Boy', 'third': 'Bender'] %}
    {% if key is numeric %}
        {{ name }}
    {% endif %}
{% endfor %}

{% set robots = [1: 'Voltron', 2: 'Astroy Boy'] %}
{% if robots is iterable %}
    {% for robot in robots %}
        ...
    {% endfor %}
{% endif %}

{% set world = "hello" %}
{% if world is sameas("hello") %}
    {{ "it's hello" }}
{% endif %}

{% set external = false %}
{% if external is type('boolean') %}
    {{ "external is false or true" }}
{% endif %}
```
###~~宏定义（禁用/结合片段使用）~~

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
###~~标签助手，phalcol\tag方法（禁用）~~

###~~包含（include）&&继承(extends、block)&&多重继承（禁用/用于片段）~~

###~~注入服务到模版(禁用)~~






###独立组件&外部资源&扩展（开发）


###缓存视图片段（考虑结合动态片段更新）