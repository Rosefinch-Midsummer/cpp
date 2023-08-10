# vector和string程序设计

<!-- toc -->

## STL简介

在C++中，STL（标准模板库，Standard Template Library）是一组通用的模板类和函数，提供了多种常用数据结构和算法的实现。STL的设计目标是提供高效、可复用的组件，使得开发人员能够更加简单地编写高质量的C++代码。

STL包含了以下几个主要组件：

1. 容器（Containers）：STL提供了多种容器，如向量（vector）、链表（list）、双向队列（deque）、队列（queue）、栈（stack）、集合（set）、映射（map）等。容器用于存储和组织数据，每种容器都具有特定的特性和适用场景。

2. 算法（Algorithms）：STL提供了大量的算法，如排序、查找、合并、遍历等。这些算法可以直接应用于各种容器，使得开发人员无需自己实现这些常见的操作，提高了代码的复用性和可读性。

3. 迭代器（Iterators）：STL的迭代器提供了一种统一的访问容器元素的方式，使得算法可以独立于具体容器的实现。迭代器可以遍历容器中的元素，支持指针类的操作，如解引用、递增等。

4. 函数对象（Function Objects）：STL中的函数对象是可调用对象，可以像函数一样使用。函数对象可以用于算法中的操作，如比较、排序等，提供了一种灵活的扩展方式。

5. 适配器（Adapters）：STL提供了适配器，用于将容器和算法进行适配，以满足特定的需求。常见的适配器有栈适配器（stack adapter）和队列适配器（queue adapter）等。

STL的作用是提供了一组高效、可复用的组件，使得C++开发人员可以更加方便地进行数据结构和算法的实现。通过使用STL，开发人员可以减少重复的工作，提高代码的可读性和可维护性，并且可以利用STL的高效实现来获得更好的性能。此外，STL还促使了C++标准库的发展和规范化，成为C++编程中不可或缺的一部分。

## 动态数组vector

### vector介绍

vector是一个容器，能存放各种类型的数据，包括简单类型如int、double、char或自定义的结构体类型或类类型，简单地说，vector是一个能够存放任意类型的动态数组，可以动态改变大小。

在C++中，`std::vector`是标准模板库（STL）提供的动态数组容器，它能够自动调整大小以适应元素的添加和删除操作。`std::vector`提供了与普通数组类似的访问方式，同时还提供了一系列方便的成员函数和操作符重载来简化数组的操作。

要使用`std::vector`，首先需要包含头文件`<vector>`。

下面是一些常见的 `std::vector` 操作示例：

1. 创建一个空的 `std::vector`：

```cpp
std::vector<int> myVector;//也可以创建一个初始大小为10的动态数组vector<int> nums(10);
```

2. 在 `std::vector` 中添加元素：

```cpp
myVector.push_back(10);  // 向尾部添加元素
myVector.push_back(20);
myVector.push_back(30);
```

3. 获取 `std::vector` 的大小（元素个数）：
```cpp
int size = myVector.size();
```

4. 访问 `std::vector` 中的元素：
```cpp
int element = myVector[index];  // 使用下标访问元素
```

5. 修改 `std::vector` 中的元素：
```cpp
myVector[index] = newValue;
```

6. 迭代遍历 `std::vector` 中的元素：
```cpp
for (const auto& element : myVector) {
    // 使用 element 进行操作
}
```

7. 删除 `std::vector` 中的元素：
```cpp
myVector.pop_back();  // 删除尾部元素
```

8. 清空 `std::vector` 中的所有元素：
```cpp
myVector.clear();
```

`std::vector` 还提供了许多其他的成员函数和操作符重载，用于插入、删除、查找等操作，以及获取容量、判断是否为空等功能。可以查阅C++的文档或参考教程来获取更详细的信息和示例。

需要注意的是，`std::vector` 是动态数组，它会在需要时自动调整大小，但这可能会导致内存重新分配和元素复制，因此在需要频繁添加或删除元素的情况下，可能会影响性能。
### 创建vector对象

（1）`vector<T> v`：创建空的vector

（2）`vector<T> v(n)`：创建一个大小为n的vector，每个元素具有默认值

（3）`vector<T> v(n,e）`：创建一个大小为n的vector，每个元素都是e

（4）`vector<T> v2(v1)`：创建同类型的v2，并将拷贝v1的所有元素如`vector<char> v1(5,'a');vector<char> v2(v1);`

