---
layout: post
title:  C++11 中std::function和std::bind的用法
category: C＋＋
tags: C++11
keywords: 
description: 
---

[CSDN博客地址](http://blog.csdn.net/liukang325/article/details/53668046)

**关于std::function 的用法：** 

其实就可以理解成函数指针

1. 保存自由函数

```
void printA(int a)
{
    cout<<a<<endl;
}

std::function<void(int a)> func;
func = printA;
func(2);
```

2. 保存lambda表达式

```
std::function<void()> func_1 = [](){cout<<"hello world"<<endl;};
func_1();

```

3. 保存成员函数

```
struct Foo {
    Foo(int num) : num_(num) {}
    void print_add(int i) const { cout << num_+i << '\n'; }
    int num_;
};

// 保存成员函数
std::function<void(const Foo&, int)> f_add_display = &Foo::print_add;
Foo foo(2);
f_add_display(foo, 1);
```

在实际使用中都用 **auto** 关键字来代替std::function... 这一长串了。

----------

**关于std::bind 的用法：** 

看一系列的文字，不如看一段代码理解的快

```
#include <iostream>
using namespace std;
class A
{
public:
    void fun_3(int k,int m)
    {
        cout<<k<<" "<<m<<endl;
    }
};

void fun(int x,int y,int z)
{
    cout<<x<<"  "<<y<<"  "<<z<<endl;
}

void fun_2(int &a,int &b)
{
    a++;
    b++;
    cout<<a<<"  "<<b<<endl;
}

int main(int argc, const char * argv[])
{
    auto f1 = std::bind(fun,1,2,3); //表示绑定函数 fun 的第一，二，三个参数值为： 1 2 3
    f1(); //print:1  2  3
    
    auto f2 = std::bind(fun, placeholders::_1,placeholders::_2,3);
    //表示绑定函数 fun 的第三个参数为 3，而fun 的第一，二个参数分别有调用 f2 的第一，二个参数指定
    f2(1,2);//print:1  2  3
    
    auto f3 = std::bind(fun,placeholders::_2,placeholders::_1,3);
    //表示绑定函数 fun 的第三个参数为 3，而fun 的第一，二个参数分别有调用 f3 的第二，一个参数指定
    //注意： f2  和  f3 的区别。
    f3(1,2);//print:2  1  3
    
    
    int n = 2;
    int m = 3;
    
    auto f4 = std::bind(fun_2, n,placeholders::_1);
    f4(m); //print:3  4

    cout<<m<<endl;//print:4  说明：bind对于不事先绑定的参数，通过std::placeholders传递的参数是通过引用传递的
    cout<<n<<endl;//print:2  说明：bind对于预先绑定的函数参数是通过值传递的
    
    
    A a;
    auto f5 = std::bind(&A::fun_3, a,placeholders::_1,placeholders::_2);
    f5(10,20);//print:10 20
    
    std::function<void(int,int)> fc = std::bind(&A::fun_3, a,std::placeholders::_1,std::placeholders::_2);
    fc(10,20);//print:10 20
    
    return 0;
}
```
