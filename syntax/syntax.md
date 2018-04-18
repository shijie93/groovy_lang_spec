这一章包含 `Groovy` 程序设计语言的语法。该语言的语法来源于Java语法，但通过Groovy的特定结构对其进行了增强，并允许进行某些简化。

# 1.Comments
## 1.1.单行注释
同 C,java 等，使用 `//` 进行注释，从之后知道行尾。
```java
// a standalone single line comment
println "hello" // a comment till the end of the line
```
## 1.2.多行注释
同 C,java 等，使用 `/**/` 进行注释
```java
/* a standalone multiline comment
   spanning two lines */
println "hello" /* a multiline comment starting
                   at the end of a statement */
println 1 /* one */ + 2 /* two */
```
## 1.3.GroovyDoc注释
相当于 `javadoc`，使用 `/** */` 进行注释，主要用于：

 - 类型定义
 - 字段和属性定义
 - 方法定义

> GroovyDoc遵循与Java自己的JavaDoc相同的约定。所以你可以使用与JavaDoc相同的标签。

```java
/**
 * A Class description
 */
class Person {
    /** the name of the person */
    String name

    /**
     * Creates a greeting method for a certain person.
     *
     * @param otherPerson the person to greet
     * @return a greeting message
     */
    String greet(String otherPerson) {
       "Hello ${otherPerson}"
    }
}
```
## 1.4.Shebang
与大多数例如 shell , perl 一样，在 groovy 代码(脚本)的第一行可以添加 `shebang`:
```bash
#!/usr/bin/env groovy
println "Hello from the shebang line"
```
> 该字符必须是该文件的第一个字符。任何缩进都会产生编译错误。

# 2.关键字
下面列举出 groovy 的所有关键字：

 - as  assert   break   case  catch  
 - class  const  continue  def  default  
 - do  else  enum  extends  false  finally  
 - for  goto  if  implements  import  in  
 - instanceof  interface  new  null  package  
 - return  super  switch  this  throw  throws  
 - trait  true  try  while  

# 3.标识符
## 3.1.一般标识符
标识符以字母， `$` 或下划线开头。他们不能以数字开头。

下面给出了一些例子：
```groovy
// right
def name
def item3
def with_underscore
def $dollarStart

// error
def 3tier
def a+b
def a#b
```

## 3.2.被引用的标识符
引用的标识符出现在点表达式的后面。例如：person.name 表达式的名称部分可以用person."name" 或 person.'name'。 当特定的标识符包含Java语言规范禁止的非法字符时，Groovy在引用时允许这些字符。例如，像短划线，空格，感叹号等字符：
```groovy
def map = [:]

map."an identifier with a space and double quotes" = "ALLOWED"
map.'with-dash-signs-and-single-quotes' = "ALLOWED"

assert map."an identifier with a space and double quotes" == "ALLOWED"
assert map.'with-dash-signs-and-single-quotes' == "ALLOWED"
```
Groovy提供了不同的字符串文字:
```groovy
map.'single quote'
map."double quote"
map.'''triple single quote'''
map."""triple double quote"""
map./slashy string/
map.$/dollar slashy string/$
```
可以使用内插字符串：
```groovy
def firstname = "Homer"
map."Simpson-${firstname}" = "Homer Simpson"

assert map.'Simpson-Homer' == "Homer Simpson"
```
# 4.字符串
以连续的字符表示的文本称为字符串。

## 4.1.单引号字符串
单引号字符串是普通的java.lang.String，**不支持内插变量**。
```groovy
'a single quoted string'
```

## 4.2.字符串连接
使用 `+`
```groovy
assert 'ab' == 'a' + 'b'
```

## 4.3.三重单引号字符串
```groovy
'''a triple single quoted string'''
```
同样也是普通的java.lang.String，**不支持内插变量**，但是它支持多行。
```groovy
def aMultilineString = '''line one
line two
line three'''
```
以下方式的第一个字符为换行符:
```groovy
def startingAndEndingWithANewline = '''
line one
line two
line three
'''
```
可以通过换行来消除换行符:
```groovy
def strippedFirstNewline = '''\
line one
line two
line three
'''
```
### 4.3.1.转义特殊的字符
您可以使用反斜线字符转义单引号以避免终止字符串文字：
```groovy
'an escaped single quote: \' needs a backslash'
```
也可以转义反斜线字符本身:
```groovy
'an escaped escape character: \\ needs a double backslash'
```

 - '\t'                制表符
 - '\b'                退格符
 - '\n'                换行符
 - '\r'                回车符
 - '\f'                换页符
 - '\\'                反斜线
 - '\''                单引号
 - '\"'                双引号 