（5）`vector<T> v(begin,end)`：创建一个vector，以区间`[begin,end)`为元素初值

begin作为一个迭代器（或者说指针），通过拷贝迭代器区间`[begin,end)`的元素值（不包括end所指向的元素），创建一个新的vector对象。例如下面的代码利用int数组iArray各元素值，创建了vector对象。

```cpp
int iArray[]={11,13,19,23,27};
vector<int> v(iArray,iArray+3);
```

代码将`iArray[0]`到`iArray[2]`拷贝到vector对象v中，从而v中有三个元素。

注意：

如果vector中存放的元素类型是类，并且数据成员有动态生成的、存在深拷贝的问题的话，这个时候该类必须具有拷贝构造函数，否则在vector的创建、插入等拷贝的过程中会发生错误。

比如有以下类声明：

```cpp
class student 
{
private:
    int ID;
    char *name;
public:
    student(char* n)
    {
        name = new char[strlen(n)+1];
        strcpy(name,n);
    }
};
```

该类没有拷贝构造函数，不可放到vector容器中。

由于该类中的name需要在构造函数中`动态生成`，所以存在深拷贝问题。如果class student没有声明拷贝构造函数（如上的代码），则`vector<student> v;`创建出来的vector在使用的过程中会发生错误。

如前所述，student类应该声明拷贝构造函数、析构函数、重载赋值运算符。

C++11增加了定义时初始化的操作如下：

```cpp
vector<int> month={1,2,3,4,5};
//也可以去掉等号如下：
vector<int> month {1,2,3,4,5};
```
### push_back尾部插入

 在 `std::vector` 中添加元素：

```cpp
myVector.push_back(10);  // 向尾部添加元素
myVector.push_back(20);
myVector.push_back(30);
```

push_back操作不会造成溢出。当vector已经装满时，它会自动扩充容量。

### 遍历操作

（1）基于下标的遍历

`v.at(idx)`函数也能获取某下标处的元素，at函数会进行越界检查，如果下标越界，将抛出异常。

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v(3);
    v.push_back(3);
    v.push_back(10);
    v.push_back(19);
    for(int i=0;i<v.size();i++)
        cout<<v.at(i)<<" ";//cout<<v[i]<<" ";

    return 0;
}
```

```
0 0 0 3 10 19
```

（2）基于范围的for循环

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v(3);
    v.push_back(3);
    v.push_back(10);
    v.push_back(19);
    for(auto elem:v)//for(int elem:v)
        cout<<elem<<" ";

    return 0;
}

```

注意：使用范围的for循环，elem是从v中复制出来的元素，所以不能修改v中的值。

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v(3);
    v.push_back(3);
    v.push_back(10);
    v.push_back(19);
    for(auto elem:v)
        elem+=2;
    for(auto elem:v)
        cout<<elem<<" ";//0 0 0 3 10 19

    return 0;
}
```

此时需要使用引用。

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v(3);
    v.push_back(3);
    v.push_back(10);
    v.push_back(19);
    for(auto &elem:v)
        elem+=2;
    for(auto elem:v)
        cout<<elem<<" ";//2 2 2 5 12 21

    return 0;
}
```

实际上，为了提高程序执行效率，避免元素的拷贝，通常使用引用，代码模板如下：

```cpp
for(auto &elem:v)
        //对elem进行处理
```

### 迭代器

迭代器是一种检查容器内元素并遍历元素的数据类型。C++更趋向于使用迭代器而不是下标操作，因为标准库为每一种标准容器如vector定义了一种迭代器类型，而只有少数容器如vector支持下标操作访问容器元素。

（1）**定义和初始化**

每种容器都定义了自己的迭代器类型如vector：

`vector<int>::iterator iter1;`定义一个名为iter1的迭代器变量

`vector<int>::reverse_iterator iter2;`定义一个名为iter2的逆向迭代器变量

每种容器都定义了一对名为begin和end的函数，用于返回迭代器。部分容器如vector还定义了一对名为rbegin和rend的函数用于返回逆向迭代器。各函数的功能如下表：

操作|效果
-|-
begin()|返回一个迭代器，指向第一个元素
end()|返回一个迭代器，指向最后一个元素之后
rbegin()|返回一个逆向迭代器，指向逆向遍历的第一个元素，即容器的最后一个元素
rend()|返回一个逆向迭代器，指向逆向遍历的最后一个元素之后，即容器的第一个元素之前

下面对迭代器进行初始化操作：

