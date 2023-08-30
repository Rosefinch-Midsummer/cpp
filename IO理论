# 输入输出和文件

<!-- toc -->

## 输入输出流介绍

C++把输入输出看成是一个数据流。在C++中，输入输出不是语言所定义的部分，而是由标准库提供，所以需要包含相应头文件。

C++的输入输出分为：

- 基于控制台的IO
- 基于字符串的IO
- 基于文件的IO

例子：ios类

ios类是所有i/o流类的基础类，描述了流的基本性质。

iostream对istream和ostream进行了多重派生，因而它既继承了读取流操作又继承了写入流操作。

ifstream和ofstream分别用于文件的输入与输出，派生于iostream的fstream用于控制文件流的输入输出。

I/O类库中的常用流类

类名|作用|在哪个头文件中声明
-|-|-
ios|抽象基类|iostream
istream、ostream、iostream|通用输入流和其他输入流的基类、输出、输入输出|iostream
ifstream、ofstream、fstream|输入文件流类、输出文件流类、输入输出文件流类|fstream
stringstream、istringstream、ostringstream|字符串流|sstream


## 字符串流

stringstream最大的特点是可以很方便地实现几种数据类型的转换。

**从字符串流读入**

实例1：istringstream和stringstream可以和一个字符串绑定，并作为输入源。

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    int a;
    double d;
    char s[10];
    string str("123 54.525 abcdefg");
    stringstream ss(str);
    ss>>a>>d>>s;
    cout<<"a:"<<a<<endl<<"d:"<<d<<endl<<"s:"<<s<<endl;
}
```

```
a:123
d:54.525
s:abcdefg
```

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    int a;
    double d;
    char s[10];
    string str("123 54.525 abcdefg");
    stringstream ss;
    ss<<str;
    ss>>a>>d>>s;
    cout<<"a:"<<a<<endl<<"d:"<<d<<endl<<"s:"<<s<<endl;
}
```

```
a:123
d:54.525
s:abcdefg
```

实例2：输入一个未知长度的数组及一个数num，从数组中将num删除

输入方式：共输入两行，第一行包含若干个数字，表示数组的各个元素；第二行包含一个数num。

分析：数组长度未知，这里不能用for循环输入数据，数组的后面还需要继续输入，所以也不能用`while(cin>>a[i++])`（Ctrl+Z终止所有输入）

解决方案：先将第一行读入一个字符串，然后用stringstream从字符串里读到数组中。由于字符串内容有限，所以可以用`while(ss>>a[i++])`输入数据

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    int a[100],i=0,n=0,num;
    string str;
    getline(cin,str);
    stringstream ss;
    ss<<str;
    while(ss>>a[i++])
        n++;
    cin>>num;
    cout<<"The size of array is: "<<n<<endl;
    cout<<"num:"<<num<<endl;
}
```

```
1 2 3 4 5 6 7 8 9 10
3
The size of array is: 10
num:3
```

**输出到流**

ostringstream和stringstream类可以用于执行流的输出操作，实现格式化字符串的目的

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    double d=12.0;
    stringstream ss;
    ss<<123<<45.82<<" "<<"abcdefg"<<endl<<'*'<<d;

    string result=ss.str();
    cout<<result;
    return 0;
}
```

```
12345.82 abcdefg
*12
```

**int和string类型的相互转换实例**

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    int n=300;
    string result;
    stringstream oss;
    oss<<n;//将n的值传入oss流
    oss>>result;//将oss流中的数据写入result变量
    //上面这句可以写成result=oss.str();
    cout<<"(int)300--to-->string(300):"<<result<<endl;


    int m;
    string int_number="12345";
    stringstream oss1;
    oss1<<int_number;//将int_number的值传入oss1流
    oss1>>m;//将oss1流中的数据写入m变量
    cout<<"string(12345)--to-->int(12345):"<<m<<endl;

    return 0;
}
```

```
(int)300--to-->string(300):300
string(12345)--to-->int(12345):12345
```

**float和string类型相互转换实例**

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

int main()
{
    float n=300.8;
    string result;
    stringstream oss;
    oss<<n;//将n的值传入oss流
    oss>>result;//将oss流中的数据写入result变量
    //上面这句可以写成result=oss.str();
    cout<<"float(300.8)--to-->string(300.8):"<<result<<endl;


    float m;
    string float_number="12345.7";
    stringstream oss1;
    oss1<<float_number;
    oss1>>m;
    cout<<"string(12345.7)--to-->float(12345.7):"<<m<<endl;

    return 0;
}
```

```
float(300.8)--to-->string(300.8):300.8
string(12345.7)--to-->float(12345.7):12345.7
```

