# *dustbin226* 的C++编程规范
###### C++  Style Guidence for *dustbin226*

- 作者：Yujian Lin (yjlin2018@mail.nwpu.edu.cn)
- 创建日期：2020年4月13日
- 最后修改：2020年4月27日

## 3. 代码格式


每个人都可能有自己的代码风格和格式，但是为了便于互相阅读，应该遵循一个规范的代码格式。

### 3.1. 行长度

每一行代码字符数**不超过80**.

虽然对于现代的显示器来说，120的字符数更方便，但是分屏阅读的时候120还是过长，因此限制在80字符。80字符限制事实上有助于避免代码可读性失控，比如超多重嵌套块，超多重函数调用等等。

如果无法在不伤害易读性的条件下进行断行，那么可以超过80个字符，这样可以方便复制粘贴。例如带有命令示例或URL的行可以超过80个字符。

包含长路径的<kbd><small><font color=#CC1E1E>#include</font></small></kbd>语句可以超出80列，头文件保护可以无视该原则。

### 3.2 非 ASCII 字符

非ASCII字符统一使用**UTF-8**编码，因为UTF-8的兼容性较好。

不要使用<kbd><small><font color=#CC1E1E>char16_t</font></small></kbd>和<kbd><small><font color=#CC1E1E>char32_t</font></small></kbd>。<kbd><small><font color=#CC1E1E>wchar_t</font></small></kbd>同理, 除非你写的代码要调用Windows API。C++ 20标准以后，使用<kbd><small><font color=#CC1E1E>char8_t</font></small></kbd>。

### 3.3 代码缩进

使用空格缩进，每级**4个**空格。**不要**在代码中使用制表符，请设置编辑器将制表符转为空格。

### 3.4 函数声明与定义

返回类型和函数名在同一行。例如：

```c++
ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) 
{
    DoSomething();
    ...
}
```

函数参数也尽量放在同一行，如果放不下就对形参分行。例如：

```c++
ReturnType ClassName::ReallyLongFunctionName(Type par_name1, Type par_name2,
                                             Type par_name3) 
{
    DoSomething();
    ...
}
```

按照Google规范的要求，函数定义时注意以下几点：

- 使用好的参数名
- 只有在参数未被使用或者其用途非常明显时, 才能省略参数名
- 如果返回类型和函数名在一行放不下, 分行
- 如果返回类型与函数声明或定义分行了, 不要缩进
- 左圆括号总是在函数名下一行 (Google要求在同一行，但是按照我们的习惯来)
- 函数名和左圆括号间、圆括号与参数间没有空格.
- 左大括号总在最后一个参数同一行的末尾处, 不另起新行.
- 右大括号总是单独位于函数最后一行, 或者与左大括号同一行.
- 所有形参应尽可能对齐.
- 尽量不要省略参数名，即使Google建议未使用/用途明显的参数名在声明中可省略
- 属性, 和展开为属性的宏, 写在函数声明或定义的最前面, 即返回类型之前，例如：
```c++ 
MUST_USE_RESULT bool IsOK();
```
### 3.5 Lambda 表达式

Lambda表达式对形参和函数体的格式化和其他函数一致。捕获列表同理，表项用逗号隔开。

若用引用捕获，在变量名和<kbd><small><font color=#CC1E1E>&</font></small></kbd>之间不留空格。例如：
```c++
int x = 0;
auto add_to_x = [&x](int n) { x += n; };
```

短Lambda就写得和内联函数一样。例如：
```c++
std::set<int> blacklist = {7, 8, 9};
std::vector<int> digits = {3, 9, 1, 8, 4, 7, 1};
digits.erase(std::remove_if(digits.begin(), digits.end(), [&blacklist](int i) 
            {
                return blacklist.find(i) != blacklist.end();
            }),
            digits.end());
```

关于Lambda表达式的捕获规则，详见<font color=red>TODO: 其他C++特性</font>

### 3.6 函数调用

> 要么一行写完函数调用，要么在圆括号里对参数分行，要么参数另起一行且缩进四格。如果没有其它顾虑的话，尽可能精简行数，比如把多个参数适当地放在同一行里。

函数调用遵循如下形式：
```c++
bool retval = DoSomething(argument1, argument2, argument3);
```