```cpp
vector<int> v(10);
vector<int>::iterator iter1=v.begin();//将迭代器iter1初始化为指向v的第一个元素

vector<int>::reverse_iterator iter2=v.end();//将迭代器iter2初始化为指向v的最后一个元素的下一个位置
```

注意end并不指向容器的任何元素，而是指向容器的最后元素的下一位置，称为超出末端迭代器，如果vector为空，则begin返回的迭代器和end返回的迭代器相同。一旦向上面这样定义和初始化，就相当于把该迭代器和容器进行了某种关联，就像把一个指针初始化为指向数组的某一个元素一样。

（2）**常用操作**

对于vector和string等少数容器，我们可以将迭代器理解成数组的指针，因此具有下面的常用操作：

操作|作用
-|-
`*iter`|对iter进行解引用，返回迭代器iter指向的元素的引用
iter->men|对iter进行解引用，获取指定元素中名为men的成员，等价于`(*iter).men`
++iter|给iter加一，使其指向容器的下一个元素
iter--|给iter减一，使其指向容器的前一个元素
`iter1==iter2`|比较两个迭代器是否相等，当它们指向同一个容器的同一个元素或者都指向都一个容器的超出末端的下一个位置时，它们相等
iter+n|在迭代器上加整数n，将产生指向容器中后面第n各元素的迭代器，新计算出来的迭代器必须指向容器中的元素或超出容器末端的下一个元素
`iter[n]`|相当于`*(iter+n)`
iter1-iter2|两个迭代器相减，得出两个迭代器的距离，两个迭代器必须指向容器中的元素或超出容器末端的下一个元素
!=、<=|元素靠后的迭代器大于靠前的迭代器，两个迭代器必须指向同一个容器中的元素或超出容器末端的下一个元素

假设已经声明`vector<int> v`，下面用迭代器来遍历v容器，把每个元素加2：

```cpp
for(vector<int>::iterator iter=v.begin();iter!=v.end();++iter)
        *iter+=2;
```

由于迭代器的类型写起来很复杂，我们也可以使用auto简化如下：

```cpp
for(auto iter=v.begin();iter!=v.end();++iter)
        *iter+=2;
```

这里再看一个简单的例子——用迭代器算术操作来移动迭代器直接指向某个元素：

```cpp
vector<int>::iterator mid=v.begin()+v.size()/2;
```

以上语句初始化mid迭代器，使其指向v中最靠近正中间的元素。

从这些运算可以看出，将迭代器看成数组的指针，理解起来非常容易。

（3）**reverse_iterator**

下面代码是现在vector中查找最后一个等于3的元素并将它修改为0

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v(10,3);
    v.push_back(8);
    v.push_back(9);
    v.push_back(10);
    for(vector<int>::reverse_iterator iter=v.rbegin();iter!=v.rend();++iter)
    {
        if(*iter==3)
        {
            *iter=0;
            break;
        }
    }
    for(auto elem:v)
        cout<<elem<<" ";
    return 0;
}
```

```
3 3 3 3 3 3 3 3 3 0 8 9 10
```

需要注意的是，对于reverse_iterator类型的iter，++iter使得指针往前移动。

（4）**const_iterator**

每种容器都定义了一种名为const_iterator的类型。该类型的迭代器只能读取容器中的元素，不能用于改变其值。之前的例子中，普通迭代器可以对容器中的元素进行解引用并修改，但const_iterator类型的迭代器只能用于读不能进行重写。例如可以进行如下操作：

```cpp
for(vector<int>::const_iterator iter=v.begin();iter!=v.end();++iter)
	cout<<*iter<<endl;//合法读取容器中元素值
```

```cpp
for(vector<int>::const_iterator iter=v.begin();iter!=v.end();++iter)
	*iter=0;//不合法，不能进行写操作
```

const_iterator和const iterator不一样，后者是一个常量，对迭代器进行声明时，必须对迭代器进行初始化，并且不能修改迭代器的值。**而const_iterator生命的迭代器可以修改迭代器自身的值，但是不能通过修改它修改vector的元素的值。这有点像常量指针和指针常量的关系。** 例如

```cpp
vector<int> ivec(10);
const vector<int>::iterator iter=v.begin();
*iter=0;//合法，可以改变其指向的元素的值
++iter;//不合法，无法改变其指向的位置
```

对于const类型的容器，必须使用const_iterator，而不能使用iterator。比如：

```cpp
#include <iostream>
#include <vector>
using namespace std;

