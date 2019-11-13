# RegExp 正则表达式 

工欲善其事必先利其器
## Tools 工具
* [在线 正则表达式 图表化工具](https://regexper.com/)
* [在线 正则表达式测试](http://tool.oschina.net/regex/)
* [在线 IDE](http://jsbin.com/?js,console)

## Syntax 语法
```
/pattern/flags
new RegExp(pattern [, flags])
RegExp(pattern [, flags])
```
### flags 通过标志进行高级搜索
|标志|描述|
|:--|:--|
|g|全局|
|i|不分大小写|
|m|多行|
|s|dotAll模式，匹配任何字符（包括终止符 '\n'）/ 允许 . 匹配换行符|
|u|使用unicode码的模式进行匹配|
|y|执行“粘性”搜索,匹配从目标字符串的当前位置开始，可以使用y标志|


## Create tips 创建注意事项
When using the constructor function, the normal string escape rules (preceding special characters with \ when included in a string) are necessary. For example, the following are equivalent:

当使用构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \）。比如，以下是等价的：

```
var re = /\w+/;
var re = new RegExp('\\w+');
```

## Character Classes 字符类别 / 预定义类（匹配常见的字符类）
|字符|等价|含义|
|:---|:---|:---|
|.|[^\r\n]|(点号，小数点)匹配任意单个字符，但是行结束符除外：\n \r \u2028 或 \u2029。<br/><br/>在字符集中，点( . )失去其特殊含义，并匹配一个字面点( . )。<br/>需要注意的是，m多行（multiline）标志不会改变点号的表现。因此为了匹配多行中的字符集，可使用[^]（当然你不是打算用在旧版本 IE 中），它将会匹配任意字符，包括换行符。<br/><br/>例如，/.y/ 匹配 "yes make my day" 中的 "my" 和 "ay"，但是不匹配 "yes"。|
|\d|[0-9]|数字字符|
|\D|[^0-9]|非数字字符|
|\w|[a-zA-Z_0-9]|单词字符(字母、数字、下划线)|
|\W|[^a-zA-Z_0-9]|非单词字符|
|\s|[\t\n\xOB\f\r]|空白符<br/>包括空格、制表符、换页符、换行符和其他 Unicode 空格|
|\S|[^\t\n\xOB\f\r]|非空白符|
|\t||tab 水平制表符|
|\v||vertical tab 垂直制表符|
|\r||carriage return 回车符|
|\n||linefeed 换行符|
|\f||form-feed 换页符|
|[\b]||退格符（backspace）（不要与 \b 混淆）|
|\0||NUL 字符<br/>不要在此后面跟小数点。|

## Character Sets 字符集合
|字符|含义|
|:---|:---|
|[xyz]|一个字符集合，也叫字符组。匹配集合中的任意一个字符。你可以使用连字符'-'指定一个范围。<br/><br/>例如，[abcd] 等价于 [a-d]，匹配"brisket"中的'b'和"chop"中的'c'。|
|[^xyz]|一个反义或补充字符集，也叫反义字符组。也就是说，它匹配任意不在括号内的字符。你也可以通过使用连字符 '-' 指定一个范围内的字符。<br/><br/>例如，[^abc] 等价于 [^a-c]。 第一个匹配的是 "bacon" 中的'o' 和 "chop" 中的 'h'。|

### 辅助理解
### 元字符
<pre> 正则表达式由两种字符类型组成：
  - 原义文本字符（abc）
  - 元字符（\b）
 元字符是在正则表达式中有特殊含义的非字母字符
 `* + ？ ^ $ . | \ ( ) { } [ ]`
 </pre>
#### 字符类
<pre>
我们可以使用元字符 `[ ]` 来创建一个简单的类
所谓类是指符合某些特性的对象，一个泛指，而不是特指某个字符
表达式[abc]把字符a或b或c归为一类，表达式可以匹配这类的字符
</pre>

#### 字符类取反
<pre>
使用元字符 `^` 创建反向类/负向类
反向类的意思是不属于某类的内容
表达式 `[^abc]`表示 不是字符a或b或c的内容
</pre>

#### 范围类
<pre>
[a-z]表示从a到z的任意字符，这是个闭区间，也就是包含a和z本身
同时范围类内包含字符 `-` ：如 `[0-9-]` 
"2012-08-08".replace(/[0-9-]/,'');
</pre>


## Boundaries 边界字符
|字符|含义|
|:---|:---|
|^|以xxx开始|
|$|以xxx结束|
|\b|单词边界|
|\B|非单词边界|

eg:
`\b`<br/>
例如，/\bno/ 匹配 "at noon" 中的 "no"，/ly\b/ 匹配 "possibly yesterday." 中的 "ly"。<br/>
`\B`<br/>
例如，/\Bon/ 匹配 "at noon" 中的 "on"，/ye\B/ 匹配 "possibly yesterday." 中的 "ye"。<br/>


## Grouping 分组
|字符|含义|
|:---|:---|
|(x)|匹配 x 并且捕获匹配项。<br/>这被称为捕获括号（capturing parentheses）。<br/><br/>例如，/(foo)/ 匹配且捕获 "foo bar." 中的 "foo"。被匹配的子字符串可以在结果数组的元素 [1], ..., [n] 中找到，或在被定义的 RegExp 对象的属性 $1, ..., $9 中找到。<br/><font color="red"><pre>捕获组（Capturing groups）有性能惩罚。</pre></font>如果不需再次访问被匹配的子字符串，最好使用非捕获括号（non-capturing parentheses），见下面。|
|(\n)|n 是一个`正整数`。一个反向引用（back reference），指向正则表达式中第 n个括号（从左开始数）中匹配的子字符串。<br/><br/>例如，/apple(,)\sorange\1/ 匹配 "apple, orange, cherry, peach." 中的 "apple,orange,"。一个更全面的例子在该表格下面。|
|(?:)|匹配 x 不会捕获匹配项。这被称为非捕获括号（non-capturing parentheses）。匹配项不能够从结果数组的元素 [1], ..., [n] 或已被定义的 RegExp 对象的属性 $1, ..., $9 再次访问到。|

### 辅助理解
#### 分组-量词
<pre>
使用 `( )` 可以达到分组的功能，使量词作用于分组
'a1b2c3d4'.replace(/([a-zA-Z]\d){3}/g,'X')
'Xd4'
</pre>

####  分组-或
<pre>
`|` 表示或者
 'abcdef'.replace(/abc|def/g,'X')
 // 'XX'
'abcfabdf'.replace(/ab(c|d)f/g,'X')
// 'XX'
</pre>

#### 分组-反向引用（$1、$2、$3...）
<pre>
'2019-09-10'.replace(/(\d{4})-(\d{2})-(\d{2})/g,'$2/$3/$1')
// "09/10/2019"
</pre>

#### 忽略分组
<pre>不希望捕获某些分组，只需要在分组内前面加上 `?:` 就可以
'abcdef'.replace(/(ab)(?:cd)(ef)/g,'$1-$2-$3')
// "ab-ef-$3"
</pre>

## Quantifiers 量词
|字符|含义|
|:---|:---|
|?|出现0或1次（最多1次）|
|+|等价:{1,} <br/>出现1或多次（至少1次）|
|*|出现0或多次（任意次）|
|{n}|出现n次|
|{n,m}|出现n到m次|
|{n,}|至少出现n次|
|{0,m}|至多出现m次|

## 贪婪模式
<pre>正则表达式尽可能多的匹配
量词默认是贪婪模式
'12345678'.replace(/\d{3,6}?/,'X')
// "X78"
</pre>

## 非贪婪模式
<pre>
正则表达式尽可能少的匹配
量词后加上 `?` 是非贪婪模式
'12345678'.replace(/\d{3,6}?/,'X')
// "X45678"
</pre>

## Assertions 前瞻 
(下面所有断言均只匹配exp，assert不参与匹配)
|正则断言|含义|
|:-|:-|:-|
|x(?=y)|仅匹配被y跟随的x。<br/><br/>举个例子，`/Jack(?=Sprat)/`，如果"Jack"后面跟着sprat，则匹配之。<br/><br/>`/Jack(?=Sprat|Frost)/`，如果"Jack"后面跟着"Sprat"或者"Frost"，则匹配之。但是，"Sprat" 和"Frost"都不会在匹配结果中出现。|
|x(?!y)|仅匹配不被y跟随的x。<br/><br/>举个例子，`/\d+(?!\.)/`只会匹配不被点（.）跟随的数字。<br/>`/\d+(?!\.)/.exec('3.141')`匹配"141"，而不是"3.141"|
|(?<=y)x|x只有在y后面才匹配。<br/>`/(?<=\$)\d+/.exec('Benjamin Franklin is on the $100 bill') `<br/>`// ["100"]`|
|(?<!y)x|x只有不在y后面才匹配。<br/>`/(?<!\$)\d+/.exec('it’s is worth about €90')`<br/> `// ["90"]`|

<!--|正向前瞻|exp(?=assert)| |-->
<!--|负向前瞻|exp(?!assert)| |-->
<!--|正向后顾|exp(?<=assert)| JS不支持|-->
<!--|负向后顾|exp(?<!assert)| JS不支持|-->

<pre>
正则表达式从文本头部向尾部开始解析，文本尾部方向，称为前瞻
前瞻就是在正则表达式匹配到规则的时候，向前检查是否符合断言，
后顾/后瞻方向相反
JS 不支持后顾
符合不符合特定断言成为 `肯定/正向`匹配 和`否定/负向`匹配

'a1b2c3'.replace(/\w(?=\d)/g,'X')
// "X1X2X3"
'a1b2c3'.replace(/\w(?!\d)/g,'X')
// "aXbXcX"
</pre>

## properties 属性
<pre>
RegExp.prototype
允许为所有正则对象添加属性。
RegExp.length
 值为 2。
</pre>
## functions 方法
<pre>
全局对象 RegExp 自身没有方法, 不过它会继承一些方法通过原型链
Methods inherited from Function:
apply, call, toSource, toString
</pre>

## RegExp instance 实例

### instance 实例属性
<pre>
注意，RegExp 对象的几个属性既有完整的长属性名，也有对应的类 Perl 的短属性名。两个属性都有着同样的值。JavaScript 的正则语法就是基于 Perl 的。

RegExp.prototype.constructor
创建该正则对象的构造函数。
RegExp.prototype.global
是否开启全局匹配，也就是匹配目标字符串中所有可能的匹配项，而不是只进行第一次匹配。
RegExp.prototype.ignoreCase
在匹配字符串时是否要忽略字符的大小写。
RegExp.prototype.lastIndex
下次匹配开始的字符串索引位置。
RegExp.prototype.multiline
是否开启多行模式匹配（影响 ^ 和 $ 的行为）。
RegExp.prototype.source
正则对象的源模式文本。
RegExp.prototype.sticky 
是否开启粘滞匹配。

Properties inherited from Object:
__parent__, __proto__
</pre>

* `global`：是否全文搜索，默认false
* `ignore case`：是否大小写敏感，默认是false
* `multiline`：多行搜索，默认值是false
* `lastIndex`：当前表达式匹配内容的最后一个字符的下一个位置（表示执行下一次匹配时的起始位置。）（只在全局作用下有用）
* `source`：正则表达式的文本字符串(不包括标志位)

### instance 实例方法
RegExp.prototype.exec(str)<br/>
在目标字符串中执行一次正则匹配操作。<br/>
<pre>
使用正则表达式模式对字符串执行搜索，并将更新全局RegExp对象的属性以反映匹配结果
如果没有匹配的文本就返回`null`，否则就返回一个结果数组
  - `index`声明匹配文本的第一个字符的位置
  - `input`存放被检索的字符串`string`
</pre>
<!--非全局调用：-->
<!--调用非全局的RegExp对象的exec（）时，返回数组-->
<!--第一个元素是与正则表达式相匹配的文本-->
<!--第二个元素是与RegExpObject的第一个子表达式（分组）相匹配的文本(如果有的话)-->
<!--第三个元素是与RegExp对象的第二个子表达式（分组）相匹配的文本(如果有的话)-->
<!--以此类推-->


RegExp.prototype.test(str)
测试当前正则是否能匹配目标字符串。

如果存在则返回`true`，否则返回`false`
  ```
  const reg1 = /\w/;
  const reg2 = /\w/g;
  reg1.test('a') // true
  reg2.test('b') // true | false
  while(reg2.test('b')){
    console.log(reg2.lastIndex)
  }
  ```
  
  
RegExp.prototype.toSource() 
返回一个字符串，其值为该正则对象的字面量形式。覆盖了Object.prototype.toSource 方法.
RegExp.prototype.toString()
返回一个字符串，其值为该正则对象的字面量形式。覆盖了Object.prototype.toString() 方法。

Methods inherited from Object:
__defineGetter__, __defineSetter__, hasOwnProperty, isPrototypeOf, __lookupGetter__, __lookupSetter__, __noSuchMethod__, propertyIsEnumerable, toLocaleString, unwatch, valueOf, watch
</pre>

### 字符串对象方法

#### stringl.protatype.search(reg)
<pre>
search()方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串
方法返回第一个匹配结果index，查找不到返回-1
search（）方法不执行全局匹配，它将忽略标志g，并且总是从字符串开始进行检索
</pre>

#### string.prototype.match(reg)
<pre>
match（）方法将检索字符串，以找到一个或多个与regexp匹配的文本
regexp是否具有标志g对结果影响很大
非全局调用：如果regexp没有标志g，那么match（）方法就只能在字符串中执行一次匹配，没找到任何匹配文本将返回null，否则将返回一个数组，
*其中存放了与它找到的匹配文本有关的信息：返回数组的第一个元素存放的是匹配文本，而其余元素存放的是与正则表达式的子表达式匹配的文本。
除了常规数组元素之外，返回的数组还含有2个对象属性：
  - `index`声明匹配文本的起始字符在字符串的位置；
  - `input`声明对`stringobject`的引用
全局调用：如果regexp具有标志g则match（）方法将执行全局检索，找到字符串中的所有匹配子字符串：没有找到任何匹配的子穿，则返回null，否则返回一个数组，数组元素中存放的是字符串中所有匹配子串，而且也没有index属性或input属性
</pre>

#### string.prototype.split(reg)
```
'a,d,c,d'.split(',');
// ["a", "d", "c", "d"]
'a2b2c3d4e5'.split(/\d/);
// ["a", "b", "c", "d", "e", ""]
```

#### string.prototype.replace(reg,function)
* 分组有无，影响参数顺序
<pre>
function的参数
  1. 匹配到的字符串
  2. 分组匹配到的内容（如果有的话）
  3. 匹配到的字符串的index
  4. 原始字符串
</pre>

  ```
  'a2b2c3d4e5'.replace(/\d/g,function(match,index,origin){
    console.log(index);
    return parseInt(match) + 1;
  })

  'a2b2c3d4e5'.replace(/(\d)(\w)(\d)/g,function(match,group1,group2,group3,index,origin){
    console.log(index);
    return group1 + group3;
  })
  ```
