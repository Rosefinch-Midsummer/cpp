# vector类程序设计

<!-- toc -->

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


