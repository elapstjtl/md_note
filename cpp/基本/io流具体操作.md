## 输出流概述

>   **C++中有三个重要的输出流**
>
>   -   `ostream`
>   -   `ofstream`
>   -   `ostringstream`
>
>   **预先定义的三个输出流对象**
>
>   -   `cout` 标准输出
>   -   `cerr` 标准错误输出，没有缓冲，发送给他的内容立即输出
>   -   `clog` 类似于`cerr`，但是有缓冲，缓冲区满时被输出

**标准输出换向**

```
ofstream fout("b.out");
streambuf* pOld=cout.rdbuf(fout.rdbuf());
//···
cout.rdbuf(pOld);1234
```

>   **构造输出流对象**
>
>   -   `ofstream`类支持磁盘文件输出
>
>   -   如果在构造函数中指定一个文件名,当构造这个文件时该文件是自动打开的
>
>       -   `ofstream myfile(filename");`
>
>   -   可以在调用默认构造函数之后使用
>
>       ```
>       open
>       ```
>
>       成员函数打开文件
>
>       -   `ofstream myfile;`声明一个静态文件输出流对象
>       -   `myfile.open(" filename")`打开文件,使流对象与文件建立联系
>           -在构造对象或用`open`打开文件时可以指定模式
>           -`ofstream myfile("filename", ios. base out | ios base:binary);`
>
>   **文件输出流成员函数的三种类型**
>
>   -   与操作符等价的成员函数
>   -   执行非格式化写操作的成员函数
>   -   其它修改流状态且不同于操作符或插入运算符的成员函数
>
>   **文件输出流成员函数**
>
>   -   ```
>       open
>       ```
>
>       函数
>
>       -   把流与一个特定的磁盘文件关联起来
>       -   需要指定打开模式。
>
>   -   ```
>       put
>       ```
>
>       函数
>
>       -   把一个字符写到输出流中
>
>   -   ```
>       writer
>       ```
>
>       函数
>
>       -   将内存中的一块内容写到一个文件输出流中
>
>   -   ```
>       seekp
>       ```
>
>       和
>
>        
>
>       ```
>       tellp
>       ```
>
>       函数
>
>       -   操作文件流的内部指针
>
>   -   ```
>       close
>       ```
>
>       函数
>
>       -   关闭与个文件输出流关联的磁盘文件
>
>   -   错误处理函数
>
>       -   在写到一个流时进行错误处理

------

## 向文本文件输出

>   **插入(<<)运算符**
>
>   -   为所有标准`c++`数据类型预先设计的，用于传送字节到一个输出流对象
>
>   **操纵符（`manipulator`）**
>
>   -   插入运算符与操纵符一起工作
>       -   输出格式控制
>   -   很多操纵符都定义在
>       -   `ios_base`类中（如`hex()`）、`<iomanip>`头文件（如`setprecision()`）
>   -   控制输出宽度
>       -   在流中放入`setw`操纵符或调用width成员函数为每个项指定输出宽度
>   -   `setw`和`width`仅影响紧随其后的输出项，但其它流格式操纵符是持久的，保持有效直到发生改变
>   -   `dec`、`oct`和`hex`操纵符设置输入和输出的默认进制

例子：使用`wisth`控制输出宽度

```
#include <iostream>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    for (int i = 0; i < 4; i++) {
        cout.width(10);
        cout << values[i] << endl;
    }
    return 0;
}123456789101112
```

程序运行结果：没有指定对齐方式采用默认对齐方式输出

```
      1.23
     35.36
     653.7
   4358.24
请按任意键继续. . .12345
```

同样的例子用操纵符实现：注意包含相应头文件

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    string names[] = { "Zoot","Jimmy","AI","Stan" };
    for (int i = 0; i < 4; i++) {
        cout << setw(6)<<names[i] <<setw(10)<<values[i]<< endl;
    }
    return 0;
}1234567891011121314
```

程序运行结果：

```
  Zoot      1.23
 Jimmy     35.36
    AI     653.7
  Stan   4358.24
请按任意键继续. . .
123456
```

控制对齐方式

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    string names[] = { "Zoot","Jimmy","AI","Stan" };
    for (int i = 0; i < 4; i++) {
        cout << setiosflags(ios_base::left)
            <<setw(6)
            <<names[i] 
            <<resetiosflags(ios_base::left)
            <<setw(10)
            <<values[i]<< endl;
    }
    return 0;
}12345678910111213141516171819
```

程序运行结果：

```
Zoot        1.23
Jimmy       35.36
AI          653.7
Stan        4358.24
请按任意键继续. . .12345
```

