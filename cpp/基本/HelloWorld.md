```c++
#include <iostream> //io:input,output  stream:流   包含了输出输入语句的函数库
using namespace std; 
int main()
{
  int a = 1234;
  cout<<"Hello"<<"world"<<endl;
  cout << "HelloWorld" <<endl;
  system("pause");
  return 0;
}
```



## 在使用<iostream>等新格式时，须包括```using namespace std;```

.h在c++可以用
stdio.h也可以用
但是被淘汰了
可以用<cstdio>

## cout(插入运算符):将···插入到输出流中

endl(end line):换行 \n
endl 与 \n
endl:   换行，并清空缓冲区
\n: 只是换行