void print(const vector<int>&v)
{
    for(vector<int>::const_iterator iter=v.begin();iter!=v.end();++iter)
        cout<<*iter<<" ";
}

int main() {
    vector<int> v;
    v.push_back(8);
    v.push_back(9);
    v.push_back(10);
    print(v);//8 9 10

    return 0;
}
```

（5）**能使迭代器失效的操作**

由于一些对容器的操作如删除元素或插入元素等会修改容器的内在状态，这会使得原来的迭代器失效，也可能同时是其他迭代器失效。使用无效的迭代器是没有定义的，可能会导致和使用野指针相同的问题。所以在使用迭代器编写程序时，需要特别留意哪些操作会使迭代器失效。使用无效迭代器会导致严重的运行时错误。

比如下面的函数删除vector中所有的元素3，其中erase函数将使得迭代器iter失效：

```cpp
void delete1(vector<int>&v, int num)
{
    for(vector<int>::iterator iter=v.begin();iter!=v.end();)
    {
        if(*iter==num)
            iter=v.erase(iter);//一定要将返回的迭代器赋值给iter
        else
            ++iter;
    }
}
```

代码中，执行v.erase(iter)后，iter可能指向不明确，也即迭代器失效，从而后续的操作可能发生错误，但不是必然发生错误，所以要特别注意。

因此需要将函数erase的返回值再赋给iter，语句为`iter=v.erase(iter);`，使得iter指向被删除元素的下一个元素，这样，循环可以继续正常进行。
### vector其它常用操作

操作|效果
-|-
v.size()|返回元素个数
v.empty()|判断容器是否为空，为空返回true
v1=v2|将v2的全部元素赋值给v1
v.assign(n,e)|将元素e的n个拷贝赋值给v如`v.assign(2,2);`
v.assign(beg,end)|讲区间`[beg,end)`的元素赋值给v如`v2.assign(v1.begin()+1,v1.begin()+4);`
v1.swap(v2)|互换v1和v2的元素
v.at(idx)|返回索引idx所标识的元素的引用，进行越界检查，如果越界会抛出异常
`v[idx]`|返回索引idx所标识的元素的引用，不进行越界检查
v.front()|返回第一个元素的引用，不检查元素是否存在，如`v.front()=10;`修改第0个元素
v.back()|返回最后一个元素的引用，不检查元素是否存在
v.insert(pos,e)|在pos位置插入元素e的副本，并指向插入的第一个元素
v.insert(pos,n,e)|在pos位置插入n个元素e的副本，并指向插入的第一个元素
v.insert(pos,begin,end)|在pos位置插入区间`[begin,end)`内所有元素的副本，并指向pos
v.pop_back()|移除最后一个元素，但不返回最后一个元素
v.erase(pos)|删除pos位置的元素，返回下一个元素的位置
v.erase(begin,end)|删除区间`[begin,end)`的元素，返回下一个元素的位置
v.clear()|移除所有元素，清空容器
v.resize(num)|将元素数量改为num（增加的元素用default构造函数产生，多余的元素被删除）
v.resize(num,e)|将元素数量改为num（增加的元素是e的副本）

insert操作实例

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;
    v.push_back(8);
    v.push_back(9);
    v.push_back(10);
    vector<int>::iterator iter=v.insert(v.begin()+1,8);
    //vector<int>::iterator iter=v.insert(v.begin()+1,5,8);
    //vector<int> v1={1,2,3,4};
    //vector<int>::iterator iter=v.insert(v.begin()+1,8);
    cout<<iter-v.begin()<<endl;//1

    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v2={10,20,30,40};
    
    vector<int> v1={1,2,3,4};
    vector<int>::iterator iter=v2.insert(v2.begin()+1,v1.begin(),v1.end()-1);
    //代码执行后v2元素为{10,1,2,3,20,30,40}
    vector<int>::iterator iter=v2.insert(v2.begin()+1,v1.rbegin(),v1.rend()-1);
    //代码执行后v2元素为{10,4,3,2,20,30,40}
    cout<<iter-v2.begin()<<endl;//1

    return 0;
}
```

erase操作实例

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v;
    v.push_back(8);
    v.push_back(9);
    v.push_back(10);
    vector<int>::iterator iter=v.erase(v.begin()+1);
    cout<<*iter<<endl;//10

    return 0;
}

```

```cpp
#include <iostream>
#include <vector>
using namespace std;