**用模板函数实现任意类型的转换实例**

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

template<typename out_type,typename in_type>
out_type convert(const in_type& t)
{
    stringstream stream;
    stream<<t;
    out_type result;
    stream>>result;
    return result;
}
int main()
{
    float float_num=99.876;
    string double_str="87.89";
    int int_num=convert<int,char*>("102");
    string float_str=convert<string,float>(float_num);
    double double_num=convert<double,string>(double_str);
    cout<<int_num<<endl<<float_str<<endl<<double_num<<endl;
    return 0;
}
```

```
102
99.876
87.89
```

## 文件输入输出

### 文件的概念

文件能永久保存数据，文件分为二进制文件和文本文件。

文本文件也被称为ASCII文件，它将文件中的每个字节看成是一个字符的ASCII值。文本文件可以直接用记事本打开或者显示在显示器上。文本文件通常以.txt为后缀，C++的源文件也属于文本文件。

二进制文件是指含ASCII码字符之外的数据的文件，它不能由文本编辑软件打开。在实际应用中，大多数文件都是二进制文件，如图像、影像、声音、数据库文件。.doc文件也是二进制文件。

输入：其它到内存

输出：内存到其它
### 文件的打开

使用`fstream myfile;`建立一个文件流对象，然后利用fstream提供的open()成员函数打开文件与流连接。

open()函数的原型如下：

```cpp
void open(const char* filename,int nMode=ios::in)
```

文件打开（操作）模式表

模式参数|说明
-|-
ios::in|为输入打开文件，是fstream和istream的默认模式
ios::out|为输出打开文件，是ostream的默认模式
ios::ate|打开文件输出，文件指针位于文件尾。ate=at end
ios::app|从文件尾部添加数据
ios::trunc|如果文件存在，清除文件内容（打开输出文件的默认模式）
ios::binary|以二进制方式打开文件（默认模式为文本模式）

例如`myfile.open("D:\\prog\\p1_1.cpp",ios::in|ios::out);`打开文本文件p1_1.cpp用于输入输出

说明：

（1）多个打开模式之间用”|“分隔

（2）用于输入时，想要打开的文件必须已经存在，否则出错

（3）文件路径中的`\\`必须是两个因为`\`是转义字符

（4）文件路径也可以写成”D:/prog/p1_1.cpp"

当用fstream、ofstream、ifstream建立文件流对象时可直接给出文件名、操作模式等参数，这样可以省略open()函数的使用。

输出文件流的建立可以用如下办法：

```cpp
fstream ofile("D:\\prog\\p1_1.cpp",ios::out);
ofstream ofile("D:\\prog\\p1_1.cpp");
```

输入文件流的建立可以用如下办法：

```cpp
fstream ofile("D:\\prog\\p1_1.cpp",ios::in);
ifstream ofile("D:\\prog\\p1_1.cpp");
```

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    char line[180];
    fstream myfile;//创建文件流
    myfile.open("D:\\prog\\p1_1.cpp",ios::in|ios::out);
    if(!myfile)
    {
        cout<<"File open or create error!"<<endl;
        exit(1);
    }
    return 0;
}
```

```
File open or create error!

Process returned 1 (0x1)   execution time : 0.070 s
Press any key to continue.
```

删除`ios::in|`会创建一个新文件
### 文件的输出

C++默认文件模式是文本模式。当使用文本模式时，输出到文件的内容是ASCII码字符（包括回车、换行）。也就是说，文本文件只能存储ASCII码字符。如整数123在文本文件中被存储为“123”。

文本文件输出可以用插入操作符<<和成员函数write()

文件输出的一般步骤如下：

（1）建立输出流（对象），将建立的文件连接到文件流上。此步需要对文件是否建立成功进行判断，如果文件建立错误，则退出。

（2）向输出文件流输出内容。

（3）关闭文件（文件流对象消失时也会自动关闭文件）

文本文件输出实例：

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main()
{
    char name[80];
    double score;
    fstream myfile;
    myfile.open("e:\\cpp\\p1_1.cpp",ios::out);
    if(!myfile)
    {
        cout<<"File open or create error!"<<endl;
        exit(1);
    }
    while(cin>>name>>score)
        myfile<<name<<score<<endl;
    myfile.close();
    return 0;
}
```

二进制文件输出实例：

输出二进制文件的方法需要使用write()成员函数。

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main()
{
    char* name[3]={"Antony","John","Tom"};
    float score[3]={85.5,90,60};
    fstream txtfile,binfile;
    txtfile.open("e:\\cpp\\p1_1.txt",ios::out|ios::trunc);
    binfile.open("e:\\cpp\\p1_2.dat",ios::binary|ios::out|ios::trunc);
    if(!txtfile)
    {
        cout<<"Txtfile open or create error!"<<endl;
        exit(1);
    }
    if(!binfile)
    {
        cout<<"Binfile open or create error!"<<endl;
        exit(1);
    }
    for(int i=0;i<3;i++)
    {
        txtfile<<name[i]<<'\t'<<score[i]<<endl;
        binfile.write(name[i],8*sizeof(char));
        binfile.write((char*)&score[i],sizeof(float));
    }
    txtfile.close();
    binfile.close();
    return 0;
}
```

