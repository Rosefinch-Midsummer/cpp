# string类程序设计

<!-- toc -->

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












