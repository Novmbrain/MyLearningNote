# C++学习记录

## #include

`#include`叫做文件包含命令，用来引入对应的头文件（`.h`文件）。#include 也是C语言预处理命令的一种。

\#include 的用法有两种，如下所示：

```
#include <stdHeader.h>
#include "myHeader.h"
```

- 习惯是使用尖括号来引入标准头文件，使用双引号来引入自定义头文件（自己编写的头文件），这样一眼就能看出头文件的区别。

- 一个 #include 命令只能包含一个头文件，多个头文件需要多个 #include 命令。

## .h文件

## 构造函数

如果创建一个类你没有写任何构造函数,则系统会自动生成默认的无参构造函数，函数为空，什么都不做
    只要你写了一个下面的某一种构造函数，系统就不会再自动生成这样一个默认的构造函数，如果希望有一个这样的无参构造函数，则需要自己显示地写出来



### 封装性

- protected成员可以被派生类对象访问
- public，用户代码（类外）可以访问public成员而不能访问private成员；
- private成员只能由类成员（类内）和友元访问。

## 两种实例化方法

```cpp
A a;  // a存在栈上
A* a = new a();  // a存在堆中
```

- 前者在栈中分配内存，后者在堆中分配内存
- 动态内存分配会使对象的可控性增强
- 大程序用new，小程序不加new，直接申请
- new必须delete删除，不用new系统会自动回收内存

### new创建类对象例子：

```cpp
CTest* pTest = new CTest();
delete pTest;
```

pTest用来接收类对象指针。

new申请的对象，则只有调用到delete时再会执行析构函数，如果程序退出而没有执行delete则会造成内存泄漏。

### 不用new，直接使用类定义声明：

```
CTest mTest;
```

此种创建方式，使用完后不需要手动释放，该类析构函数会自动执行。

## Java指针和应用

- 引用只能在定义时初始化一次，之后不能改变指向其它变量（从一而终）；指针变量的值可变。
- 引用必须指向有效的变量，指针可以为空。
- 相对而言，引用比指针更安全。

## C++函数传递的三种方式

在C++中，参数传递的方式是“实虚结合”。

- 按值传递(pass by value)
- 地址传递(pass by pointer)
- 引用传递(pass by reference)

### 按值传递

按值传递的过程为：首先计算出实参表达式的值，接着给对应的形参变量分配一个存储空间，该空间的大小等于该形参类型的，然后把以求出的实参表达式的值一一存入到形参变量分配的存储空间中，成为形参变量的初值，供被调用函数执行时使用。这种传递是把实参表达式的值传送给对应的形参变量，故称这种传递方式为“按值传递”。

传递的是一个对象的话， 会在被调用函数的堆内复制并创建一个新的对象

使用这种方式，调用函数本身不对实参进行操作，也就是说，即使形参的值在函数中发生了变化，实参的值也完全不会受到影响，仍为调用前的值。

### 地址传递

如果在函数定义时将形参说明成指针，对这样的函数进行调用时就需要指定地址值形式的实参。这时的参数传递方式就是地址传递方式。

地址传递与按值传递的不同在于，它把实参的存储地址传送给对应的形参，从而使得形参指针和实参指针指向同一个地址。因此，被调用函数中对形参指针所指向的地址中内容的任何改变都会影响到实参。

### 引用传递

以引用为参数，则既可以使得对形参的任何操作都能改变相应的数据，又使得函数调用显得方便、自然。引用传递方式是在函数定义时在形参前面加上引用运算符“&”。

```c++

#include <iostream>
 
using namespace std;
 
void swap(int&, int&);
int main(){
	int a = 3, b = 4;
	cout << "a=" << a << ", b=" << b << endl;
	swap(a, b);
	cout << "a=" << a << ", b=" << b << endl;
	system("pause");
	return 0;
}
 
void swap(int &x, int &y){
	int t = x;
	x = y;
	y = t;

```

当函数的参数是按引用传递的时候，相比于使用指针按地址传递的好处是可以避免空指针问题，或者可以不需要进行空指针检验。因为引用一定要指向一个确定的，已经分配的空间。

## &&与右值引用

### 左值与右值

**左值(lvalue, left value)**，顾名思义就是赋值符号左边的值。准确来说， 左值是表达式（不一定是赋值表达式）后依然存在的持久对象。它是一个变量，存在于内存中，它的值可以被改变，**可以被取地址**。

**右值(rvalue, right value)**，右边的值，是指表达式结束后就不再存在的临时对象。rvalue则是一个临时变量，不存在于内存中，存在于CPU的寄存器或者指令的立即数中(immediate number)，因此我们不能改变它的值，**不能取地址**。它们通常是一个直接的数值，运算符返回的数值，或是函数的返回值，或者通过隐式类型转换得到的对象，大部分字面值(e.g., 10 and 5.3)也是rvalues。

区分左值和右值是很重要的，这是使用C++11 move语义的基础。

```cpp
lvalue = rvalue;
```

**纯右值(prvalue, pure rvalue)**，纯粹的右值，没有标识符、不可以取地址的表达式， 要么是纯粹的字面量，例如 10, true； 要么是求值结果相当于字面量或匿名临时对象，例如 1+2。非引用返回的临时变量、运算表达式产生的临时变量、 原始字面量、Lambda 表达式都属于纯右值。

**将亡值(xvalue, expiring value)**，是 C++11 为了引入右值引用而提出的概念（因此在传统 C++中， 纯右值和右值是同一个概念），也就是即将被销毁、却能够被移动的值。

```
std::vector<int> foo() {
    std::vector<int> temp = {1, 2, 3, 4};
    return temp;
}

std::vector<int> v = foo();
```