>   `setiosflags`操纵符
>
>   -   这个程序中，通过使用带参数的`setiosflags`操纵符来设置左对齐，`setiosflags`定义在头文件`iomanip`中
>   -   参数`ios_base::left`是`ios_base`静态常量，因此引用时必须包括`ios_base::`前缀。
>   -   这里需要用`resetiosflags`操纵符关闭左对齐标志。`setiosflags`不同于`width`和`setw`，它的影响是持久的，直到`resetiosflags`重新恢复默认为止
>   -   `setiosflags`的参数是该流的格式标志值，可用按位或(`|`)运算符进行组合
>       ![这里写图片描述](https://img-blog.csdn.net/20180415170812915?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvejk2MTk2ODU0OQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
>   **精度**
>
>   -   浮点数输出精度默认值是`6`，例如：`3466.98`
>   -   要改变精度：`setprecision`操作符（定义在`iomanip`中）
>   -   如果不指定`fixed`或`scientific`，精度值表示有效数字位数
>   -   如果设置了`ios_base::fixed`或`ios_base::scientific`精度值表示小数点之后的位数

**控制输出精度例子（不设置fixed或scientific）**

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    string names[] = { "Zoot","Jimmy","AI","Stan" };
    for (int i = 0; i < 4; i++) {
        cout << setiosflags(ios_base::left)
            << setw(6)
            << names[i]
            <<resetiosflags(ios_base::right)
            <<setw(10)
            <<setprecision(1)<<values[i]<<endl;

    }
    return 0;
}1234567891011121314151617181920
```

程序运行效果：

```
Zoot  1
Jimmy 4e+01
AI    7e+02
Stan  4e+03
请按任意键继续. . .12345
```

**控制输出精度例子（设置fixed，setprecision表示小数点后几位）**

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    string names[] = { "Zoot","Jimmy","AI","Stan" };
    cout << setiosflags(ios_base::fixed);    //设置
    for (int i = 0; i < 4; i++) {
        cout << setiosflags(ios_base::left)
            << setw(6)
            << names[i]
            <<resetiosflags(ios_base::right)
            <<setw(10)
            <<setprecision(1)<<values[i]<<endl;  //表示小数点后的位数

    }
    return 0;
}123456789101112131415161718192021
```

程序运行结果：

```
Zoot  1.2
Jimmy 35.4
AI    653.7
Stan  4358.2
请按任意键继续. . .12345
```

**控制输出精度例子（设置scientific科学计数法，setprecision表示小数点后几位）**

```
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main(void)
{
    double values[] = { 1.23,35.36,653.7,4358.24 };
    string names[] = { "Zoot","Jimmy","AI","Stan" };
    cout << setiosflags(ios_base::scientific);     //科学计数法
    for (int i = 0; i < 4; i++) {
        cout << setiosflags(ios_base::left)
            << setw(6)
            << names[i]
            <<resetiosflags(ios_base::right)
            <<setw(10)
            <<setprecision(1)<<values[i]<<endl;

    }
    return 0;
}123456789101112131415161718192021
Zoot  1.2e+00
Jimmy 3.5e+01
AI    6.5e+02
Stan  4.4e+03
请按任意键继续. . .12345
```

------

## 向二进制文件输出

>   **二进制文件流**
>
>   -   使用`ofstream`构造函数中的模式参量指定二进制文件输出模式
>   -   以通常方式构造一个流，然后使用`setmode`成员函数，在文件打开后改变模式
>   -   通过二进制文件输出流对象完成输出

**二进制文件输出例子**

```
#include <iostream>
#include <iomanip>
#include <string>
#include <fstream>
using namespace std;

struct Date {
    int mon, day, year;
};
int main(void)
{
    Date dt = {6,10,92};
    ofstream file("date.dat", ios_base::binary);
    file.write(reinterpret_cast<char *>(&dt),sizeof(dt));
    file.close();
    return 0;
}1234567891011121314151617
```

在工程目录下可以查看到这个二进制文件

------

## 向字符串输出文件

**典型应用：**将数据输出到字符串可以将数据转化为字符串类型

>   **字符串输出流**
>
>   -   用于构造输出流（`ostringstream`）
>   -   功能
>       -   支持`ofstream`类的除`open`、`close`外的所有操作
>       -   `str`函数可以返回当前已构造的字符串
>   -   典型应用
>       -   将数值转化为字符串

**将数值转化为字符串例子**

```
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

template <class T>
inline string toString(const T &v) {
    ostringstream os;   //创建字符串流
    os << v;            //将变量v写入字符串流
    return os.str();    //返回输出的流生成的字符串
}

int main(void)
{
    string s1 = toString(5);
    cout << s1 << endl;
    string s2 = toString(1.2);
    cout << s2 << endl;
    return 0;
}1234567891011121314151617181920
```

代码运行效果：

```
5
1.2
请按任意键继续. . .123
```

------

## 输入流类