int main() {
    vector<int> v={1,2,3,4,5,6,7,8};

    vector<int>::iterator iter=v.erase(v.begin()+1,v.begin()+5);
    //v={1,6,7,8}
    cout<<*iter<<endl;//6

    return 0;
}
```

### 二维vector

vector中的元素也可以是vector，从而可以得到二维、三维等多维的vector。要声明一个二维的vector，可以使用以下语句：`vector<vector<int>> Array;`。如果C++版本在是C++11以前，则以上声明中`>>`内要有一个空格，否则编译报错，如：`vector<vector<int> > Array;`

下面举例说明二维vector的应用：输出一个杨辉三角形。

```cpp
#include <iostream>
#include <vector>
#include <iomanip>
using namespace std;


int main() {
    int i=0,j=0;
    vector<vector<int>> Array;
    vector<int> aLine;
    aLine.push_back(1);
    Array.push_back(aLine);//第0行是1
    aLine.push_back(1);
    Array.push_back(aLine);//第1行是1 1
    for(i=2;i<9;i++)
    {
        vector<int> lastLine=Array[i-1];
        aLine.clear();
        aLine.push_back(1);
        for(j=1;j<lastLine.size();j++)
            aLine.push_back(lastLine[j-1]+lastLine[j]);

        aLine.push_back(1);//最后一个元素是1
        Array.push_back(aLine);

    }
    for(j=0;j<Array.size();j++)
    {
        for(i=0;i<Array[j].size();i++)
            cout<<setw(2)<<Array[j][i]<<" ";
        cout<<endl;
    }

    return 0;
}
```

```
 1
 1  1
 1  2  1
 1  3  3  1
 1  4  6  4  1
 1  5 10 10  5  1
 1  6 15 20 15  6  1
 1  7 21 35 35 21  7  1
 1  8 28 56 70 56 28  8  1
```

## 字符串string

### 创建对象

在前面的章节中，存储字符串使用的是字符数组，对于字符串的处理是借助cstring头文件提供的字符串函数完成的。

但是这种方式不符合面向对象风格，并且也又很多不方便的地方，C++标准类库提供了字符串类string。
### 字符串的输入

对于字符串，输入方式主要有以下两种：

（1）cin>>方式

和前面介绍的输入字符、字符数组型字符串等方式一样，输入规则为：

- 读取并忽略开头所有的空白字符（如空格、换行符、制表符）
- 读取字符直至再次遇到空白字符或者流结束符eof，读取终止

```cpp
#include <iostream>
#include <string>
using namespace std;


int main() {
    string s1,s2;
    cout<<"Please input:"<<endl;
    cin>>s1>>s2;
    cout<<s1<<s2<<endl;

    return 0;
}
```

```
Please input:
I love Chunghwa
Ilove
```

（2）getline函数

用getline可以读取整行文本，getline函数的原型为：`istream& getline(istream &is, string &str,char delim='\n');`

第一个参数is为进行读入操作的输入流可以是cin，表示从键盘读入。

第二个参数str用来存储读入的内容。

第三个参数delim为终结符，默认为回车符，也可以自定义终结符。

函数功能为将输入流is中读到的字符存入str中，直到遇到终结符delim或者流结束符EOF才结束，所以读入的长度没有限制。在遇到终结符delim后，delim会被丢弃，不存入str中。在下次读入操作时，将从delim的下个字符开始读入。

```cpp
#include <iostream>
#include <string>
using namespace std;


int main() {
    string s1,s2;
    cout<<"Please input:"<<endl;
    getline(cin,s1);
    getline(cin,s2);
    cout<<s1<<s2<<endl;//连续输出，之间没有换行符

    return 0;
}
```

```
Please input:
I love Chunghwa!
1314
I love Chunghwa!1314
```

以上程序用回车符作为终结符，我们也可以自定义终结符。

```cpp
#include <iostream>
#include <string>
using namespace std;