就传统的理解而言，函数 `foo` 的返回值 `temp` 在内部创建然后被赋值给 `v`， 然而 `v` 获得这个对象时，会将整个 temp 拷贝一份，然后把 `temp` 销毁，如果这个 `temp` 非常大， 这将造成大量额外的开销（这也就是传统 C++ 一直被诟病的问题）。在最后一行中，`v` 是左值、 `foo()` 返回的值就是右值（也是纯右值）。但是，`v` 可以被别的变量捕获到， 而 `foo()` 产生的那个返回值作为一个临时值，一旦被 `v` 复制后，将立即被销毁，无法获取、也不能修改。 而将亡值就定义了这样一种行为：临时的值能够被识别、同时又能够被移动。

#### 例1

下面这个这个f2需要的是一个int型的reference。reference一定要指向一个确定的地址空间，但是在调用的时候传入的是一个左值，这是不被允许的

```c++
int f2(int& number){
    return number + 1;
}

int main(int argc, char* argv[]) {
    f2(2); // No matching function for call to 'f2'
    return 0;
}
```



```
#include <iostream>
#include <string>
#include "Point.h"

int f1(int number){
    return number + 1;
}

int f2(int&& number){
    return number + 1;
}

int f3(const int& number){
    return number + 1;
}

int g1(int number){
    return number * 2;
}

int g2(int&& number){
    return number * 2;
}

int g3(const int& number){
    return number * 2;
}

int main(int argc, char* argv[]) {

    int k = 10;

    f1(k);
    f2(2);
    f3(k);

    g1(f1(k));
    g2(f1(k));
    cout << g3(f1(k)) << endl;

    return 0;
}
```

现在使用使用g2()的写法代替g3()

#### 例2

根据GetVertex函数的定义，返回的是一个rvalue，，不能够用一个reference接收

```
Point Polygon::GetVertex(int iVertex) const {
    return pointArray[iVertex];
}

int main(int argc, char* argv[]) {

    Polygon polygon(2);

    Point& point = polygon.GetVertex(1);
    
    return 0;
}

C:\Users\dell\CLionProjects\TP\main.cpp: In function 'int main(int, char**)':
C:\Users\dell\CLionProjects\TP\main.cpp:18:38: error: cannot bind non-const lvalue reference of type 'Point&' to an rvalue of type 'Point'
   18 |     Point & point = polygon.GetVertex(1);
      |                     ~~~~~~~~~~~~~~~~~^~~

```



### move()语义

传统的 C++ 没有区分『移动』和『拷贝』的概念，造成了大量的数据拷贝，浪费时间和空间。 右值引用的出现恰好就解决了这两个概念的混淆问题，为了结合左值引用来轻易完成**move语义**的实现

move函数所做的只是拿到一个左值或右值参数，然后都将其**返回为右值**同时不触发任何拷贝函数。

## 几种不同的构造器

```c++
struct Image {
 int width , height ;
 byte* image ;
 Image (int w , int h) : width(w) , height(h) {
 image = new byte [w*h] ;
 }
 // Constructeur par copie
 Image (const Image& I) : width(I.width) , height(I.height) {
 image = new byte [I.width * I.height] ;
 memcpy (image , I.image , I.width*I.height) ;
 }
 // Constructeur par appropriation (PRIORITAIRE / copy ctor)
 Image (Image&& I) :
 width(I.width) , height(I.height) , image(I.image) {
 I.width = I.height = 0 ;
 I.image = NULL ;
 }
} ;
```





第一种用途：“与”（AND）逻辑运算符。做条件判断时，`&&`常用来连接多个条件。

第二种用途：右值引用，这个功能自C++11起才可用。**移动语义**是C++11新增的重要功能，其重点是对右值的操作。右值可以看作程序运行中的临时结果，右值引用可以避免复制提高效率。`&&`用法示例：





## C++引用

引用很容易与指针混淆，它们之间有三个主要的不同：

- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。

试想变量名称是变量附属在内存位置中的标签，您可以把引用当成是变量附属在内存位置中的第二个标签。因此，您可以通过原始变量名称或引用来访问变量的内容。例如：

```
int i = 17;
```

我们可以为 i 声明引用变量，如下所示：

```
int&  r = i;
double& s = d;
```

在这些声明中，& 读作**引用**。因此，第一个声明可以读作 "r 是一个初始化为 i 的整型引用"，第二个声明可以读作 "s 是一个初始化为 d 的 double 型引用"。

引用本身也是一个变量，但是这个变量又仅仅是另外一个变量一个别名，它不占用内存空间，它不是指针哦！不要混淆了，仅仅是一个别名，别名，别名，重要的事情说三遍。

## 类和结构体



## auto关键词

auto的作用是类型推导，也就是让编译器根据等号右边的表达式来决定auto实际代表的类型。

用于两种情况

（1）声明变量时根据初始化表达式自动推断该变量的类型

（2）声明函数时函数返回值的占位符

## C++运算符重载

重载的运算符是带有特殊名称的函数，函数名是由关键字 operator 和其后要重载的运算符符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表。

```
Box operator+(const Box&);
```

大多数的重载运算符可被定义为普通的非成员函数或者被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数，如下所示：

```
Box operator+(const Box&, const Box&);
```

## 动态数组

在创建数组时，指定长度；在编译时给数组分配内存被称为静态联编。（不管用不用，都会占用内存）

使用new时，如果在运行阶段需要数组，则创建它；如果不需要，则不创建。还可以在程序运行时选择数组的长度。这被称为动态联编。意味着数组是在程序运行时创建的。这种数组叫做动态数组。在运行时确定数组的长度。

```javascript
    int* p = new int[10]; // new运算符返回第一个元素的地址。
    delete [] p;          // 释放整个数组，new如果带[] 则delete也需要带[]
```
