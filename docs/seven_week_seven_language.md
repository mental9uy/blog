# 七周七语言

- [七周七语言](#七周七语言)
  - [第一章 简介](#第一章-简介)
    - [1.1 不走寻常路](#11-不走寻常路)
    - [1.2 语言](#12-语言)
  - [第二章 Ruby](#第二章-ruby)
    - [2.1 Ruby 简史](#21-ruby-简史)
    - [2.2 第一天：找个保姆](#22-第一天找个保姆)
      - [2.2.1 快速起步](#221-快速起步)
      - [2.2.2 从命令行执行Ruby](#222-从命令行执行ruby)
      - [2.2.5 鸭子类型](#225-鸭子类型)
      - [2.2.6 第一天我们学到了什么](#226-第一天我们学到了什么)
      - [2.2.7 第一天自习](#227-第一天自习)
    - [2.3 第二天：从天而降](#23-第二天从天而降)
      - [2.3.1 定义函数](#231-定义函数)
      - [2.3.1 数组](#231-数组)
      - [2.3.3 散列表](#233-散列表)
      - [2.3.4 代码块和 yield](#234-代码块和-yield)
      - [2.3.5 定义类](#235-定义类)
      - [2.3.6 编写Mixin](#236-编写mixin)
      - [2.3.7 模块、可枚举和集合](#237-模块可枚举和集合)
      - [2.3.8 第二天我们学到了什么](#238-第二天我们学到了什么)
      - [2.3.9 第二天自习](#239-第二天自习)
    - [2.4 第三天：重大改变](#24-第三天重大改变)
      - [2.4.1 开放类](#241-开放类)
      - [2.4.2 使用method_missing](#242-使用method_missing)
      - [2.4.3 模块](#243-模块)
      - [2.4.4 第三天我们学到了什么](#244-第三天我们学到了什么)
      - [2.4.5 第三天自习](#245-第三天自习)
    - [2.5 趁热打铁](#25-趁热打铁)
      - [2.5.1 核心优势](#251-核心优势)
      - [2.5.2 不足之处](#252-不足之处)
      - [2.5.3 最后的思考](#253-最后的思考)
  - [第三章 Io](#第三章-io)
    - [3.1 Io 简介](#31-io-简介)

---

## 第一章 简介

> 人总是处于各种目的学习自然语言。为了生存、为了生活交往、为了征服。
> 编程语言亦是如此。导游式的带你体验一次启迪心智之旅，改变看待编程的视角。

### 1.1 不走寻常路

> 互动教程。难以找到称心如意的教程。

* 语言的类型模型：语言在类型模型间的权衡会对开发者产生影响，会改变对问题的处理方式，还会控制语言的运行方式。
  * 强类型（Java）
  * 弱类型（C语言）
  * 静态类型（Java）
  * 动态类型（Ruby）
* 语言的变成范型：面向对象、函数式、过程式
  * 基于逻辑：Prolog
  * 完全支持面向对象思想：Ruby，Scala
  * 带有函数式特性：Scala，Erlang，Clojure，Hashell
  * 原型语言：Io
* 怎样和语言交互？语言可编译也可解释，可以有虚拟机也可以没有。
* 语言的判断结构和核心数据结构是什么？很多不同于if和while的结构。如Erlang的模式匹配，Prolog的合一（unification）。核心数据结构：集合。
* 语言的核心特性：有些语言支持并发编程，有些语言提供独一无二的高级结构，如Clojure的宏（marco）和Io的消息解释。有些语言包含性能强劲的虚拟机，如Erlang的BEAM，能让Erlang构建的容错分布式系统远远快于其他语言。有些语言提供专门针对特定问题的变成模型。

### 1.2 语言

1. Ruby：面向对象，好用好读。重点介绍Ruby元编程（metaprogramming），可以用来扩展Ruby的语法。
2. Io：兼具简单性和语法一致性的并发结构。Io和JavaScript同为原型语言，还有消息分发机制。
3. Prolog：威力无穷。能轻松解出数独问题，Erlang深受Prolog影响。
4. Scala：运行于Java虚拟机上的新一代语言，Scala为Java系统引入了强大的函数式思想，同时未丢弃面向对象编程。
5. Erlang：不仅是一门函数式语言，而且在并发、分布式编程、容错等诸多方面都有优异表现。在Erlang帮助下，设计带有并发、分布式、容错等特征的应用程序将变得无比简单。
6. Clojure：运行于Java虚拟机的语言，颠覆了我们在Java虚拟机上并发编程的思考方式。
7. Haskell：纯函数式语言。

## 第二章 Ruby

> 把精力放在提高程序员的编程效率上

### 2.1 Ruby 简史

> 松本行弘大约在 1993 年发明了 Ruby，Ruby 出身于所谓的脚本语言家族，是一种解释型、面向对象、动态类型的语言。
> 解释型：意味着Ruby代码由解释器而非编译器执行。
> 动态类型：意味着类型在运行时而非编译时绑定。
> 面向对象，类继承，多态等特性
> 集合用到的代码块

### 2.2 第一天：找个保姆

> 完成能用其他语言搞定的事

#### 2.2.1 快速起步

> 安装地址：http://www.ruby-lang.org/en/downloads/
> Mac 自带 Ruby 环境
> 输入 irb测试是否安装成功

#### 2.2.2 从命令行执行Ruby

```shell
irb(main):001:0> puts 'hello,world'
hello,world
=> nil
irb(main):002:0> properties = ['object oriented', 'duck typed', 'productive', 'fun']
=> ["object oriented", "duck typed", "productive", "fun"]
irb(main):003:0> properties.each {| property | puts "Ruby is #{property}."}
Ruby is object oriented.
Ruby is duck typed.
Ruby is productive.
Ruby is fun.
=> ["object oriented", "duck typed", "productive", "fun"]

irb(main):013:0> language = 'Ruby';
=> "Ruby"
irb(main):014:0> puts "Hi,#{language}";
Hi,Ruby
=> nil
irb(main):015:0> puts 'Hi,#{language}';
Hi,#{language}
=> nil
irb(main):016:0> 
```

* Ruby 是解释执行
* 无变量声明
* Ruby 每条代码都会有返回值
* 字符串，用单引号表示不加改动地直接解释，双引号则会引发字符串替换
  2.2.3 Ruby 的编程模型
  
  > Ruby 是一门纯面向对象的语言

```shell
irb(main):025:0> 4
=> 4
irb(main):026:0> 4.class
=> Integer
irb(main):027:0> 4 + 4
=> 8
irb(main):028:0> 4.methods
=> [:-@, :**, :<=>, :upto, :<<, :<=, :>=, :==, :chr, :===, :>>, :[], :%, :&, :inspect, :*, :+, :ord, :-, :/, :size, :...]
2.2.4 判断
if,while,until,unless,not,!
`除了 nil 和 false 之外，其他值都代表 true`
irb(main):034:0> puts 'This appears to be false.' unless x == 4
=> nil
irb(main):035:0> puts 'This appears to be false.' if x == 4
This appears to be false.
=> nil
irb(main):036:0> puts 'This appears to be true.' if x == 4
This appears to be true.
=> nil
irb(main):037:0> if x == 4
irb(main):038:1>     puts 'This appears to be true'
irb(main):039:1> end
This appears to be true
=> nil
irb(main):040:0> unless x == 4
irb(main):041:1>     puts 'This appears to be false.'
irb(main):042:1> end
=> nil
irb(main):043:0> x = x + 1 while x < 10
=> nil
irb(main):044:0> x
=> 10
```

#### 2.2.5 鸭子类型

> Ruby 是强类型语言，发生类型冲突时，将返回错误。Ruby 在运行时而非编译时进行类型检查。

```shell
irb(main):076:0> 4 + 'four'
Traceback (most recent call last):
        5: from /usr/bin/irb:23:in `<main>'
        4: from /usr/bin/irb:23:in `load'
        3: from /Library/Ruby/Gems/2.6.0/gems/irb-1.0.0/exe/irb:11:in `<top (required)>'
        2: from (irb):76
        1: from (irb):76:in `+'
TypeError (String can't be coerced into Integer)
irb(main):077:0> 4.class
=> Integer
irb(main):078:0> (4.0).class
=> Float
irb(main):079:0> 4 + 4.0
=> 8.0
irb(main):100:0> def add_them_up
irb(main):101:1> 4 + 'four'
irb(main):102:1> end
=> :add_them_up
irb(main):103:0> add_them_up
Traceback (most recent call last):
        6: from /usr/bin/irb:23:in `<main>'
        5: from /usr/bin/irb:23:in `load'
        4: from /Library/Ruby/Gems/2.6.0/gems/irb-1.0.0/exe/irb:11:in `<top (required)>'
        3: from (irb):103
        2: from (irb):101:in `add_them_up'
        1: from (irb):101:in `+'
TypeError (String can't be coerced into Integer)
```

动态类型：直到尝试执行代码时，Ruby才进行类型检查。
在采用静态类型系统的情况下，编译器和工具能捕获更多错误，因此Ruby不会像静态类型语言捕获的错误那么多。这是它的劣势。
类型系统优势：多个类不必继承自相同的父类就能以相同的方式使用

```shell
irb(main):115:0> a = ['100', 100.0]
=> ["100", 100.0]
irb(main):116:0> while i < 2
irb(main):117:1> puts a[i].to_i
irb(main):118:1> i = i + 1
irb(main):119:1> end
100
100
=> nil
```

String 类型和 Float 类型都能使用 to_i 转为 Integer 类型

#### 2.2.6 第一天我们学到了什么

> 把基础只是过了一遍。Ruby是一门解释性语言。一切皆为对象

#### 2.2.7 第一天自习

API:http://www.ruby-lang.org/zh_cn/documentation/

- [x] 替换字符串某一部分
- [x] range 资料
- [x] 打印名字十遍
- [x] 猜数字大小程序

### 2.3 第二天：从天而降

> 体会Ruby的小魔法。对象、集合、类、代码块。

#### 2.3.1 定义函数

> 不必像Java、C#一样为了定义函数而把整个类都构建出来。

```shell
irb(main):178:0> def tell_the_truth
irb(main):179:1>   true
irb(main):180:1> end
=> :tell_the_truth
irb(main):181:0> tell_the_truth()
=> true
irb(main):182:0> 
```

#### 2.3.1 数组

```shell
irb(main):196:0> animals = ['lions', 'tigers', 'bears']
=> ["lions", "tigers", "bears"]
irb(main):197:0> puts animals
lions
tigers
bears
=> nil
irb(main):198:0> animals[0]
=> "lions"
irb(main):199:0> animals[10]
=> nil
irb(main):200:0> animals[-1]
=> "bears"
irb(main):201:0> animals[0..1]
=> ["lions", "tigers"]
irb(main):202:0> (0..1).class
=> Range

irb(main):207:0> a = [1]
=> [1]
irb(main):208:0> a.push(2)
=> [1, 2]
irb(main):209:0> a.push(10)
=> [1, 2, 10]
irb(main):210:0> a
=> [1, 2, 10]
irb(main):211:0> a.pop
=> 10
irb(main):212:0> 
```

数组拥有及其丰富的API，可将其用作队列、链表、栈、集合等等。

#### 2.3.3 散列表

```shell
irb(main):221:0> numbers = {1 => 'one', 2 => 'two'}
=> {1=>"one", 2=>"two"}
irb(main):222:0> numbers[1]
=> "one"
irb(main):223:0> numbers[2]
=> "two"
irb(main):224:0> stuff = {:array => [1,2,3], :string => 'Hi, mon!'}
=> {:array=>[1, 2, 3], :string=>"Hi, mon!"}
irb(main):225:0> stuff[:string]
=> "Hi, mon!"
irb(main):226:0> 
```

符号（symbol）是前面带有冒号的标识符，蕾丝于 :symbol 的形式。它在给事物和概念命名时非常好用。

#### 2.3.4 代码块和 yield

> 代码块是没有名字的函数。它可以作为参数传递给函数或者方法，如：
> 代码块可以采用 {/} 或 do/end 两种形式

```shell
irb(main):291:0> 3.times {puts 'hi ya there, kiddo'}
hi ya there, kiddo
hi ya there, kiddo
hi ya there, kiddo
=> 3

irb(main):289:0> animals = ['lions and ', 'tigers and ', 'bears', 'oh my']
=> ["lions and ", "tigers and ", "bears", "oh my"]
irb(main):290:0> animals.each {|a| puts a}
lions and 
tigers and 
bears
oh my
=> ["lions and ", "tigers and ", "bears", "oh my"]
```

在 Ruby 中，参数名前加一个 "&" 表示将代码块作为闭包传递给函数

```shell
irb(main):306:0> def call_block(&block)
irb(main):307:1>   block.call
irb(main):308:1> end
=> :call_block
irb(main):309:0> def pass_block(&block)
irb(main):310:1>   call_block(&block)
irb(main):311:1> end
=> :pass_block
irb(main):312:0> pass_block {puts 'hello, block'}
hello, block
=> nil
```

这个技术能让你把可执行代码派发给其他方法。
在Ruby中，代码块不仅可用于循环，还可用于延迟执行（等到调用该代码块相关的yield时才会执行）：
execute_at_noon {puts 'Beep beep... time to get up'}
从文件中运行Ruby
集成开发环境虽然功能完善，但大多数人还是乐于使用简便易用的文件编辑器

#### 2.3.5 定义类

> 类、对象。Ruby中的类只能继承自一个叫做超类的类

Integer - > Numeric -> Object -> BasicObject

```shell
irb(main):325:0> 4.class
=> Integer
irb(main):326:0> 4.class.superclass
=> Numeric
irb(main):327:0> 4.class.superclass.superclass
=> Object
irb(main):329:0> 4.class.superclass.superclass.superclass
=> BasicObject
irb(main):330:0> 4.class.superclass.superclass.superclass.superclass
=> nil
Class -> Module -> Object
irb(main):337:0> 4.class
=> Integer
irb(main):338:0> 4.class.class
=> Class
irb(main):339:0> 4.class.class.superclass
=> Module
irb(main):340:0> 4.class.class.superclass.superclass
=> Object
irb(main):341:0> 
```

命名规则：

* 类：大写字母开头，并且采用驼峰命名法，如 CamelCase
* 实例变量：前面加上@
* 类变量：前面加上@@
* 实例变量和方法名：小写字母开头，采用下划线命名发，如underscore_style
* 常量：全大写，下划线命名，如ALL_CAPS
* 用于逻辑测试的函数和方法：一般要加上问号，如 if test?
  其他：
* attr 关键字可用来定义实例变量和访问变量的同名方法。
* attr-accessor 关键字用来定义实例变量、访问方法和设置方法。

```ruby
class Tree

    attr_accessor :children, :node_name

    def initialize(name, children=[])
      @children = children
      @node_name = name
    end

    def visit_all(&block)
      visit &block
      children.each {|c| c.visit_all &block}
    end

    def visit(&block)
      block.call self
    end

end
```

```shell
irb(main):401:0> ruby_tree = Tree.new("ruby", [Tree.new("Reia"),Tree.new("MacRuby")])
=> #<Tree:0x00007f8bd1144d08 @children=[#<Tree:0x00007f8bd1144ec0 @children=[], @node_name="Reia">, #<Tree:0x00007f8bd1144d80 @children=[], @node_name="MacRuby">], @node_name="ruby">
irb(main):402:0> 
irb(main):403:0> ruby_tree.visit {|node| puts node.node_name}
ruby
=> nil
irb(main):404:0> ruby_tree.visit_all {|node| puts node.node_name}
ruby
Reia
MacRuby
=> [#<Tree:0x00007f8bd1144ec0 @children=[], @node_name="Reia">, #<Tree:0x00007f8bd1144d80 @children=[], @node_name="MacRuby">]
```

#### 2.3.6 编写Mixin

> 面向对象语言利用继承，将行为传播到相似的对象上。
> 但多继承复杂切问题多。
> Java 采用接口解决这一问题
> Ruby 采用模块，模块是函数和常量的集合，如果在类中包含了一个模块，那么该模块的行为和常量也会成为类的一部分。

```ruby
module ToFile
  def filename
    "object_#{self.object_id}.txt"
  end

  def to_f
    File.open(filename, 'w') {|f| f.write(to_s)}
  end
end

class Person
  include ToFile
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def to_s
    name
  end
end
```

Person.new('matz').to_f

#### 2.3.7 模块、可枚举和集合

> Ruby 有两个至关重要的mixin：枚举（enumerable）和比较（comparable）。如果想让类可枚举，必须实现each方法。如果想让类可比较，必须实现 <=> 操作符（太空船操作符）

```shell
irb(main):064:0> 'begin' <=> 'end'
=> -1
irb(main):065:0> 'same' <=> 'same'
=> 0
irb(main):066:0> a = [5,3,4,1]
=> [5, 3, 4, 1]
irb(main):067:0> a.sort
=> [1, 3, 4, 5]
irb(main):068:0> a.any? {|i| i>6}
=> false
irb(main):069:0> a.any? {|i| i>3}
=> true
irb(main):070:0> a.all? {|i| i>3}
=> false

irb(main):071:0> a.collect {|i| i*2}
=> [10, 6, 8, 2]
irb(main):072:0> a.collect {|i| i % 2 == 0}
=> [false, false, true, false]
irb(main):073:0> a.select {|i| i % 2 == 0}
=> [4]
irb(main):074:0> a.select {|i| i % 2 == 1}
=> [5, 3, 1]
irb(main):075:0> a.max
=> 5
irb(main):076:0> a.member?(2)
=> false
irb(main):077:0> a.member?(4)
=> true
```

集合操作：

* collect和map方法把函数应用到每个元素上，并返回结果数组
* find 方法找到一个符合条件的元素
* select和find_all 方法均返回所有符合条件的元素
* inject 方法集合列表的和与积

```shell
irb(main):093:0> a
=> [5, 3, 4, 1]
irb(main):091:0> a.inject {|sum,i| sum + i}
=> 13
irb(main):092:0> a.inject {|mutil,i| mutil * i}
=> 60
```

增加辅助代码，看看 inject 方法执行过程
// inject 方法，代码块中有两个参数和一个表达式，通过第二个参数传入每个列表元素
// inject 方法的唯一参数是返回结果初始值

```shell
irb(main):114:0> a.inject(0) do | sum,i|
irb(main):115:1*   puts "sum: #{sum}, i:#{i}, sum+i=#{sum+i}"
irb(main):116:1>   sum + i
irb(main):117:1> end
sum: 0, i:5, sum+i=5
sum: 5, i:3, sum+i=8
sum: 8, i:4, sum+i=12
sum: 12, i:1, sum+i=13
=> 13

// 
irb(main):121:0> a.inject do | sum,i|
irb(main):122:1*   puts "sum: #{sum}, i:#{i}, sum+i=#{sum+i}"
irb(main):123:1>   sum + i
irb(main):124:1> end
sum: 5, i:3, sum+i=8
sum: 8, i:4, sum+i=12
sum: 12, i:1, sum+i=13
=> 13
```

#### 2.3.8 第二天我们学到了什么

> Ruby 灵活。好用的集合

#### 2.3.9 第二天自习

- [x] 使用代码块和不使用代码块读取文件
- [x] 简单 grep 程序

```ruby
# 简单 grep 程序， grep "name","to_file.rb"
def grep(content, filename)
  text = File.read(filename)
  index = 1
  text.each_line {|line|
    if /#{content}/ =~ line
      puts "#{index} -- #{line}"                
      index = index + 1
    end
    }
end
```

### 2.4 第三天：重大改变

> 妙趣横生。
> 元编程

```ruby
class Department < ActiveRecord::Base
  has_many :employees
  has_one :manager
end  
```

#### 2.4.1 开放类

> 可以随时改变任何类的定义，常用于给类添加行为

```ruby
class NilClass
  def blank?
    true
  end
end

class String
  def blank?
    self.size == 0
  end
end
```

```shell
# ["", "person", nil].each do |element| puts element unless element.blank
# end
```

#### 2.4.2 使用method_missing

> Ruby 找不到某个方法时，会调用一个特殊的调试方法显示诊断信息。
> 通过复写 method_missing 方法，调用不存在的方法时，获得名称和参数并处理。

```ruby
class Roman
  def self.method_missing name, *args
    roman = name.to_s
    roman.gsub!("IV", "IIII")
    roman.gsub!("IX", "VIIII")
    roman.gsub!("XL", "XXXX")
    roman.gsub!("XC", "LXXXX")

    (roman.count("I") + roman.count("V") * 5 + roman.count("X") * 10 + roman.count("L") * 50 + roman.count("C") * 100)
    end
end
```

#### 2.4.3 模块

> Ruby 最流行的元编程方式非模块莫属。
> 通过一些令人惊叹的方式扩展类定义，其中一种技术是设计自己的 DSL（领域特定语言），再用 DSL 定义自己的类
> 根据类名，打开相应的 CSV 文件：

```ruby
class ActsAsCsv
  def read
    file = File.new(self.class.to_s.downcase + '.txt')
    @headers = file.gets.chomp.split(', ')

    file.each do |row|
      @result << row.chomp.split(', ')
    end
  end

  def headers
    @headers
  end

  def csv_contents
    @result
  end

  def initialize
    @result = []
    read
  end
end
```

#### 2.4.4 第三天我们学到了什么

> Ruby 自定义语法，动态地改变类。
> 利用 method_missing 方法写出了漂亮的罗马数字代码。

#### 2.4.5 第三天自习

### 2.5 趁热打铁

> Ruby 犹如徐徐吹来的一阵清风，为人们带来了些许清爽的感觉。

#### 2.5.1 核心优势

> Ruby 的纯面向对象可以让你用一致的方式来处理对象。Ruby 的模块和开放类，使程序员能把行为紧密结合到语法上，超越了类中定义的传统方法和实例变量。

1. 脚本：Ruby 是一门梦幻般的脚本语言。编写爬虫程序抓取股票报价或图书价格的网页，运行本地编译环境或自动测试等。
2. Web 开发：Rails现已成为有史以来最成功的Web开发框架之一，MVC 架构理念。
3. 市场投放时间：生产力。对于快速开发可推向市场的合格产品。

#### 2.5.2 不足之处

1. 性能：为了改善程序员的体验，而不是优化语言的性能。
2. 并发和面向对象编程：面向对象编程模型有个重大弱点，程序存在并发时，几乎无法在并发环境下调试程序，也无法可靠的测试程序。
3. 类型安全：

#### 2.5.3 最后的思考

> 核心优势：语法和灵活性。
> 不足：性能

## 第三章 Io

> 问题不是“我们要干点儿什么” 而是 “我们有什么不能干”

### 3.1 Io 简介

> 2002 年，Serve Dekorte 发明了 Io 语言。
> Io是一种原型语言，这意味着每个对象都是另一个对象的复制品