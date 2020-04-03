# *dustbin226* 的C++编程规范
###### C++  Style Guidence for *dustbin226*

- 作者：Yujian Lin (yjlin2018@mail.nwpu.edu.cn)
- 创建日期：2020年4月2日
- 最后修改：2020年4月3日

## 1. 命名约定

命名一致性约定是代码风格管理中最为重要的部分，因此放在开头。数学系Coder有时候会有一些很坏的命名习惯，包括但不限于如下样例：

- 多个重复字母作为循环变量
```matlab
% 循环变量用j是约定俗成的
for j = 1 : 10
    % 好吧，两个还勉强能看...
    for jj = 1 : 10
        ......
        % 这能忍???
        for jjjjj = 1 : 10
            ......
        end
        ......
    end
end
```
- 随意到令人发指的命名
```matlab
% 至少数学系自己人看得懂这是个二维坐标
x = 1;
y = 1;
p = [x, y];

% 好吧，这个也勉强能懂...
xx = 2;
yy = 2;
pp = [xx, yy];

% What the f*ck is that???
xixi = 1;
haha = @z (z * xixi);
function hahaxixi = xixihaha(blahblahblah)
    ......
end
```

这样的代码虽然符合工作的需要(~~能跑就行~~)，但是在程序修改检查或日后重读时会带来严重的影响。这里摘录[Google C++风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide)中[命名约定](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming)一节的开头。

> 最重要的一致性规则是命名管理。命名的风格能让我们在不需要去查找类型声明的条件下快速地了解某个名字代表的含义：类型，变量，函数，常量，宏，等等。甚至，我们大脑中的模式匹配引擎非常依赖这些命名规则。
> 命名规则具有一定随意性，但相比按个人喜好命名，一致性更重要，所以无论你认为它们是否重要，规则总归是规则。

因此，为了至少让*dustbin226* 的三个参与者互相能够看懂彼此的代码，遵守命名规范是有必要的。

### 1.1 通用命名规则

> 函数命名，变量命名，文件命名要有描述性，少用缩写。

尽可能的使用描述性的命名。相比于少打几个字母，能让他人或者一段时间后的自己读懂要重要得多。不过，广为人知的特定缩写是可以接受的。例如：

```c++
/* 好的命名示例 */
std::string my_name;    // 没有缩写
int my_class_id;        // id是常见缩写
int dns_num;            // dns和num都很常见

/* 不好的命名示例 */
int n;                  // 没有任何意义
int ierr;               // 缩写含糊不清(虽然在Fortran MPI中经常出现)
int dstb;               // 连dustbin226自己的成员都不会懂的省略
```

对于C++来说，有一些缩写是约定俗成，比如用<kbd><small><font color=#CC1E1E>i</font></small></kbd>和<kbd><small><font color=#CC1E1E>j</font></small></kbd>表示循环变量，用<kbd><small><font color=#CC1E1E>T</font></small></kbd>表示模板参数等。常用的一些缩写可以参照如下对照表。

| 全称  |  缩写 |
| :----: | :----: |

> 假装这里有个表格

### 1.2 文件命名