如果同一行放不下，可断为多行，后面每一行都和第一个实参对齐。左圆括号后和右圆括号前不要留空格：
```c++
bool retval = DoSomething(averyveryveryverylongargument1,
                          argument2, argument3);
```
参数也可以放在次行，缩进四格：
```c++
if (...) 
{
    ...
    ...
    if (...) 
    {
        DoSomething(
            argument1, argument2,  // 4 空格缩进
            argument3, argument4);
    }
}
```

如果一些参数本身就是略复杂的表达式，且降低了可读性，则应该直接创建临时变量描述该表达式，并传递给函数：
```c++
int my_heuristic = scores[x] * y + bases[x];
bool retval = DoSomething(my_heuristic, x, y, z);
```

如果某参数独立成行对可读性更有帮助的话，那也可以如此做。参数的格式处理应当以可读性而非其他作为最重要的原则。此外，如果一系列参数本身就有一定的结构，可以酌情地按其结构来决定参数格式：
```c++
// 通过 3x3 矩阵转换 widget.
my_widget.Transform(x1, x2, x3,
                    y1, y2, y3,
                    z1, z2, z3);
```

### 3.7 列表初始化格式

与函数调用保持一致即可。

### 3.8 条件语句
关键字<kbd><small><font color=#CC1E1E>if</font></small></kbd>和<kbd><small><font color=#CC1E1E>else</font></small></kbd>另起一行。

对基本条件语句，不要在空格和条件中添加空格。

如果能增强可读性，简短的条件语句允许写在同一行。只有当语句简单并且**没有使用**<kbd><small><font color=#CC1E1E>else</font></small></kbd>子句时使用。例如：

```c++
if (x == kFoo) return new Foo();
if (x == kBar) return new Bar();
```
为了统一，除了上面例子中的单行<kbd><small><font color=#CC1E1E>if</font></small></kbd>可以不使用大括号外，其余的条件语句必须使用大括号。例如：

```c++
if(condition)
{
    Foo();
}
else
{
    Bar();
}
```

### 3.9 循环和选择语句

<kbd><small><font color=#CC1E1E>switch</font></small></kbd>语句使用大括号分段，以表明cases之间不是连在一起的。<kbd><small><font color=#CC1E1E>switch</font></small></kbd>语句应该有<kbd><small><font color=#CC1E1E>default</font></small></kbd>子句用于处理不满足条件的枚举值。如果<kbd><small><font color=#CC1E1E>default</font></small></kbd>永远执行不到，添加一条<kbd><small><font color=#CC1E1E>assert</font></small></kbd>。例如：


```c++
switch(var)
{
    case 0:
    {
        DoSth1();
        break;
    }
    case 1:
    {
        DoSth2();
        break;
    }
    default:
    {
        assert(false);
    }
}
```

单语句循环里，括号可用可不用。空循环体应使用<kbd><small><font color=#CC1E1E>continue</font></small></kbd>表明而不是只用一个分号。例如：

```c++
// 循环只有一条语句，大括号可以忽略不写
for(int i = 0; i < 10; ++i) std::cout << i << std::endl;

// 空循环，应该用continue标明
while(condition) continue;

// 多个语句的循环
do
{
    DoSth1();
    DoSth2();
    DoSth3();
} while(condition);
```

### 3.10 指针和引用表达式

句点或箭头前后**不能**有空格，取地址/解引用操作符之后**不能**有空格。例如：

```c++
// 符合规范的示例
x = *p;
p = &x;
x = r.y;
x = r->y;

// 不符合规范的示例
x = * p;
p = & x;
x = r. y;
x = r ->y;
```

在声明指针变量或引用时，<kbd><small><font color=#CC1E1E>\*/&</font></small></kbd>与类型或变量名紧挨都可以。为了规范的统一，这里强行要求使用空格后置的规范，并且要求在多重声明中**不能**使用<kbd><small><font color=#CC1E1E>\*/&</font></small></kbd>。例如：

```c++
// 符合规范的示例
int* iptr = &a;
const std::vector<int>& vec = given_array;

// 不符合规范的示例
int *iptr = &b;   // 我们要求空格后置而不是前置
int a, *b;        // 多重声明中不能有*/& 
```
在单个文件内要保持风格一致，所以，如果是修改现有文件，要遵照该文件的风格而不是上面的规则。

### 3.11 布尔表达式

如果一个布尔表达式超过标准行宽，断行时将逻辑操作符<kbd><small><font color=#CC1E1E>&&</font></small></kbd>或<kbd><small><font color=#CC1E1E>||</font></small></kbd>放在行尾。可以额外插入圆括号增强可读性。例如：

