# *dustbin226* 的C++编程规范
###### C++  Style Guidence for *dustbin226*

- 作者：Yujian Lin (yjlin2018@mail.nwpu.edu.cn)
- 创建日期：2020年4月27日
- 最后修改：2020年4月27日

## 4. 头文件

通常每一个<kbd><small><font color=#CC1E1E>.cpp</font></small></kbd>文件都有一个对应的<kbd><small><font color=#CC1E1E>.h</font></small></kbd>文件。也有一些常见例外，如单元测试代码和只包含<kbd><small><font color=#CC1E1E>main()</font></small></kbd>函数的文件。

正确使用头文件可令代码在可读性、文件大小和性能上大为改观。


### 4.1 Self-contained 头文件

头文件应该能够自给自足(self-contained，也就是可以作为第一个头文件被引入)，以<kbd><small><font color=#CC1E1E>.h</font></small></kbd>结尾。换言之，用户和重构工具不需要为特别场合而包含额外的头文件。一个头文件要有<kbd><small><font color=#CC1E1E>#define</font></small></kbd>保护，统统包含它所需要的其它头文件。

> 有个例外：即一个文件并不是 self-contained 的，而是作为文本插入到代码某处。或者，文件内容实际上是其它头文件的特定平台（platform-specific）扩展部分。这些文件就要用<kbd><small><font color=#CC1E1E>.inc</font></small></kbd>文件扩展名。

如果<kbd><small><font color=#CC1E1E>.h</font></small></kbd>文件声明了一个模板或内联函数，同时也在该文件加以定义。凡是有用到这些的<kbd><small><font color=#CC1E1E>.cpp</font></small></kbd>文件，就得统统包含该头文件，否则程序可能会在构建中链接失败。

> 有个例外：如果某函数模板为所有相关模板参数显式实例化，或本身就是某类的一个私有成员，那么它就只能定义在实例化该模板的<kbd><small><font color=#CC1E1E>.cpp</font></small></kbd>文件里。

### 4.2 #define 保护

所有头文件都应该使用<kbd><small><font color=#CC1E1E>#define</font></small></kbd>来防止头文件被多重包含。

<kbd><small><font color=#CC1E1E>#define</font></small></kbd>保护的命名格式如下：

```c++
#define _<PROJECT>_<PATH>_<FILE>_H_ .
```

为保证唯一性，头文件的命名应该基于所在项目源代码树的全路径。例如项目<kbd><small><font color=#CC1E1E>foo</font></small></kbd>中的头文件<kbd><small><font color=#CC1E1E>foo/src/bar/baz.h</font></small></kbd>可按如下方式保护：

```c++
#ifndef _FOO_BAR_BAZ_H_
#define _FOO_BAR_BAZ_H_
...
/* Add New Contents Here */
#endif // _FOO_BAR_BAZ_H_
```

为了防止将新添加的内容写到<kbd><small><font color=#CC1E1E>#endif</font></small></kbd>之后，在该语句前添加如上例所示的一句注释作为提示(<del>虽然作用聊胜于无</del>)。

### 4.3 前置声明

> 尽可能地避免使用前置声明。使用<kbd><small><font color=#CC1E1E>#include</font></small></kbd>包含需要的头文件即可。

所谓**前置声明**(forward declaration)是类、函数和模板的纯粹声明，没有定义。

虽然前置声明可以明显的减少编译时间，但是前置声明隐藏了依赖关系，头文件改动时，用户的代码会跳过必要的重新编译过程。前置声明可能会被库的后续更改所破坏。前置声明函数或模板有时会妨碍头文件开发者变动其API，例如扩大形参类型，加个自带默认参数的模板形参等等。

因此，我们选择尽量避免前置声明那些定义在其他项目中的实体。

### 4.4 内联函数

只有当函数**短于10行且没有循环或选择语句块**时才将其定义为内联函数。


有些函数即使声明为内联的也不一定会被编译器内联，比如虚函数和递归函数就不会被正常内联。通常递归函数不应该声明成内联函数。

### 4.5 #include的路径及顺序

严格按照以下头文件包含顺序。使用标准的头文件包含顺序可增强可读性，避免隐藏依赖。

- 相关头文件
- C 库
- C++ 库
- 其他库的 .h
- 本项目内的 .h

每一块头文件包含结束后，添加一个空行以示分割。

项目内头文件应按照项目源代码目录树结构排列，避免使用UNIX特殊的快捷目录<kbd><small><font color=#CC1E1E>.</font></small></kbd>(当前目录)或<kbd><small><font color=#CC1E1E>..</font></small></kbd>(上级目录)。

所依赖的符号 (symbols)被哪些头文件所定义，就应该包含哪些头文件，前置声明 (forward declarations)情况除外。比如要用到<kbd><small><font color=#CC1E1E>bar.h</font></small></kbd>中的某个符号，哪怕所包含的<kbd><small><font color=#CC1E1E>foo.h</font></small></kbd>已经包含了<kbd><small><font color=#CC1E1E>bar.h</font></small></kbd>，也照样得包含<kbd><small><font color=#CC1E1E>bar.h</font></small></kbd>。

举例来说，<kbd><small><font color=#CC1E1E>google_awesome_project/src/foo/internal/fooserver.cpp</font></small></kbd>的包含次序如下:

```c++
#include "foo/public/fooserver.h" // 优先位置

#include <sys/types.h>
#include <unistd.h>

#include <unordered_map>
#include <vector>

#include "base/basictypes.h"
#include "base/commandlineflags.h"
#include "foo/public/bar.h"
```

有时，平台特定代码需要条件编译（conditional includes），这些代码可以放到其它头文件包含之后。例如：

```c++
#include "foo/public/fooserver.h"

#include "base/port.h"  // For LANG_CXX11.

#ifdef LANG_CXX11
#include <initializer_list>
#endif  // LANG_CXX11
```

- [返回目录](./0_title.md)
- [上一节: 代码格式](./3_formatting.md)
- [下一节: ...]()