### 4.3.2.Unicode 转义序列
对于键盘上不存在的字符，可以使用 unicode 转义序列：反斜线后跟'u'，然后是4个十六进制数字。
例如，欧元货币符号可以用：
```groovy
'The Euro currency symbol: \u20AC'
```
## 4.4.双引号字符串
```groovy
"a double quoted string"
```
如果没有插值表达式，双引号字符串是普通的java.lang.String，但是如果存在插值，则是groovy.lang.GString实例。
```groovy
def startingAndEndingWithANewline = /The Euro currency "symbol": \u20AC/

println(startingAndEndingWithANewline.class)

def name = "Tom"
def greeting = "Hello ${name}"

println(greeting.class)
// class java.lang.String
// class org.codehaus.groovy.runtime.GStringImpl
```
### 4.4.1.字符串插值
除了单引号和三重引号字符串之外，任何Groovy表达式都可以插入所有字符串文字中。插值是在评估字符串时用字符串的值替换字符串中的占位符。占位符表达式由 `${}` 包围。
```groovy
def name = 'Guillaume' // a plain string
def greeting = "Hello ${name}"

assert greeting.toString() == 'Hello Guillaume'
```
任何表达式都是可以的:
```groovy
def sum = "The sum of 2 and 3 equals ${2 + 3}"

assert sum.toString() == 'The sum of 2 and 3 equals 5'
```
`${}` 不仅仅可以插入表达式，还可以插入多个语句，最后的值是最后一个语句的返回值，例如：
```groovy
"The sum of 1 and 2 is equal to ${def a = 1; def b = 2; a + b}"
```
> 推荐尽量使用简短的表达式

除了 `${}` 占位符外，我们还可以使用单独的 `$` 符号作为有点的表达式的前缀：
```groovy
def person = [name: 'Guillaume', age: 36]
assert "$person.name is $person.age years old" == 'Guillaume is 36 years old'
```
点表达式方法不能使用 `$`:
```groovy
def number = 3.14

shouldFail(MissingPropertyException) {
    println "$number.toString()"
}
```
会抛出异常 `groovy.lang.MissingPropertyException` ,可以把它看成 `${number.toString}()`

### 4.4.2.插值闭包表达式的特殊例子
到目前为止，我们已经看到，我们可以在 `${}` 占位符内插入任意表达式，但是对于闭包表达式有一个特殊情况和表示法。当占位符包含一个箭头 `${→}` 时，表达式实际上是一个闭包表达式 --- 您可以将其视为一个封闭体，并在其前面添加一个 `$`:
```groovy
def sParameterLessClosure = "1 + 2 == ${-> 3}" 
assert sParameterLessClosure == '1 + 2 == 3'

def sOneParamClosure = "1 + 2 == ${ w -> w << 3}" 
assert sOneParamClosure == '1 + 2 == 3'
```
从表面上看，它看起来像是一种更加详细的定义表达式的方式，但与简单表达式相比，闭包有一个有趣的优势：`lazy evaluation`
```groovy
def number = 1 
def eagerGString = "value == ${number}"
def lazyGString = "value == ${ -> number }"

assert eagerGString == "value == 1" 
assert lazyGString ==  "value == 1" 

number = 2 
assert eagerGString == "value == 1" 
assert lazyGString ==  "value == 2" 
```
对 `number` 的每一次修改，都会更新 `lazyGString` 的值。
> 嵌入式闭包表达式在使用多个参数会在运行时产生异常。只允许使用零个或一个参数。

### 4.4.3.与 java 的互通性
当一个方法（不管是在Java还是Groovy实现）需要一个java.lang.String，但是我们传递一个groovy.lang.GString实例时，GString的toString（）方法会自动且透明地被执行：
```groovy
String takeString(String message) {         
    assert message instanceof String        
    return message
}

def message = "The message is ${'hello'}"   
assert message instanceof GString           

def result = takeString(message)            
assert result instanceof String
assert result == 'The message is hello
```