int main() {
    string s;
    char ch;
    cout<<"Please input:"<<endl;
    getline(cin,s,'#');
    cin>>ch;

    cout<<s<<endl<<ch;

    return 0;
}
```

```
Please input:
abc#def
abc
d
```

从结果可以看出，getline时设置的终结符为'#'，因此，输入字符串”abc#bdf"时，读到s中的字符为abc，然后将'#'从输入缓冲区丢弃，再输入ch时，输入的是字符'd'。

说明：

- 读入的字符串中不包含终结符。如果终结符是回车符，读入的字符串中不包含换行符`'\n'`
- getline操作返回的是is（输入流，我们这里是cin），在输入流结束时（在键盘上输入ctrl+z然后回车即可结束输入流），如果使用if(getline(cin,s))判断，则将得到结果false。如果使用while(getline(cin,s))输入多个字符串，在键盘上输入ctrl+z（回车）结束循环。
- 本节讲的getline是一个全局函数，不是cin.getline()成员函数
- 在getline时，如果碰到的第一个字符就是终结符，则输入的是空字符串。

先读入数字再读入字符串，可以参照下面的代码：

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s;
    int n;
    cin>>n;

    for(int i=0;i<n;i++)
    {
        getline(cin,s);
        cout<<s<<endl;
    }
    return 0;
}
```

```
2

abc
abc
```

原因是输入2之后按了回车，在执行getline函数时，碰到的第一个字符就是回车（终止符）。要解决这个问题，可以在cin>>n之后，增加一个语句cin.get()将该回车符读掉。

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s;
    int n;
    cin>>n;
    cin.get();
    for(int i=0;i<n;i++)
    {
        getline(cin,s);
        cout<<s<<endl;
    }
    return 0;
}
```

```
2
abc
abc
def
def
```
### 遍历操作

（1）**基于下标的遍历**

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s("hello");

    for(int i=0;i<s.length();i++)
    {
        cout<<s.at(i);//cout<<s[i];
    }
    for(int i=0;i<s.size();i++)
    {
        cout<<s.at(i);//cout<<s[i];
    }
    return 0;
}
```

这和字符数组不一样，判断条件写成`s[i]!='\0'`是错误的，不能认为字符串的最后一个字符是`'\0'`。

（2）**基于范围的for循环**

- `for(char ch:s) cout<<ch;`
- `for(char &ch:s) cout<<ch;`
- `for(auto ch:s) cout<<ch;`
- `for(auto &ch:s) cout<<ch;`

在变量ch前加&引用符号时，对ch的修改就是对字符串s中字符的修改。

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s("abcd");

    for(auto &ch:s)
    {
        ch+=1;
    }
    cout<<s<<endl;//bcde
    return 0;
}
```

（3）**基于迭代器的循环**

和vector一样，string有iterator、reverse_iterator、const_iterator。

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s="hello";

    for(string::iterator iter=s.begin();iter<s.end();iter++)
    {
        cout<<*iter;//hello
    }

    return 0;
}
```

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s="hello";

    for(string::reverse_iterator iter=s.rbegin();iter<s.rend();iter++)
    {
        cout<<*iter;//olleh
    }

    return 0;
}
```

const_iterator和iterator使用效果相同，只不过对string常量，必须使用const_iterator，不能用iterator。

### 运算符

string类重载了多个运算符用于对字符串进行操作，这样，相对于字符数组，string字符串用起来方便了很多。

运算符|示例|功能
-|-|-
+|S+T|将S和T连成一个新串，字符数组用strcat
=|T=S|将S赋给T，字符数组用strcpy
+=|T+=S|T=T+S
`==,<=`|`T==S`|将T和S进行比较，字符数组用strcmp
`[]`|`s[i]`|存取串中第i个元素
<<,>>|cout<<s|输入输出

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string str1("C++");
    string str2("is");
    string str3,str4;
    string str5("powerful");
    cout<<"Please input a string:"<<endl;
    cin>>str3;
    cout<<str1<<" "<<str2<<" "<<str3<<"!"<<endl;
    str4=str1+" "+str2+" "+str3+"!";
    cout<<str4<<endl;
    if(str3<str5)
        cout<<"str3<str5"<<endl;
    string str6(str5);
    str6=str1+" "+str2+" "+str6+"!";
    cout<<str6<<endl;
    return 0;
}
```

```
Please input a string:
great
C++ is great!
C++ is great!
str3<str5
C++ is powerful!
```