### 文件的输入

文件的输入是指从文件中读取数据到内存中，文本文件常用提取操作符>>，在文件输入中要经常检查文件是否到达尾部，输入流的成员函数eof()用来检查是否到达文件结尾。若读取到文件结尾时，返回true。

文件输入一般要经过下列三个步骤：

（1）建立输入文件流（对象），将以输出方式打开的文件连接到文件流上。此步骤需要对文件是否成功打开进行判断。如果文件打开错误，则退出。

（2）从输入文件流中读内容。此步骤需要对读文件是否成功进行判断，如果读入不成功或到达文件尾，则读入结束。

（3）关闭文件（文件流对象消失时也会自动关闭文件）。

文本文件输入实例：

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    char name[8],score[6];

    ifstream txtfile;
    txtfile.open("e:\\cpp\\p1_1.txt");

    if(!txtfile)
    {
        cerr<<"Txtfile open or create error!"<<endl;
        exit(1);
    }
    while(!txtfile.eof())
    {
        txtfile>>name>>score;
        cout<<name<<'\t'<<score<<endl;
    }
    txtfile.close();
    return 0;
}
```

```
Antony  85.5
John    90
Tom     60
Tom     60
```

实际文件中内容如下：

```
Antony  85.5
John    90
Tom     60
```

判断条件滞后产生这样的结果。

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    char name[8],score[6];

    ifstream txtfile;
    txtfile.open("e:\\cpp\\p1_1.txt");

    if(!txtfile)
    {
        cerr<<"Txtfile open error!"<<endl;
        exit(1);
    }
    while(!txtfile.eof())
    {
        if(txtfile>>name>>score)
            cout<<name<<'\t'<<score<<endl;
    }
    txtfile.close();
    return 0;
}
```

```
Antony  85.5
John    90
Tom     60
```

二进制文件输入实例：

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    char name[8];
    float score;

    ifstream binfile;
    binfile.open("e:\\cpp\\p1_2.dat",ios::binary);

    if(!binfile)
    {
        cerr<<"Binfile open error!"<<endl;
        exit(1);
    }
    while(!binfile.eof())
    {
        binfile.read((char*)(name),8*sizeof(char));
        binfile.read((char*)(&score),sizeof(float));
        cout<<name<<'\t'<<score<<endl;
    }
    binfile.close();
    return 0;
}
```

```
Antony  85.5
John    90
Tom     60
Tom     60
```

这里输出也多了一行，解决这个问题可以像前面文本文件一样添加一个if判断条件，也可以采用如下办法：

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    char name[8];
    float score;

    ifstream binfile;
    binfile.open("e:\\cpp\\p1_2.dat",ios::binary);

    if(!binfile)
    {
        cerr<<"Binfile open error!"<<endl;
        exit(1);
    }
    while(binfile.peek()!=EOF)//只适用于二进制文件
    {
        binfile.read((char*)(name),8*sizeof(char));
        binfile.read((char*)(&score),sizeof(float));
        cout<<name<<'\t'<<score<<endl;
    }
    binfile.close();
    return 0;
}
```

```
Antony  85.5
John    90
Tom     60
```

### 文本文件输入的小提醒

输出

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    ofstream fout("test");
    if(!fout)
    {
        cerr<<"Fail to open output file\n";
        return 1;
    }
    fout<<10<<" "<<123.456<<"\"This is a text file\"\n";
    fout.close();
    return 0;
}
```

文件中内容为10 123.456"This is a text file"

输入：

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    ifstream fin("test");
    char s[80];
    int i;
    float x;
    if(!fin)
    {
        cout<<"Fail to open input file\n";
        return 1;
    }
    fin>>i>>x>>s;
    cout<<i<<" "<<x<<s;
    fin.close();
    return 0;
}
```

输出结果：

```cpp
10 123.456"This
```

修改方法：

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    ifstream fin("test");
    char s[80];
    int i;
    float x;
    if(!fin)
    {
        cout<<"Fail to open input file\n";
        return 1;
    }
    fin>>i>>x>>s;
    cout<<i<<" "<<x;
    fin.getline(s,80);
    fin.close();
    return 0;
}
```