文件名全部小写，单词之间使用下划线(<kbd><small><font color=#CC1E1E>\_</font></small></kbd>)分割。Google规范中允许使用下划线(<kbd><small><font color=#CC1E1E>\_</font></small></kbd>)和连字符(<kbd><small><font color=#CC1E1E>\-</font></small></kbd>)两种分隔符，但是为了和STL等已有的库保持一致，我们选择下划线。

- 可接受的文件命名
    - <kbd><small><font color=#CC1E1E>my_kernel.cpp</font></small></kbd>
    - <kbd><small><font color=#CC1E1E>huqiang_simple_allocator.cpp</font></small></kbd>
    - <kbd><small><font color=#CC1E1E>a_very_very_very_long_but_useful_headfile.h</font></small></kbd>

- 不接受的文件命名
    - <kbd><small><font color=#CC1E1E>LiAsNH3Vector.cpp</font></small></kbd>
    - <kbd><small><font color=#CC1E1E>YujianLinStack.cpp</font></small></kbd>
    - <kbd><small><font color=#CC1E1E>connect-by-hyphen-is-also-rejected.h</font></small></kbd>

C++文件以<kbd><small><font color=#CC1E1E>.cpp</font></small></kbd>结尾，头文件以<kbd><small><font color=#CC1E1E>.h</font></small></kbd>结尾。**不要使用**在诸如<kbd><small><font color=#CC1E1E>/usr/include</font></small></kbd>等文件夹下已经存在的文件名。

应该尽量让文件名更加明确，比如<kbd><small><font color=#CC1E1E>fem_poission_l2error.h</font></small></kbd>比<kbd><small><font color=#CC1E1E>l2error.h</font></small></kbd>好得多 (当然<kbd><small><font color=#CC1E1E>fem</font></small></kbd>这种其他人不一定理解的缩写应该尽量避免)。

如果定义类，文件名**必须**成对出现。比如<kbd><small><font color=#CC1E1E>poission_weakform.cpp</font></small></kbd>和<kbd><small><font color=#CC1E1E>poission_weakform.h</font></small></kbd>对应于类<kbd><small><font color=#CC1E1E>PoissionWeakform</font></small></kbd>。

### 1.3 类型命名

类名、结构体名、类型定义(<kbd><small><font color=#CC1E1E>typedef</font></small></kbd>)、<kbd><small><font color=#CC1E1E>using</font></small></kbd>别名、枚举和类型模板参数等均以大写字母开始，每个单词首字母均大写，不含下划线。例如：

```c++
/* 类和结构体 */
class MyClass { /* ... */ };
struct MyStruct { /* ... */ };

/* 类型定义和using别名 */
typedef unordered_map<int, pair<int, int>::iterator> AStrangeHashMap;
using Matrix2D = array<array<int, 2>, 2>;

/* 枚举 */
enum class FemErrorType { /* ... */  };
```

### 1.4 变量命名

变量(包括函数参数)和数据成员名一律小写，单词之间用下划线连接。驼峰命名法是**不接受**的。

#### 1.4.1 普通变量命名

按照变量命名一般规则即可。例如：

```c++
/* 好的命名示例 */
std::string my_name;            // 下划线分割
std::string country;            // 全部小写也可以

/* 不好的命名示例 */
int Student_ID;                 // 不应该有大写
std::string studentName;        // 驼峰命名是不接受的
double dNum;                    // 更不要用匈牙利命名法!
```

#### 1.4.2 类数据成员

<kbd><small><font color=#CC1E1E>TODO: 具体使用哪种待定</font></small></kbd>
类的数据成员(成员变量)不管是静态还是非静态，都和普通变量一样命名，但是要<kbd><small><font color=#CC1E1E>TODO: 在其前面/在其后面</font></small></kbd>添加下划线。

#### 1.4.3 结构体数据成员

结构体的数据成员不管是静态还是非静态，都和普通变量一样命名，**不需要**像类成员变量一样<kbd><small><font color=#CC1E1E>TODO: 在其前面/在其后面</font></small></kbd>添加下划线。

### 1.5 常量命名
#### 1.5.1 声明为const的变量

声明为<kbd><small><font color=#CC1E1E>const</font></small></kbd>，或运行期间其值始终保持不变的变量命名时以字母<kbd><small><font color=#CC1E1E>k</font></small></kbd>开头，并且大小写混合，不加下划线。例如：

```c++
const int kDaysInAWeek = 7;
```

按照Google规范的要求，所有具有静态储存类型的变量都应当以此方式命名。对于自动变量等，可以不按照该规则而按照普通变量的规则命名。例如：

```c++
int MyFunction(const int days_in_a_week);
```

#### 1.5.2 声明为constexpr的字面量

声明为<kbd><small><font color=#CC1E1E>constexpr</font></small></kbd>的字面量和宏使用方式相似，因此按照宏的方式命名，全大写并且加下划线。例如：

```c++
/* instead of '#define DAYS_IN_A_WEEK 7' */
constexpr int DAYS_IN_A_WEEK = 7;

/* use auto instead of int */
constexpr auto WEEKDAYS_IN_A_WEEK = 5;
```

### 1.6 宏命名

> **注意**： 在使用宏之前，好好想想能不能用字面量等方式替代。除非不得不用，否则**不允许**使用宏。

确保上面的注意事项已经满足。如果要命名宏，使用全大写加下划线。例如：

```c++
#define FLOAT(x) ...;
#define LOW_PRECISION_PI 3.14;
#define HIGH_PRECISION_PI 3.14159265358979324;
```

### 1.7 函数命名

常规函数使用大小写混合、首字母大写的驼峰命名法，不使用下划线。例如：

```c++
void FillMyVector(...);
bool DeleteUrl(...);
```

如果函数命名中出现了首字母缩写的单词，为了便于辨认，将它们视作一个单词进行首字母大写。例如：

```c++
void GenerateFemSpace(...);         // good
void GenerateFEMSpace(...);         // bad
```

函数的命名有一个特例：取值/设值函数(即<kbd><small><font color=#CC1E1E>get/set</font></small></kbd>函数)的命名应该与变量一致。不过这不是强制要求，实际命名时怎么舒服怎么来。例如：

<kbd><small><font color=#CC1E1E>TODO：类成员变量的下划线尚未确定，以前置代替</font></small></kbd>
```c++
class MyClass
{
public:
    void set_count(int input_count);    // 和变量名一致
    int count();                        // 实际应该是get_count
                                        // 不过直接用变量名也不会混淆

private:
    int _count;
}
```

### 1.8 命名空间命名

根据使用习惯，我们没有选择Google的命名空间命名方式(和变量相同，小写加下划线连接)，而是选择了微软推荐的命名规范：命名空间采用和类型命名相同的方式，即单词首字母大写，不使用下划线等连接符。**不要**在命名空间中使用缩写。

**最高级的命名空间名字与项目名称要相同**。命名空间中的代码应当存放于和命名空间名字相匹配的文件夹或其子文件夹中。

要避免嵌套的命名空间与常见的顶级命名空间发生冲突。尤其是**不要**创建嵌套的<kbd><small><font color=#CC1E1E>std</font></small></kbd>命名空间。例如：

```c++
namespace Dustbin226FirstProject {}
namespace InterpolationPolynomial
{
    namespace Lagrange {}
}
```

对<kbd><small><font color=#CC1E1E>internal</font></small></kbd>命名空间，要当心加入到同一 <kbd><small><font color=#CC1E1E>internal</font></small></kbd>命名空间的代码之间发生冲突 (由于内部维护人员通常来自同一团队, 因此常有可能导致冲突). 在这种情况下, 请使用文件名以使得内部名称独一无二 。

### 1.9 枚举命名

枚举名属于类型，因此采用大小写混合的方式。

单独的枚举值应该优先采用常量的命名方式，即命名时以字母<kbd><small><font color=#CC1E1E>k</font></small></kbd>开头，并且大小写混合，不加下划线。考虑到使用习惯，枚举变量的命名也和枚举名命名方式一样。例如：

```c++
enum class ErrorType
{
    L2Error,
    H1Error,
    L2hError,
    H1hError
};
```

> 注意：另外一种惯例是将枚举值按照宏的方式命名，但是可能会导致编译期问题，因此**不允许**使用宏命名方式来命名枚举值。