对字符串进行排序：

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {

    vector<string> line={"A","above","a","an","about","1234"};
    int n=line.size();
    for(int i=0;i<n-1;i++)
        for(int j=i+1;j<n;j++)
            if(line[i]>line[j])
            {
                //下面的语句等价于line[i].swap(line[j]);
                string temps;
                temps=line[i];
                line[i]=line[j];
                line[j]=temps;
            }
    for(int i=0;i<n;i++)
        cout<<line[i]<<endl;
    return 0;
}
```

```
1234
A
a
about
above
an
```
### 增删改查操作

（1）增加字符（串）操作

字符串尾部添加字符或字符串，可以使用如下几个重载的append和push_back函数，注意，push_back函数只能添加一个字符。

操作|效果
-|-
s.append(str)|将字符串str（str可以是指针，也可以是string对象）附加到s的末尾
`s.append(const char*str,int n)`|将字符串str（str只能是字符数组或字符串指针，不能是string对象）的最前面n个字符附加到s的末尾
s.append(begin,end)|将迭代器`[begin,end)`范围的字符序列附加到s的末尾
s.append(str,index,n)|将字符串str（str可以是指针，也可以是string对象）从下标index开始的n个字符附加到s的末尾，如果n为string::npos，表示从下标index开始的所有字符都被附加到s的末尾
s.push_back(ch)|将字符ch附加到s的尾部，这里的实参只能是字符

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s;
    string str("abcdefg");
    s.append(str,1,2);//等价于s.append(str.begin()+1,str.begin()+3);
    cout<<s<<endl;
    s.append(str,5,string::npos);
    cout<<s<<endl;
    s.append("12345",1,3);
    cout<<s<<endl;
    return 0;
}
```

```
bc
bcfg
bcfg234
```

（2）插入字符（串）操作

插入字符到字符串中，可使用如下几个重载的insert函数，可以插入到指定的下标处，或者指定的迭代器位置处。

操作|效果
-|-
s.insert(index,str)|将字符串str（str可以是指针，也可以是string对象）插到s的下标index处
s.insert(index,str,p,n)|将字符串str（str可以是指针，也可以是string对象）从下标p开始的n个字符插入到插到s的下标index处
s.insert(pos,ch)|在s的迭代器pos所指的位置插入一个字符ch
s.insert(pos,start,end)|在s的迭代器pos所指的位置插入迭代器`[start,end)`范围内的字符

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s0="01234";
    s0.insert(s0.begin()+1,'h');
    cout<<s0<<endl;//0h1234
    string s="01234";
    string s1("abcdefg");
    char *s2="ABCDEFG";
    s.insert(1,s1,2,3);
    cout<<s<<endl;//0cde1234
    //直接修改
    s.insert(2,s2,1,2);//0cBCde1234
    cout<<s<<endl;

    string str="01234";
    str.insert(str.begin()+1,s1.begin()+2,s1.end()-2);
    cout<<str<<endl;//0cde1234
    str.insert(str.begin()+2,s2+1,s2+4);
    cout<<str<<endl;//0cBCDde1234

    return 0;
}
```

```
0h1234
0cde1234
0cBCde1234
0cde1234
0cBCDde1234
```

（3）删除字符（串）操作

删除可以用如下几个重载的erase和clear函数，分别删除指定位置上的若干字符或删除所有的字符串

操作|功能
-|-
s.erase(pos)|删除pos指向的字符，返回指向下一个字符的迭代器
s.erase(start,end)|删除`[start,end)`范围内所有字符，返回一个指向被删除的最后一个字符的下一个字符的迭代器
s.erase(int index=0,int num=npos)|删除从index索引开始的num个字符，index默认值为0，num默认值为无限多个
s.clear()|删除s中所有字符，与s.erase()等价

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s="012345678";
    string::iterator iter=s.erase(s.begin()+2);//重新赋值iter，防止指错位置
    cout<<s<<endl;//01345678
    cout<<*iter<<endl;//3

    string s1="012345678";
    string::iterator iter1=s1.erase(s1.begin()+2);
    cout<<s1<<endl;//01345678
    cout<<*iter1<<endl;//3

    string s2="012345678";
    s2.erase(2,3);//015678
    cout<<s2<<endl;

    return 0;
}
```

```
01345678
3
01345678
3
015678
```

（4）替换字符（串）操作

要替换字符串中部分字符，可以用如下几个重载的replace函数，替换指定位置上的若干字符。