```c++
if(this_one_thing > this_other_thing &&
    a_third_thing == a_fourth_thing &&
    yet_another && (last_one == LAST_ONE)) 
{
  ...
}
```

### 3.12 函数返回值

不要在<kbd><small><font color=#CC1E1E>return</font></small></kbd>表达式里加上非必须的圆括号，只有在写<kbd><small><font color=#CC1E1E>x = expr</font></small></kbd>要加上括号的时候才使用括号。例如：

```c++
return result;                  // 返回值很简单, 没有圆括号.
                                // 可以用圆括号把复杂表达式圈起来, 改善可读性.
return (some_long_condition &&
        another_condition);
return (value);                 // 毕竟没人会写 var = (value);
return(result);                 // return 可不是函数！
```

### 3.13 变量及数组初始化

用<kbd><small><font color=#CC1E1E>=</font></small></kbd>，<kbd><small><font color=#CC1E1E>()</font></small></kbd>和<kbd><small><font color=#CC1E1E>{}</font></small></kbd>均可，但是要注意列表初始化会涉及<kbd><small><font color=#CC1E1E>std::initializer_list</font></small></kbd>构造函数的问题。


### 3.14 预处理指令

预处理指令不要缩进。即使预处理指令位于缩进代码块中，指令也应从行首开始。例如：


```c++
// 符合规范的示例
    if (lopsided_score) {
#if DISASTER_PENDING      
        DropEverything();
#if NOTIFY               
        NotifyClient();
#endif
#endif
        BackToNormal();
    }

// 不符合规范的示例
    if (lopsided_score) {
        #if DISASTER_PENDING  // 差 - "#if" 应该放在行开头
        DropEverything();
        #endif                // 差 - "#endif" 不要缩进
        BackToNormal();
    }
```

### 3.15 类格式

访问控制块的声明依次序是<kbd><small><font color=#CC1E1E>public:</font></small></kbd>，<kbd><small><font color=#CC1E1E>protected:</font></small></kbd>，<kbd><small><font color=#CC1E1E>private:</font></small></kbd>，不需要空格缩进。例如：

```c++
class MyClass : public MyBaseClass {
public:      
    MyClass();  
    explicit MyClass(int var);
    ~MyClass() {}

    void set_some_var(int var) { _some_var = var; }
    int some_var() const { return _some_var; }

protected:
    void SomeFunction();
    void SomeFunctionThatDoesNothing() { }

private:
    bool SomeInternalFunction();

    int _some_var;
    int _some_other_var;
};
```

### 3.16 构造函数初始值列表

构造函数初始化列表放在同一行或按4个空格缩进并排多行。例如：

```c++
// 如果所有变量能放在同一行:
MyClass::MyClass(int var) : _some_var(var) 
{
    DoSomething();
}

// 如果不能放在同一行,
// 必须置于冒号后, 并缩进 4 个空格
MyClass::MyClass(int var)
    : _some_var_(var), _some_other_var(var + 1) 
{
    DoSomething();
}

// 如果初始化列表需要置于多行, 将每一个成员放在单独的一行
// 并逐行对齐
MyClass::MyClass(int var)
    : _some_var(var),             
      _some_other_var(var + 1) 
{  
  DoSomething();
}

// 右大括号 } 可以和左大括号 { 放在同一行
// 如果这样做合适的话
MyClass::MyClass(int var)
    : _some_var(var) {}
```

### 3.17 命名空间格式化

命名空间内容不缩进。

声明嵌套命名空间时，每个命名空间都独立成行。

### 3.18 水平留白和垂直留白

水平留白的使用根据在代码中的位置决定，永远不要在行尾添加没意义的留白。

添加冗余的留白会给其他人编辑时造成额外负担。因此，行尾不要留空格。如果确定一行代码已经修改完毕，将多余的空格去掉；或者在专门清理空格时去掉(尤其是在没有其他人在处理这件事的时候)。

垂直留白越少越好。尤其是：两个函数定义之间的空行不要超过 2 行。函数体首尾不要留空行，函数体中也不要随意添加空行。除非添加的空行能显著提升程序的可读性，否则，**不要使用空行**。

- [返回目录](./0_title.md)
- [上一节: 注释](./2_comment.md)
- [下一节: ...]()