>   **重要的输入流类**
>
>   -   `istream`类最适合用于顺序文本模式输入，cin是它的实例
>   -   `ifstream`类支持磁盘文件输入
>   -   `istringstream`
>
>   **构造输入流对象**
>
>   -   如果在构造函数中指定一个文件名，在构造该对象时该文件自动打开。
>       `ifstream myFile("filename");`
>   -   在调用默认构造函数之后用open函数打开文件。
>       `ifstream myFile;` //建立一个文件流对象
>       `myFile.open("filename");`//打开文件“filename”
>   -   打开文件模式可以指定
>       `ifstream myFile("filename",ios_base::in|ios_base::binary);`
>
>   **使用提取运算符从文本文件输入**
>
>   -   提取运算符`<<`对于所有标准`C++`数据类型都是预先设计好的
>   -   是从一个输入流对象获取字节最容易的方法
>   -   `ios`类中的很多操纵符都可以应用到输入流。但是只有少数几个对输入流对象具有实际影响，其中最重要的是进制操纵符`dec`、`oct`和`hex`
>
>   **输入流相关函数**
>
>   -   `open`函数把该流与一个特定磁盘文件相关联
>   -   `get`函数的功能与提取运算符(`>>`)很相像,主要的不同点是`get`函数在读入数据时包括空白字符。
>   -   `getline`的功能是从输入流中读取多个字符,并且允许指定输入终止字符读取完成后,从读取的内容中删除终止字符。
>   -   `read`成员函数从一个文件读字节到一个指定的内存区域,由长度魯数确定要读的字节数。当遇到文件结束或者在文本模式文件中遇到文件结東标记字符时结東束读取
>   -   `seekg`函数用来设置文件输入流中读取数据位置的指针
>   -   `tellg`函数返回当前文件读指针的位置
>   -   `close`函数关闭与一个文件输入流关联的磁盘文件

**get函数应用举例**

```
#include <iostream>
using namespace std;
int main() {
    char ch;
    while ((ch = cin.get()) != EOF)
        cout.put(ch);
    return 0;
}12345678
```

------

**为输入流指定一个终止字符**

```
#include <iostream>
#include <string>
using namespace std;
int main() {
    string line;
    cout << "Type a line terminated by 't'" << endl;
    getline(cin, line, 't');
    cout << line << endl;
    return 0;
}12345678910
```

代码运行结果：

```
Type a line terminated by 't'
qweteeeee
qwe
请按任意键继续. . .1234
```

------

**从文件读一个二进制记录到一个结构中**

```
#include <fstream>
#include <cstring>
#include <iostream>
using namespace std;
struct SalaryInfo {
    unsigned id;
    double salary;
};
int main() {
    SalaryInfo employee1 = {600001,8000};
    ofstream os("payroll", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(&employee1),sizeof(employee1));
    os.close();
    ifstream is("payroll", ios_base::out | ios_base::binary);
    if (is) {
        SalaryInfo employee2;
        is.read(reinterpret_cast<char *>(&employee2), sizeof(employee2));
        cout << employee2.id << employee2.salary << endl;
    }
    else {
        cout << "ERROR:Can not open file" << endl;
    }
    is.close();
    return 0;
}12345678910111213141516171819202122232425
```

程序运算结果：

```
6000018000
请按任意键继续. . .12
```

------

**函数位置指针**

```
#include <fstream>
#include <cstring>
#include <iostream>
using namespace std;
struct SalaryInfo {
    unsigned id;
    double salary;
};
int main() {
    int values[] = {3,7,0,5,4};
    ofstream os("payroll", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(&values),sizeof(values));
    os.close();
    ifstream is("payroll", ios_base::out | ios_base::binary);
    if (is) {
        int v;
        is.seekg(3 * sizeof(int));
        is.read(reinterpret_cast<char *>(&v), sizeof(int));
        cout << "the 4th is"<<v<< endl;
    }
    else {
        cout << "ERROR:Can not open file" << endl;
    }
    is.close();
    return 0;
}1234567891011121314151617181920212223242526
```

代码运行结果：

```
the 4th is 5
请按任意键继续. . .12
```

------

## 字符串输入流

>   **字符串输入流（`istringstream`）**
>
>   -   用于从字符串读取数据
>   -   在构造函数中设置要读取的字符串
>   -   功能
>       -   支持`ifstream`类的除`open`、`close`外的所有操作
>   -   典型应用
>       -   将字符串转化为数值

------

**从字符串输入**

```
#include <fstream>
#include <cstring>
#include <iostream>
#include <sstream>
using namespace std;
template <class T>
inline  T fromString(const string& str) {
    istringstream is(str);  //创建字符串输入流
    T v;
    is >> v;  //从字符串输入流中读取变量v
    return v; //返回变量v
}

int main(){
    int v1 = fromString<int>("5");
    cout << v1 << endl;
    double v2 = fromString<double>("1.2");
    cout << v2 << endl;
    return 0;
}1234567891011121314151617181920
5
1.2
请按任意键继续. . .123
```

------

## 输入/输出流

>   **两个重要的输入输出流**
>
>   -   一个`iostream`对象可以是数据的源或目的
>   -   两个重要的`I/O`流类都是从`iostream`派生的，他们是`fstream`和`stringstrea`m。这些类继承了前面描述的`istream`和`ostream`类的功能
>
>   **`fstream`类**
>
>   -   `fstream`类支持磁盘文件输入或输出
>   -   如果需要在同一程序中从一个特定磁盘文件读并写到该磁盘文件，可以构造一个`fstream`对象
>   -   一个`fstream`对象是有两个逻辑子流的单个流，两个子流一个用于输入，另一个用于输出
>
>   **`stringstream`类**
>
>   `stringstream`类支持面向字符串的输入输出可以用与对同一个字符串的内容交替读写，同样是由两个逻辑子流构成