## 4.5.三重双引号字符串
它功能类似双引号，可以支持插值，也类似三重单引号，可以支持换行:
```groovy
def name = 'Groovy'
def template = """
    Dear Mr ${name},

    You're the winner of the lottery!

    Yours sincerly,

    Dave
"""

assert template.toString().contains('Groovy')
```
## 4.6.斜线字符串
使用 `/` 作为分隔符，多用在 **正则表达式和模式** 中，因为它不会转义反斜线：
```groovy
def fooPattern = /.*foo.*/
assert fooPattern == '.*foo.*'
```
只有正斜线需要用反斜线转义
```groovy
def escapeSlash = /The character \/ is a forward slash/
assert escapeSlash == 'The character / is a forward slash'
```
它也支持 **多行** 和 **内插**。(只有单引号和三重单引号不支持内插)
```groovy
def color = 'blue'
def interpolatedSlashy = /a ${color} car/

assert interpolatedSlashy == 'a blue car'
```

## 4.7.字符串一览表
![string_summary](https://raw.githubusercontent.com/shijie93/groovy_lang_spec/master/syntax/string_summary.png)

## 4.8.字符
与Java不同，Groovy没有明确的字符文字。但是，您可以通过三种不同的方式来明确如何使Groovy字符串成为实际字符：
```groovy
char c1 = 'A' 
assert c1 instanceof Character

def c2 = 'B' as char 
assert c2 instanceof Character

def c3 = (char)'C' 
assert c3 instanceof Character
```
# 5.数字
Groovy支持不同类型的整数文字和小数文字，并支持Java常用的Number类型。

## 5.1.整数文字
整数文字类型和java相同：

 - byte
 - char
 - short
 - int
 - long
 - java.lang.BigInteger

```groovy
// primitive types
byte  b = 1
char  c = 2
short s = 3
int   i = 4
long  l = 5

// 无限精度
BigInteger bi =  6
```
如果您使用 `def` 关键字进行可选输入，则整数的类型将有所不同：**它将适应可容纳该数字的类型的容量**。
```groovy
def a = 1
assert a instanceof Integer

// Integer.MAX_VALUE
def b = 2147483647
assert b instanceof Integer

// Integer.MAX_VALUE + 1
def c = 2147483648
assert c instanceof Long

// Long.MAX_VALUE
def d = 9223372036854775807
assert d instanceof Long

// Long.MAX_VALUE + 1
def e = 9223372036854775808
assert e instanceof BigInteger
```

### 5.1.1.非10进制表示
数字也可以用二进制，八进制，十六进制和十进制表示。
```groovy
int xInt = 0x77
assert xInt == 119
```
## 5.2.小数文字
小数文字与java相同：

 - float
 - double
 - java.lang.BigDecimal `Groovy 默认的小数类型`，其他需要显示

```groovy
// primitive types
float  f = 1.234
double d = 2.345

// infinite precision
BigDecimal bd =  3.456

def a = 1.1

println(a.class)
// class java.math.BigDecimal
```
可以使用指数 `e` 或者 `E`，后跟随者指数:
```groovy
assert 1e3  ==  1_000.0
assert 2E4  == 20_000.0
assert 3e+1 ==     30.0
assert 4E-2 ==      0.04
assert 5e-1 ==      0.5
```
## 5.3.文字下划线
在编写长文字数字时，靠眼睛难以弄清楚某些数字是如何组合在一起的，例如数千个组合等等。通过允许您在数字文字中放置下划线，可以更容易地发现这些组：
```groovy
long creditCardNumber = 1234_5678_9012_3456L
long socialSecurityNumbers = 999_99_9999L
double monetaryAmount = 12_345_132.12
long hexBytes = 0xFF_EC_DE_5E
long hexWords = 0xFFEC_DE5E
long maxLong = 0x7fff_ffff_ffff_ffffL
long alsoMaxLong = 9_223_372_036_854_775_807L
long bytes = 0b11010010_01101001_10010100_10010010
```

## 5.4.数字类型后缀
我们可以通过给一个后缀强制一个数字（包括二进制，八进制和十六进制）具有特定类型，可以是大写或小写。

![number_type_suffixes.png](https://raw.githubusercontent.com/shijie93/groovy_lang_spec/master/syntax/number_type_suffixes.png)

```groovy
assert 42I == new Integer('42')
assert 42i == new Integer('42') // lowercase i more readable
assert 123L == new Long("123") // uppercase L more readable
assert 2147483648 == new Long('2147483648') // Long type used, value too large for an Integer
assert 456G == new BigInteger('456')
assert 456g == new BigInteger('456')
assert 123.45 == new BigDecimal('123.45') // default BigDecimal type used
assert 1.200065D == new Double('1.200065')
assert 1.234F == new Float('1.234')
assert 1.23E23D == new Double('1.23E23')
assert 0b1111L.class == Long // binary
assert 0xFFi.class == Integer // hexadecimal
assert 034G.class == BigInteger // octal
```
# 6. Booleans
布尔值是一种特殊的数据类型，用于表示真值： `true` 和 `false` 。此数据类型常用于跟踪真/假条件的简单标志。
```groovy
def myBooleanVariable = true
boolean untypedBooleanVar = false
booleanField = true
```

# 7.列表
Groovy 使用逗号分隔的值列表，用方括号括起来表示列表。Groovy列表是普通的JDK `java.util.List` ，因为Groovy没有定义它自己的集合类。定义列表文字时使用的具体列表实现默认为 `java.util.ArrayList` ，除非您决定另外指定。

```groovy
def numbers = [1, 2, 3]         

assert numbers instanceof List  
assert numbers.size() == 3      
```
上述的列表为相同类型元素的列表，也可以创建不同元素类型的列表:
```groovy
def heterogeneous = [1, "a", true]
```
默认情况下，列表文字实际上是 `java.util.ArrayList` 的实例，但是可以为我们的列表使用不同的支持类型，这要归功于使用 `as` 运算符的类型强制，或者对变量显式的类型声明：
```groovy
def arrayList = [1, 2, 3]
assert arrayList instanceof java.util.ArrayList

def linkedList = [2, 3, 4] as LinkedList    
assert linkedList instanceof java.util.LinkedList

LinkedList otherLinked = [3, 4, 5]          
assert otherLinked instanceof java.util.LinkedList
```
列表操作
```groovy
def letters = ['a', 'b', 'c', 'd']

assert letters[0] == 'a'     
assert letters[1] == 'b'

assert letters[-1] == 'd'    
assert letters[-2] == 'c'

letters[2] = 'C'             
assert letters[2] == 'C'

letters << 'e'               
assert letters[ 4] == 'e'
assert letters[-1] == 'e'

assert letters[1, 3] == ['b', 'd']         
assert letters[2..4] == ['C', 'd', 'e']    
```
多维列表：
```groovy
def multi = [[0, 1], [2, 3]]     
assert multi[1][0] == 2          
```
# 8.数组
Groovy重用了列表符号作为数组声明，但为了制作这样的文字数组，您需要通过强制或类型声明来明确定义数组的类型。
```groovy
String[] arrStr = ['Ananas', 'Banana', 'Kiwi']  

assert arrStr instanceof String[]    

def numArr = [1, 2, 3] as int[]      

assert numArr instanceof int[]       
assert numArr.size() == 3
```
还可以创建多维数组：
```groovy
def matrix3 = new Integer[3][3]         
assert matrix3.size() == 3

Integer[][] matrix2                     
matrix2 = [[1, 2], [3, 4]]
assert matrix2 instanceof Integer[][]
```
> 对数组元素的访问与列表相同

# 9.Maps
含义与其他语言的字典相同
```groovy
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']   

assert colors['red'] == '#FF0000'    
assert colors.green  == '#00FF00'    

colors['pink'] = '#FF00FF'           
colors.yellow  = '#FFFF00'           

assert colors.pink == '#FF00FF'
assert colors['yellow'] == '#FFFF00'

assert colors instanceof java.util.LinkedHashMap
```
> Groovy创建的映射实际上是 `java.util.LinkedHashMap` 的实例。

访问不存在的 key 会返回 `null`.

当想要使用变量代替key时，需要用括号包围：
```groovy
def key = 'name'
person = [(key): 'Guillaume']

assert person.containsKey('name')
assert !person.containsKey('key')    
```