操作|功能
-|-
s.replace(index,num,str)|将s字符串从下标index开始的num字符替换为str
s.replace(index1,num1,str,index2,num2)|将s字符串从下标index1开始的num1字符替换为str中从下标index2开始的num2个字符
s.replace(start,end,str)|将s字符串的迭代器`[start,end)`替换为str

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s="01234";
    string s1="abcd";
    s.replace(1,3,s1);
    cout<<s<<endl;//0abcd4
    s="01234";
    char *s2="ABCD";
    s.replace(1,3,s2);
    cout<<s<<endl;//0ABCD4


    s="01234";
    s1="abcdef";
    s.replace(1,3,s1,1,2);
    cout<<s<<endl;//0bc4
    s="01234";
    char *s3="ABCDEF";
    s.replace(1,3,s3,2,3);
    cout<<s<<endl;//0CDE4

    s="01234";
    s1="abcd";
    s.replace(s.begin()+1,s.begin()+3,s1);
    cout<<s<<endl;//0abcd34
    s="01234";
    char *s4="ABCD";
    s.replace(s.begin()+1,s.begin()+3,s4);
    cout<<s<<endl;//0ABCD34

    return 0;
}
```

```
0abcd4
0ABCD4
0bc4
0CDE4
0abcd34
0ABCD34
```

（5）查找字符（串）操作

string提供了如下几个较常用的在字符串中搜索匹配的字符串或字符的函数，可以从前面往后面查找，也可以从后面往前面查找。

操作|效果
-|-
`s.find(char* str,int pos=0);` `s.find(char c,int pos=0);`|从s字符串的下标pos位置（默认为0）开始查找子串str或查找字符c，返回找到的下标位置，如果返回-1表示未找到
`s.rfind(char* str,int pos=npos);` `s.rfind(char c,int pos=npos);`|从s字符串的下标pos位置（默认为最后一个字符）开始往前查找子串str或查找字符c，返回找到的下标位置，如果返回-1表示未找到
`s.find_first_of(char* str,int pos=0);`|从s字符串的下标pos位置（默认为0）开始查找第一个属于子串str的字符，返回找到的下标位置，如果返回-1表示未找到
`s.find_first_not_of(char* str,int pos=0);`|从s字符串的下标pos位置（默认为0）开始查找第一个不属于子串str的字符，返回找到的下标位置，如果返回-1表示未找到
`s.find_last_of(char* str,int pos=npos);`|从s字符串的下标pos位置（默认为最后一个字符）开始往前查找第一个属于子串str的字符，返回找到的下标位置，如果返回-1表示未找到

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s("dog bird chicken bird cat");
    cout<<s.find("bird",5)<<endl;//17
    cout<<s.find('i',6)<<endl;//11
    cout<<s.find('o',1)<<endl;//1
    cout<<s[0]<<endl;//d

    cout<<s.rfind("bird")<<endl;//17
    cout<<s.rfind("bird",10)<<endl;//4
    cout<<s.rfind('i')<<endl;//18

    cout<<s.find_first_of("chicken")<<endl;//字符i的位置为5
    cout<<s.find_first_of("cat",10)<<endl;//10之后首个字符c的位置为12

    return 0;
}
```

```
17
11
1
d
17
4
18
5
12
```

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {

    string s("dog bird chicken bird cat");

    cout<<s.find_first_of("chicken")<<endl;//5
    cout<<s.find_first_of("cat",10)<<endl;//12

    cout<<s.find_first_not_of("hellodog2006")<<endl;//3
    cout<<s.find_first_not_of("dog bird 2006")<<endl;//9

    cout<<s.find_last_of("13r98")<<endl;//找到最后一个r的位置是19
    cout<<s.find_last_not_of("teac")<<endl;//最后两个单词间的空格位置为21

    return 0;
}
```

```
5
12
3
9
19
21
```
### 其它常用操作

操作|效果
-|-
s.size()、s.length()|返回s中字符的个数 
s.empty()|若s为空串，则返回true,否则返回false。判断是否为空也可以使用`s.size()==0`
s.compare()|按字典比较字符串，如`s.compare("abcd");`
s.assign()|字符串赋值，如`s.assign("feeling");`
s.substr(int pos=0,int n=npos)|求子串，返回s中从pos（默认为0）开始的n个字符（默认到字符串结束）构成的字符串对象
s1.swap(s2)|互换字符串内容
s.c_str()|将s字符串转换为c语言的字符数组并以`'\0'`结尾，注意一定要有strcpy()函数等来操作方法c_str()返回的指针

```cpp
#include <iostream>
#include <string>
#include <cstring>
using namespace std;

int main() {

    string s("0123456789");
    string s1=s.substr();
    cout<<s1<<endl;//0123456789
    s1=s.substr(3);
    cout<<s1<<endl;//3456789
    s1=s.substr(3,5);
    cout<<s1<<endl;//34567



    char c[5];
    string s2="1234";
    strcpy(c,s2.c_str());
    for(int i=0;i<5;i++)
        cout<<c[i]<< " ";//1 2 3 4
    return 0;
}
```












