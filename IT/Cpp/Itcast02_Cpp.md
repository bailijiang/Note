<!-- MarkdownTOC -->

- [1. C++命名空间namespace](#1-c命名空间namespace)
- [2. 自定义namespace](#2-自定义namespace)
- [3. bool类型](#3-bool类型)
- [4. 引用&：（变量别名）可修改](#4-引用：（变量别名）可修改)
- [5. const引用\(只读\)](#5-const引用只读)
- [6. C++编译器中const定义时必须初始化,而且不能被修改](#6-c编译器中const定义时必须初始化而且不能被修改)
- [7. 内联函数inline](#7-内联函数inline)
- [8. 函数默认参数值](#8-函数默认参数值)
- [9. 函数重载](#9-函数重载)
- [10. 函数指针和函数重载](#10-函数指针和函数重载)
- [11. 计算两点之间距离（二维）](#11-计算两点之间距离（二维）)
- [12. class属性可以是其他class实例](#12-class属性可以是其他class实例)
- [13. 同类之间无priviate](#13-同类之间无priviate)
- [14. 构造函数](#14-构造函数)
- [15. 析构函数](#15-析构函数)
- [16. 函数参数和返回值](#16-函数参数和返回值)
- [17. class的3种特殊函数](#17-class的3种特殊函数)
- [18. 对象的浅拷贝和深拷贝](#18-对象的浅拷贝和深拷贝)
- [19. 构造函数参数列表](#19-构造函数参数列表)
- [20. new 和 delete](#20-new-和-delete)
- [21. static静态成员变量](#21-static静态成员变量)
- [22. static静态成员函数](#22-static静态成员函数)
- [23. 货物进货出货练习](#23-货物进货出货练习)
- [24. this指针](#24-this指针)
- [25. class成员函数](#25-class成员函数)
- [26. 数组类操作](#26-数组类操作)
- [27. 友元函数](#27-友元函数)
- [28. 操作符重载operator](#28-操作符重载operator)
- [29. size_t](#29-size_t)
- [30. void*](#30-void)
- [31. class对象的仿\(伪\)函数操作符重载\(\)](#31-class对象的仿伪函数操作符重载)
- [32. && 和 || 操作符重载](#32--和-||-操作符重载)
- [33. 智能指针 auto_ptr, `-> *` 重载](#33-智能指针-auto_ptr----重载)
- [34. 继承的本质](#34-继承的本质)
- [35. 继承方式](#35-继承方式)
- [36. 返回值 void, void* 区别](#36-返回值-void-void-区别)
- [37. 类的赋值兼容性](#37-类的赋值兼容性)
- [38. 继承中的构造和析构](#38-继承中的构造和析构)
- [39. virtual 虚继承](#39-virtual-虚继承)
- [40. 多态](#40-多态)
- [41. 析构多态](#41-析构多态)
- [42. vptr](#42-vptr)
- [43. 纯虚函数](#43-纯虚函数)
- [44. 面向对象开发流程\(分层独立开发\)](#44-面向对象开发流程分层独立开发)
- [45. 人员信息管理系统](#45-人员信息管理系统)
- [46. C语言中的函数指针](#46-c语言中的函数指针)
- [47. C语言中实现多态\(控制反转IOC\)](#47-c语言中实现多态控制反转ioc)
- [48. 模板 template \(泛型\)](#48-模板-template-泛型)
- [49. 函数模板数组排序](#49-函数模板数组排序)
- [50. 函数模板与函数重载](#50-函数模板与函数重载)
- [51. 模板类](#51-模板类)
- [52. 复数模板类](#52-复数模板类)
- [53.  模板类多文件](#53-模板类多文件)
- [54. 自定义模板数组类](#54-自定义模板数组类)
- [55. 类型转换](#55-类型转换)
- [56. 异常处理](#56-异常处理)
- [57. explicit](#57-explicit)
- [58. exception 标准库异常类](#58-exception-标准库异常类)
- [59. 自定义异常类](#59-自定义异常类)
- [60. cin.putback\(\)](#60-cinputback)
- [61. 文件流的基本操作](#61-文件流的基本操作)

<!-- /MarkdownTOC -->


<a id="1-c命名空间namespace"></a>
#### 1. C++命名空间namespace
```
#include <iostream> // 引入iostream.h时就不需要using namespace std;了
using namespace std; //相当于{}   std::cout  std::cin  std::endl
std{ cout cin endl…}
using std::cout; //单引
```
<a id="2-自定义namespace"></a>
#### 2. 自定义namespace
```
namespace A
{
	int a = 10;
}
namespace B
{
	int a = 20;
}
void test_namespace()
{
	cout << "A:a = " << A::a << endl;
	cout << "B:a = " << B::a << endl;
}
```

<a id="3-bool类型"></a>
#### 3. bool类型
* C中0为假，非0为真；
* C++中false值为0，true(非0)值为1， sizeof为1；

<a id="4-引用：（变量别名）可修改"></a>
#### 4. 引用&：（变量别名）可修改
+ 引用不是作为参数时，必须初始化，初始化后不能够被改变
引用就是常量指针（右high写:不可改方向，但可以改地址中的值）
```
    int a = 10;
    int* p = &a;
    *p = 20;
    cout << "a = " << a << endl;
    int& b = a;
    b = 40;
    cout << "a = " << a << endl;
```

+ __引用&的作用__：
    - 作为函数参数进行传递，避免值拷贝（本意是为了降低对指针的使用难度）
    - __作为函数返回值__, 不能返回局部变量的引用
    - 不可以用一个引用类型变量来接收函数局部变量的地址

<a id="5-const引用只读"></a>
#### 5. const引用(只读)
+ 如果要对一个字面量做引用，则必须使用const引用：
    const int& re = 10;
+ 将const引用作为参数时，可以避免对实参进行修改（只读）：

```
  void printX(const int& re) {
  	cout << "&re: " << re << endl;
  	//re = 6;
  }
```

+ const int & re 相当于 const int * const re

<a id="6-c编译器中const定义时必须初始化而且不能被修改"></a>
#### 6. C++编译器中const定义时必须初始化,而且不能被修改
```
int main(void)
{
	const int a = 10;  // 在常量区 符号表创建
	cout << "a: " << a << endl;	// a: 10
	int* p = (int*)&a;	// temp
	*p = 20;	// *temp = 20;
	cout << "a: " << a << endl;	// a: 10

	return 0;
}
```
* `const struct A a = {10};` 依然在栈上开辟空间, 且为只读

<a id="7-内联函数inline"></a>
#### 7. 内联函数inline
省去压栈出栈的开销，直接展开代码块
使用场景：代码少，但反复被调用的函数
```
inline void printAB(int a, int b) {
	cout << "a: " << a << ", b: " << b << endl;
}

int main(void)
{
	int a = 10;
	int b = 20;
	printAB(a, b);
	printAB(a, b);
	printAB(a, b);

	return 0;
}
```
<a id="8-函数默认参数值"></a>
#### 8. 函数默认参数值
默认参数值必须放在所有参数最后

<a id="9-函数重载"></a>
#### 9. 函数重载
函数重载， 就是对参数列表的变换，不是函数返回值的变化
返回值必须一样，参数列表不同， 并且函数名相同的函数都是重载函数

<a id="10-函数指针和函数重载"></a>
#### 10. 函数指针和函数重载
函数指针恒定指向一个函数入口，想要通过函数指针发生函数重载是不可能的

<a id="11-计算两点之间距离（二维）"></a>
#### 11. 计算两点之间距离（二维）
A(x1, y1); B(x2, y2);

$distance=\sqrt{(x2 - x1)^2 + (y2 - y1)^2}$

<a id="12-class属性可以是其他class实例"></a>
#### 12. class属性可以是其他class实例
    private:
        Point p0;
<a id="13-同类之间无priviate"></a>
#### 13. 同类之间无priviate
可以直接使用实例属性，不需要setter, getter
非同类实例，必须通过setter,getter方法访问实例属性
<a id="14-构造函数"></a>
#### 14. 构造函数
* 构造函数与class同名
* 构造函数可以重载
* 构造函数可以为private属性设置初始值
* 不显示写构造函数,会有一个默认的无参构造函数
* 显示写构造函数会覆盖掉无参默认构造函数,此时如果要使用无参构造函数,就必须再显示写一个无参构造函数
* 通过另一个对象构造(拷贝构造函数)

```
	Test(const Test& another_Test)
	{
		m_x = another_Test.m_x;
		m_y = another_Test.m_y;
	}
```

    Test t5 = t4; 也会调用拷贝构造函数

* 构造函数只会在对象被创建时调用一次
* 构造函数分为: 无参, 有参, 拷贝
* 函数参数为通过值传递的class对象时,会隐式调用拷贝构造函数
* 函数返回值为class对象时(值传递),会通过**匿名对象**接收返回值,并调用拷贝构造函数
* C++是静态编译语言,没有反射,this保留的是内存地址,class实例对象名在代码块结束时会被弹栈,因此无法cout出class实例对象名称,可以采用通过字符串保存对象名称

<a id="15-析构函数"></a>
#### 15. 析构函数
* 与class同名，加~, 无参数返回值：~Test()
* 在class对象被释放之前调用
* 可以执行一些free操作
* 如果不写，系统会自动定义一个空析构函数
* 析构顺序与构造顺序相反(LIFO后进先出)
* 无变量接收的匿名class对象会立刻调用析构函数

<a id="16-函数参数和返回值"></a>
#### 16. 函数参数和返回值
* 由于构造函数和析构函数的特性, 通常在函数传参和返回值中使用class对象的引用&或指针*

<a id="17-class的3种特殊函数"></a>
#### 17. class的3种特殊函数
* 默认构造函数(有参,无参): 如果显示的写了一个普通构造函数,会隐藏默认的无惨构造函数
* 拷贝构造函数: 如果显示的写了一个拷贝构造函数,会隐藏默认的无参构造函数和默认的拷贝构造函数
* 析构函数: 如果显示的写了一个析构函数,会隐藏默认的析构函数

<a id="18-对象的浅拷贝和深拷贝"></a>
#### 18. 对象的浅拷贝和深拷贝
* 对象属性中有指针指向堆内存空间的,就有可能出现浅拷贝风险,必须手动写一个拷贝构造函数,实现内存深拷贝

```
    Teacher(const Teacher &another)
    {
        m_id = another.m_id; //给id赋值
        int len = strlen(another.m_name);
        m_name = (char*)malloc(len + 1);
        strcpy(m_name, another.m_name);
    }
```

<a id="19-构造函数参数列表"></a>
#### 19. 构造函数参数列表
* 构造函数分2个部分: 初始化, 执行{...}
    * 执行{}过程中无法初始化对象
    * 执行{}过程中调用的时对象操作符重载
    * 在初始化一个class对象调用其构造函数的过程中, 无法直接初始化另一个对象, 必须通过参数列表的方式初始化
    * 如果在参数列表中不显示的初始化private中的其他class对象, 将隐式调用那个对象的无参构造器对其初始化
* 类内部的对象初始化的顺序，跟对象的定义顺序一样，跟初始化列表中的顺序无关
* 析构的顺序就跟构造的顺序相反
* 常量成员变量不能够赋值，只能通过初始化列表进行初始化
```
class B
{
public:
  B(int b, int m) : a1(20), a2(30), m_m(m)
  {
    m_b = b;
  }
  B(int b, int aa1, int aa2, int m) : m_b(b), a1(aa1), a2(aa2), m_m(m){}

  void printB()
  {
    cout << "m_b: " << m_b << endl;
    cout << "m_m:" << m_m << endl;
    a1.printA();
    a2.printA();
  }
  ~B()
  {
    cout << "~B() run..." << endl;
  }
private:
  int m_b;
  A a1;
  A a2;
  const int m_m;
};
```

<a id="20-new-和-delete"></a>
#### 20. new 和 delete
* C++中 new 和 delete 相当于 C 中的 malloc 和 free
* new, delete 和 malloc, free 的区别:
    - malloc和free 在对于普通变量是可以混合使用的, 但是 malloc和free 是函数, new和delete 是操作符, 不是一个函数, 没有压栈和出栈操作, new和delete 效率更高
    - new 在创建一个对象的时候，会调用对象的构造函数,delete在释放一个对象指针的时候，会调用对象析构函数
```
void test2()
{
  int* p = new int;
  *p = 10;
  cout << "p: " << *p << endl;
  if (p != NULL)
  {
    delete p;
  }

  int* array_p = new int[10];
  for (int i = 0; i < 10; i++)
  {
    array_p[i] = i + 1;
  }
  for (int i = 0; i < 10; i++)
  {
    cout << "array[" << i << "]: " << array_p[i] << endl;
  }
  if (array_p != NULL)
  {
    delete[] array_p;
  }
}
```

<a id="21-static静态成员变量"></a>
#### 21. static静态成员变量
* 在类的内部声明这个变量 用static
* 静态的成员变量是对本class共享, 所有本class中的对象, 都是共用一个内存区域
* 在__类的外部__去写静态成员变量的__初始化__, 不加 static关键字
* 不能被其他类class调用
* static修饰的成员变量并不属于对象中, 而是在全局区开辟的

<a id="22-static静态成员函数"></a>
#### 22. static静态成员函数
* 如果class的static成员变量在private中, 就需要提供static成员函数对其进行访问
* 可以把他当成一个类的全局函数

<a id="23-货物进货出货练习"></a>
#### 23. 货物进货出货练习
* 知识点: static, new, delete, 链表(无头,头部插入删除), 有/无参构造器, 析构, &引用, do while, switch/case/break
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

class Goods
{
public:
  Goods() :_weight(0), next(NULL) {}
  Goods(int weight) {
    _weight = weight;
    next = NULL;
    total_weight += _weight;
  }
  static int getTotal() {
    return total_weight;
  }
  ~Goods() {
    total_weight -= _weight;
  }
  Goods* next;
private:
  int _weight;
  static int total_weight;
};

int Goods::total_weight = 0;

void buy(Goods* &head, int weight) {
  Goods* new_goods = new Goods(weight);
  if (head == NULL) {
    head = new_goods;
  }
  else
  {
    new_goods->next = head;
    head = new_goods;
  }
}

void sale(Goods* &head) {
  if (head == NULL)
  {
    cout << "goods empty" << endl;
    return;
  }
  else
  {
    Goods* temp = NULL;
    temp = head;
    head = head->next;
    delete temp;
  }
}

int main(void)
{
  Goods* head = NULL;
  int chioce = 0;
  int weight = 0;

  do
  {
    cout << "Please select(1:buy, 2:sale, 0:exit)" << endl;
    cin >> chioce;
    switch (chioce)
    {
    case 1:
      cout << "weight: ";
      cin >> weight;
      buy(head, weight);
      break;
    case 2:
      sale(head);
      break;
    case 0:
      return 0;
      break;
    default:
      break;
    }
    cout << "Total weight: " << Goods::getTotal() << endl;

  } while (1);

  return 0;
}
```

<a id="24-this指针"></a>
#### 24. this指针
* 在调用class成员函数时, C++编译器会隐含着传递这个对象的指针, 从而区分不同对象的成员变量和成员函数
```
  Test t1(10);
  t1.getI();
  //Test t1;
  //Test_init(&t1, 10);
  //Test_getI(&t1);
```
* 哪个对象调用的, 就传那个对象的指针: `t2.getK(); // t2.getK(&t2)`
* this 时一个常量指针: Test* const this
* 如果想将this指针变成只读, 可以在成员函数后面加 const
```
  int  getK()  const   // int getK(const Test *const this) // this 是一个常指针
  {
    //this++;
    //this->m_k = 0; 在成员函数后面加const 修饰的不是函数，修饰的是隐藏的this指针
    return this->m_k;
  }
```

<a id="25-class成员函数"></a>
#### 25. class成员函数
* 返回对象引用, 可以实现连缀操作符
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

class Test
{
public:
  Test(int x, int y) {
    this->x = x;
    this->y = y;
  }
  void printT() {
    cout << "x: " << this->x << " y: "<< this->y << endl;
  }
  Test& add(Test& t) {
    this->x += t.x;
    this->y += t.y;

    return *this;
  }
private:
  int x;
  int y;
};

int main(void)
{
  Test t1(10, 20);
  Test t2(30, 40);
  Test t3(50, 60);

  t1.add(t2).add(t3);
  t1.printT();

  return 0;
}
```

<a id="26-数组类操作"></a>
#### 26. 数组类操作
操作数组数据结构, 均可参照此例子. 替换int为其他任意数据类型即可

* MyArray.h
```
#pragma once
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

class MyArray
{
public:
  MyArray();
  MyArray(int length);
  MyArray(const MyArray& another);
  ~MyArray();
  void setData(int index, int data);
  int getLen();
  int getData(int index);
private:
  int length;
  int* space;
};
```
* MyArray.cpp
```
#include "MyArray.h"

MyArray::MyArray()
{
  this->length = 0;
  this->space = NULL;
}
MyArray::MyArray(int length)
{
  if (length <= 0) {
    this->length = 0;
    return; 
  }
  else
  {
    this->length = length;
    this->space = new int[this->length];
  }
}

MyArray::~MyArray()
{
  if (this->space != NULL)
  {
    delete[] this->space;
    this->length = 0;
    this->space = NULL;
  }
}

void MyArray::setData(int index, int data)
{
  if (this->space != NULL)
  {
    this->space[index] = data;
  }
}

int MyArray::getLen()
{
  return this->length;
}

int MyArray::getData(int index)
{
  return this->space[index];
}

MyArray::MyArray(const MyArray& another)
{
  if (another.length >= 0) {
    this->length = another.length;
    this->space = new int[another.length];
    for (int i = 0; i < another.length; i++) {
      this->space[i] = another.space[i];
    }
  }
}
```
* main.cpp
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include "MyArray.h"
using namespace std;

int main(void)
{
  MyArray a1(10);
  for (int i = 0; i < a1.getLen(); i++) {
    a1.setData(i, i + 10);
  }
  for (int i = 0; i < a1.getLen(); i++) {
    cout << a1.getData(i) << endl;
  }
  cout << "-----------" << endl;
  MyArray a2 = a1;
  for (int i = 0; i < a2.getLen(); i++) {
    cout << a2.getData(i) << endl;
  }
  
  return 0;
}
```

<a id="27-友元函数"></a>
#### 27. 友元函数
* 在class类的内部通过friend声明一个全局函数后, 这个全局函数就可以使用这个class类的private私有成员
* friend 会破坏class封装性
```
  //声明一个全局函数为自己的友元
  friend double PointDistance(Point & p1, Point &p2);

  //声明一个 其他类的成员函数为自己的友元
  friend double PointManager::PointDistance(Point & p1, Point &p2); 
```
* 解决循环前置声明问题,需要将函数声明和函数定义分开写
* friend class B; //声明B类是我的友元类。  友元类就可以访问我的所有成员
* 同类之间无私处, 异类之间有友元
* 友元关系不能被继承, 无交换性, 无传递性

<a id="28-操作符重载operator"></a>
#### 28. 操作符重载operator
* ` + - * / >> << += -+ 重载`
```
class Complex
{
public:
  Complex(int a, int b) {
    this->a = a;
    this->b = b;
  }
  void print() {
    cout << "(" << a << "+" << b << "i" << ")" << endl;
  }
  friend Complex operator+(const Complex& x, const Complex& y);
  Complex operator-(const Complex& another) {
    Complex temp = Complex(this->a - another.a, this->b - another.b);
    return temp;
  }
  Complex& operator-=(Complex& another) {
    this->a -= another.a;
    this->b -= another.b;
    return *this;
  }
  const Complex operator++(int) {
    Complex temp(this->a, this->b);
    a++;
    b++;
    return temp;
  }
  friend ostream& operator<<(ostream& os, Complex& c);
private:
  int a;
  int b;
};

Complex operator+(const Complex& x, const Complex& y) {
  Complex temp = Complex(x.a + y.a, x.b + y.b);
  return temp;
}

ostream& operator<<(ostream& os, Complex& c) {
  os << c.a << "+" << c.b << "i" << endl;
  return os;
}

int main(void)
{
  Complex c1(1, 2);
  c1.print();
  Complex c2(3, 4);
  Complex c3 = c1 + c2;
  c3.print();
  Complex c4 = c3 - c1;
  c4.print();
  c4 -= c1;
  c4.print();
  Complex c5(10, 20);
  c5++;
  c5.print();
  cout << c5;
  
  return 0;
}
```
* =号重载(赋值)
```
class Student
{
public:
  Student(int id, char* name) {
    this->id = id;
    int len = strlen(name);
    this->name = (char*)malloc(len + 1);
    strcpy(this->name, name);
  }
  Student(const Student& another) {
    this->id = another.id;
    int len = strlen(another.name);
    this->name = (char*)malloc(len + 1);
    strcpy(this->name, another.name);
  }
  Student& operator=(const Student& another) {
    if (this->name != NULL) {
      delete[] this->name;
      this->name = NULL;
      this->id = 0;
    }
    this->id = another.id;
    int len = strlen(another.name);
    this->name = (char*)malloc(len + 1);
    strcpy(this->name, another.name);

    return *this;
  }
  void printS() {
    cout << "id: " << this->id << " name: " << this->name << endl;
  }
  ~Student() {
    if (this->name != NULL) {
      delete[] this->name;
      this->name = NULL;
      this->id = 0;
    }
  }
private:
  int id;
  char* name;
};

int main(void)
{
  Student s1(1, "Bryan");
  s1.printS();
  Student s2(s1);
  s2.printS();
  Student s3(2, "xiaoming");
  s3 = s1;
  s3.printS();
  
  return 0;
}
```

<a id="29-size_t"></a>
#### 29. size_t
unsigned int size_t
<a id="30-void"></a>
#### 30. void*
* 在C/C++中数据类型就是指数据长度, int[2] 和 int[3] 是两个不同的数据类型,当返回值不能确定数据类型(数据长度)时,应使用无类型指针 void*
* 使用场景: new/delete 操作符重载
* 编译器会将对象长度以 size_t size 参数传入 new 方法
```
class A
{
public:
  A() {
    cout << "run A()" << endl;
    this->a = 0;
  }
  A(int a) {
    this->a = a;
  }
  void* operator new(size_t size) {
    cout << "run new" << endl;
    return malloc(size);
  }
  void* operator new[](size_t size) {
    cout << "run new[]" << endl;
    return malloc(size);
  }
  void operator delete(void* p) {
    if (p != NULL) {
      free(p);
    }
  }
  void operator delete[](void* p) {
    if (p != NULL) {
      free(p);
    }
  }
  ~A() {
    cout << "run ~A()" << endl;
  }
private:
  int a;
};

int main(void)
{
  A* ap = new A;
  delete ap;

  A* array_p = new A[10];
  delete[] array_p; // new A[10]开辟的必须用 delete[]

  return 0;
}
```

<a id="31-class对象的仿伪函数操作符重载"></a>
#### 31. class对象的仿(伪)函数操作符重载()
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Print
{
public:
  void operator()(int value) {
    cout << "value: "<< value << endl;
  }
};

int main(void)
{
  Print p;
  vector<int> v1; //vector相当于泛型数组容器
  for (int i = 0; i < 10; i++) {
    v1.push_back(i + 10);
  }
  for_each(v1.begin(), v1.end(), p);

  return 0;
}
```

<a id="32--和-||-操作符重载"></a>
#### 32. && 和 || 操作符重载
* && 和 || 操作符重载 会使短路失效, 所以不建议使用
```
class Test
{
public:
  Test(int value) {
    this->value = value;
  }
  int getValue()
  {
    cout << "value = " << this->value << endl;
    return this->value;
  }
  Test operator+(Test& another) {
    Test temp = Test(this->value + another.value);
    return temp;
  }
  bool operator&&(Test& another) {
    if (this->value && another.value) {
      return true;
    }
    else
    {
      return false;
    }
  }
  bool operator||(Test& another) {
    if (this->value || another.value) {
      return true;
    }
    else
    {
      return false;
    }
  }
  
private:
  int value;
};

int main(void)
{
  //Test t1(10);
  Test t1(0);
  Test t2(20);
  if (t1 && (t1 + t2)) {  //短路失效, 但表达式为false
    cout << "enter if(t1 && (t1 + t2)..." << endl;
  }
  if (t1 || (t1 + t2)) {  //短路失效, 表达式为true
    cout << "enter if(t1 || (t1 + t2)..." << endl;
  }

  return 0;
}
```

<a id="33-智能指针-auto_ptr----重载"></a>
#### 33. 智能指针 auto_ptr, `-> *` 重载
* `auto_ptr<A> a_p(new A);` 不需要手动释放内存
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <memory>
using namespace std;

class A
{
public:
  A()
  {
    cout << "run A()..." << endl;
  }
  ~A()
  {
    cout << "run ~A()..." << endl;
  }
  void printA() {
    cout << "run printA()" << endl;
  }
};

class MyAutoPtr
{
public:
  MyAutoPtr(A* pa) {
    this->p_a = pa;
  }

  A* operator->() { // ->返回的是指针
    return p_a;
  }
  A& operator*() {  // *返回的是对象
    return *p_a;
  }

  ~MyAutoPtr() {
    if (p_a != NULL) {
      delete p_a;
      p_a = NULL;
    }
  }
private:
  A* p_a;
};

void test_AutoPtr() {
  auto_ptr<A> auto_p(new A);
  auto_p->printA();
}

void test_MyAutoPtr() {
  MyAutoPtr my_auto_p(new A);
  my_auto_p->printA();
  (*my_auto_p).printA();
}

int main(void)
{
  //test_AutoPtr();
  test_MyAutoPtr();
  
  return 0;
}
```

<a id="34-继承的本质"></a>
#### 34. 继承的本质
* 继承是代码的复用, 而不是内存的复用
* 子类对象与父类对象没有关系, 内存空间完全独立
* 继承是一种避免代码重复, 简化创建class类的方式

<a id="35-继承方式"></a>
#### 35. 继承方式
* public: 父类的public, protected 在子类中__保持不变__, 父类的private在子类中不可见
* protected: 父类的public, protected在子类中都是 __protected__, 父类的private在子类中不可见
* private: 父类的public, protected 在子类中都变为 __private__, 父类的private在子类中不可见
![](image\extend_method.PNG)
* class中 protected成员 在类的外部(对象)不能被访问, 子类中可以访问

<a id="36-返回值-void-void-区别"></a>
#### 36. 返回值 void, void* 区别
* void 是无返回值
* void* 是任意数据类型返回值

<a id="37-类的赋值兼容性"></a>
#### 37. 类的赋值兼容性
* =等号(赋值): 左值永远要 <= 右值  
* 父类对象/指针/引用 = 子类对象/指针/引用
* 父类指针可以指向子类对象(多态: 面向对象最重要的特征)

<a id="38-继承中的构造和析构"></a>
#### 38. 继承中的构造和析构
* 子类不继承父类的构造和析构函数
* 子类构造时会调用父类构造函数(父->子)
* 子类析构时会调用父类析构函数(子->父)

<a id="39-virtual-虚继承"></a>
#### 39. virtual 虚继承
* 菱形继承关系,会产生继承的二义性
* 通过virtual 只拷贝一份
```
class Bed: virtual  public Furniture
{
public:
  void sleep()
  {
    cout << "在床上睡觉" << endl;
  }
};

//沙发类
class Sofa :virtual public Furniture
{
public:
  void sit()
  {
    cout << "在沙发上睡觉" << endl;
  }
};
```

<a id="40-多态"></a>
#### 40. 多态
* 实现多态的3个必要条件: 
    * 子类继承父类
    * 子类重写父类方法: 父类将该函数定义为__虚函数__ virtual void func_name(){...}
    * 父类指针指向子类对象
* 通过多态可以实现调用未来代码的功能
* 多态这种现象叫 动态联编 是迟绑定 或是 晚绑定
```
class Parent
{
public:
  Parent(int a) {
    this->a = a;
  }
  virtual void print() {
    cout << "Parent print()" << endl;
  }
private:
  int a;
};

class Child: public Parent
{
public:
  Child(int a, int b) : Parent(a) {
    this->b = b;
  }
  virtual void print() {
    cout << "Child print()" << endl;
  }
private:
  int b;
};

void factory_func(Parent *p) {
  p->print();
}

int main(void)
{
  Parent p(10);
  Child c(10, 100);
  factory_func(&p);
  factory_func(&c);

  return 0;
}
```

<a id="41-析构多态"></a>
#### 41. 析构多态
* 析构函数如果不加virtual实现多态,那么在父类指针指向子类对象的函数中调用delete,将只能执行父类析构, 从而发生子类对象内存泄漏
```
class A
{
public:
  A() {
    cout << "run A()..." << endl;
    this->p = new char[64];
    memset(p, 0, 64);
    strcpy(p, "A string");
  }
  virtual ~A() {
    cout << "run ~A()..." << endl;
    if (this->p != NULL) {
      delete[] this->p;
      this->p = NULL;
    }
  }
private:
  char *p;
};

class B: public A
{
public:
  B() {
    cout << "run B()..." << endl;
    this->p = new char[64];
    memset(p, 0, 64);
    strcpy(p, "B string");
  }
  virtual ~B() {
    cout << "run ~B()..." << endl;
    if (this->p != NULL) {
      delete[] this->p;
      this->p = NULL;
    }
  }
private:
  char *p;
};

void MyDelete(A *p) {
  delete p;
}

int main(void)
{
  A *p_a = new A;
  B *p_b = new B;
  MyDelete(p_b);

  return 0;
}
```
```
run A()...
run A()...
run B()...
run ~B()...
run ~A()...
请按任意键继续. . .
```

<a id="42-vptr"></a>
#### 42. vptr
* 不要在构造函数中去调用成员函数: 构造未完成时, vptr会先指向父类的vptr表
* 父类指针指向子类对象数组时, 遍历数组操作实现多态, 不能通过++来遍历, 要保证父类指针与子类指针的步长一致
```
  Child array[] = {Child(0), Child(1), Child(2) };
  //            array[0]   array[1] array[2]
  Child *cp = &array[0];
  Parent *pp = &array[0];
  int i = 0;
  for (i = 0, cp = &array[0], pp = cp; i < 3; i++, cp++, pp =cp) {
    pp->print();
  }
```
* 数据类型是对象占用内存空间大小的别名

<a id="43-纯虚函数"></a>
#### 43. 纯虚函数
* 只声明, 不定义, 相当于Java中的interface接口
  `virtual  double getArea() = 0;`
* 有纯虚函数的类是抽象类, 不能实例化
* 实现(定义)所有纯虚函数的类才能被实例化

<a id="44-面向对象开发流程分层独立开发"></a>
#### 44. 面向对象开发流程(分层独立开发)
* 抽象层: 接口 virtual
* 实现层: *.cpp 实现接口
* 业务逻辑层: main.cpp
* 整合封装接口类的类叫做__架构类__, private 中包含抽象父类指针
```
class Computer
{
public:
  Computer(CPU *cpu, Card *card, Memory *mem) //cpu = new IntelCPU
  {
    this->cpu = cpu;
    this->card = card;
    this->mem = mem;
  }

  //给电脑提供一个工作的方法
  void work()
  {
    cpu->caculate(); //发生多态
    card->display(); //发生多态
    mem->storage();//发生多态
  }

  ~Computer()
  {
    if (cpu != NULL) {
        delete cpu;
        cpu = NULL;
    }
    if (card != NULL) {
      delete card;
      card = NULL;
    }
    if (mem != NULL) {
      delete mem;
      mem = NULL;
    }
  }

private:
  CPU *cpu;
  Card* card;
  Memory*mem;
};
```

<a id="45-人员信息管理系统"></a>
#### 45. 人员信息管理系统
* 需求: 
    * 对象: 技术人员,经理,推销员,销售经理
    * 属性: 姓名,编号,级别,当月薪水
    * 功能: 
        + 员工信息初始化(init)计算月薪总额
        + 显示全部员工属性信息
        + 所有人员提升级别的功能(level +1)
        + 月薪计算办法 getpay():
            - 经理拿固定月薪 8000 元
            - 技术人员按每小时 100 元领取月薪(输入小时数)
            - 推销员的月薪按该推销员当月销售额的 4%提成(输入月销售量)
            - 销售经理拿固定月薪+领取销售提成,固定月薪为5000元,销售提成为所管辖部门当月销售总额的5%(输入月销售量)
    * 人员编号基数为 1000,每输入一个人员信息编号顺序加 1 (static int id)
    ![](image\employee_manage.PNG)
* __构造函数中不能有业务操作__, 只能有成员初始化(__避免重复构造__), 业务相关的初始化(输入输出, 计算...)应该__显示提供init()函数初始化对象__
* 析构多态时, 必须在基类(父类)上加virtual
* __new 和 delete 对象必须一致__, 否则内存溢出, 其本质是 malloc和free数据类型(内存空间长度)必须一致. 当对象头部数据类型恰巧相同, 编译器会隐式转换, 造成 new和delete对象不一致的隐患, 解决方法是__实现析构函数的多态__(virtual ~), 使 new和delete对象一致
* 通过父类指针delete子类对象,一定要在父类实现析构多态(在父类实现虚析构 virtual ~)

<a id="46-c语言中的函数指针"></a>
#### 46. C语言中的函数指针
* int(*fp)(int, int) 可以作为父类指针进行架构函数传参, 相当于virtual函数
* 相同类型, 不同业务的函数可以通过函数指针实现多态
```
//定义一个函数
int func(int a, int b) {
  cout << "func 111 " << endl;

  return 0;
}


//定义一个函数指针
// C语言中 决定一个函数类型
// 返回值 参数列表

typedef int(FUNC)(int, int); //定义一个函数类型

typedef int(*FUNC_POINTER)(int, int); //定义一个函数指针类型


//如何将一个函数指针当成一个函数参数呢？
void my_function(FUNC *fp)
{
  fp(10, 20);
}

void my_function2(FUNC_POINTER fp)
{
  fp(10, 20);
}

//他是一个架构函数。 
void my_function3(int(*fp)(int, int))
{
  cout << "1999 年写的架构" << endl;
  cout << "固定业务1" << endl;
  fp(10, 20);
  cout << "固定业务2" << endl;
}


//2018 实现一个子函数
int my_new_function(int a, int b)
{
  cout << a << b << endl;
  cout << "2018年实现的新业务" << endl;

  return 0;
}

int main(void)
{
  FUNC * p = func;
  FUNC_POINTER p2 = func;
  int(*fp)(int, int) = func;

  p(10, 20);
  (*p)(10, 20); //以上两种写法等价

  p2(10, 20);
  (*p2)(20, 20);

  fp(10, 20);

  cout << " --------  " << endl;

  //2018   实现一个子函数，再调用1999的架构
  my_function3(my_new_function);
  
  return 0;
}
```

<a id="47-c语言中实现多态控制反转ioc"></a>
#### 47. C语言中实现多态(控制反转IOC)
* 通过函数指针, 即回调函数(接口), 将业务的实现者和调用者进行隔离
* 目的: 方便代码的扩展重构, 减少代码修改量
* 赵云开锦囊(业务逻辑 create_tips 是从main函数中抽取封装出来的)
  - 接口: void open_tips(struct tips * tp)
```
typedef void(TIPS_FUNC)(void); //纯虚函数
tips1()
tips2()
tips3()
tips4()

struct tips // 包括 TIPS_FUNC *fp 指针; 

struct tips* create_tips(char *from, char *to, TIPS_FUNC *fp) //注册具体tips(函数指针传参)

void open_tips(struct tips * tp)  //架构函数, 传不同的tips, 回调, 实现多态
  tp->fp();

void destory_tips(struct tips * tp) //销毁

int main(void)  //业务逻辑, 过程
```
* Bryan.bai 打篮球
  - 接口: void use_method(struct Method* mp)
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

/*
篮球得分方法: threepoint, crossover, stepback, slamdump
*/

typedef void(TAKE_POINTS)(void);

void method1() {
  cout << "get point from three point" << endl;
}

void method2() {
  cout << "get point from crossover" << endl;
}

void method3() {
  cout << "get point from stepback" << endl;
}

void method4() {
  cout << "get point from slamdump" << endl;
}

struct Method
{
  int points;
  char name[64];
  TAKE_POINTS* fp;
};

struct Method* create_method(int points, char* name, TAKE_POINTS* fp) {
  struct Method* method = (Method*)malloc(sizeof(Method));
  strcpy(method->name, name);
  method->points = points;
  method->fp = fp;

  return method;

}

void use_method(struct Method* mp) {
  cout << mp->name << " get " << mp->points << endl;
  mp->fp();
}


void delete_method(struct Method* mp) {
  if (mp != NULL) {
    free(mp);
  }
}

int main(void)
{
  struct Method* mp1 = create_method(3, "Bryan.bai", method1);
  struct Method* mp2 = create_method(2, "Bryan.bai", method2);
  struct Method* mp3 = create_method(2, "Bryan.bai", method3);
  struct Method* mp4 = create_method(2, "Bryan.bai", method4);

  cout << "比赛开始" << endl;
  cout << "第一节: " << endl;
  use_method(mp1);
  cout << "第二节: " << endl;
  use_method(mp2);
  cout << "第三节: " << endl;
  use_method(mp3);
  cout << "第四节: " << endl;
  use_method(mp4);
  cout << "比赛结束" << endl;


  delete_method(mp1);
  delete_method(mp2);
  delete_method(mp3);
  delete_method(mp4);

  return 0;
}
```

<a id="48-模板-template-泛型"></a>
#### 48. 模板 template (泛型)
* 对数据类型的一种参数化
* `template <typename T> ==等价 template<class T>`
* 每一个使用T的函数之前都要有 `template <typename T>`
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

template<typename T>
void mySwap(T &a, T &b) {
  T temp = a;
  a = b;
  b = temp;
}

template<class T>
void print(T &a, T &b) {
  cout << "a: " << a << endl;
  cout << "b: " << b << endl;
}


int main(void)
{
  int a = 10;
  int b = 20;
  print<int>(a, b);
  mySwap<int>(a, b);
  print<int>(a, b);

  char x = 'a';
  char y = 'b';
  print<char>(x, y);
  mySwap<char>(x, y);
  print<char>(x, y);

  return 0;
}
```

<a id="49-函数模板数组排序"></a>
#### 49. 函数模板数组排序
* 数组传参, 会退化成数组指针, 所以要传数组长度
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

/*
使用template写一个函数实现对数组排序
*/

template<typename T>
void sort_array(T *array, int num) {
  T temp;

  for (int i = 0; i < num; i++) {
    for (int j = i + 1; j < num; j++) {
      if (array[i] > array[j]) {
        temp = array[i];
        array[i] = array[j];
        array[j] = temp;
      }
    }
  }
}


int main(void)
{
  char a1[] = "asdfjwerqwpiriwieqwi";
  int num = strlen(a1);
  sort_array<char>(a1, num);
  cout << "a1: " << a1 << endl;

  int a2[] = { 345,2343,5,645,21,3434 };
  num = sizeof(a2) / sizeof(a2[0]);
  sort_array<int>(a2, num);
  for (int i = 0; i < num; i++) {
    cout << a2[i] << " ";
  }
  cout << endl;

  return 0;
}
```

<a id="50-函数模板与函数重载"></a>
#### 50. 函数模板与函数重载
* 如果没有普通函数, 编译器会根据模板函数进行隐式的匹配。如果匹配到模板函数可以调用，调用模板
* 编译器匹配普通函数的优先级 大于 模板函数
* 当普通函数需要隐式转换的时候，如果模板函数能够匹配，那么就优先使用模板函数
* 模板函数 是类型的严格匹配 ，不会给参数类型进行隐式转换

<a id="51-模板类"></a>
#### 51. 模板类
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

template<class T>
class A
{
public:
  A(T a) {
    this->a = a;
  }
  void print() {
    cout << a << endl;
  }
private:
  T a;
};

template<class T>
void func(A<T> a) {
  a.print();
}

template<class T>
class C : public A<T>
{
public:
  C(T a, T c) : A(a) {
    this->c = c;
  }
  void printC() {
    cout << "c: " << this->c << endl;
  }
private:
  T c;
};

int main(void)
{
  A<int> a(10);
  a.print();

  A<char> b('b');
  b.print();

  C<int> c(1000, 2000);
  c.printC();

  return 0;
}
```

<a id="52-复数模板类"></a>
#### 52. 复数模板类
* 如果给一个模板类提供一个友元函数， `operator<< >>`要在这个函数的声明操作符和参数列表之间加 `<T>`
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

template<class T>
class Complex
{
  friend ostream& operator<< <T> (ostream &os, Complex<T> c);
public:
  Complex(T a, T b) {
    this->a = a;
    this->b = b;
  }
  void printComplex() {
    cout << a << " + " << b << "i" << endl;
  }

  Complex operator+ (Complex &another) {
    Complex temp = Complex(a + another.a, b + another.b);
    return temp;
  }
  Complex operator-(Complex &another);

private:
  T a;
  T b;
};

template<class T>
Complex<T> Complex<T>::operator-(Complex<T> & another) {
  Complex temp(this->a - another.a, this->b - another.b);
  return temp;
}

template<class T>
ostream& operator<<(ostream &os, Complex<T> c) {
  os << c.a << " + " << c.b << "i" << endl;
  return os;
}

int main(void)
{
  Complex<int> c1(1, 2);
  c1.printComplex();
  Complex<int> c2(3, 4);
  Complex<int> c3 = c1 + c2;
  c3.printComplex();

  Complex<double> c4(3.33, 4.56);
  c4.printComplex();

  Complex<int> c5 = c2 - c1;
  c5.printComplex();

  cout << c5 << endl;

  return 0;
}
```
* vs在编译有template模板类的文件时, 有时需要进行__二次编译__, 第一次编译不过时,再编译一次

<a id="53-模板类多文件"></a>
#### 53.  模板类多文件
* 结论: 模板类尽量写在一个文件中.h  .hpp

<a id="54-自定义模板数组类"></a>
#### 54. 自定义模板数组类
* 模板类要写在一个.h文件中
* 模板类中的方法尽量不要拆开写
* 容器(数组, 链表...)中的自定义数据类型一定要重写=号操作符(避免浅拷贝)
* MyVector.h
```
#pragma once
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

//自定义一个模板数组类
//1 该数组能够存放int ， double  还有自定义类型
//2 []   array[i] = ???   ; array[i]
//3 cout <<array;
//4 array1 = array2
//5 array1(array2)
//6 array[i] = array[j]

template<class T>
class MyVector
{
  
public:
  friend ostream& operator<<(ostream &os, MyVector<T> &v) {
    for (int i = 0; i < v.len; i++) {
      os << v[i] << " ";
    }
    return os;
  }
  MyVector();
  MyVector(int len);
  MyVector(const MyVector &v);
  ~MyVector();

  T& operator[](int index);
  MyVector& operator=(const MyVector &v);

private:
  int len;
  T *space;
};

template<class T>
MyVector<T>::MyVector()
{
  this->len = 0;
  this->space = NULL;
}
template<class T>
MyVector<T>::MyVector(int len) {
  this->len = len;
  this->space = new T[len];
}
template<class T>
MyVector<T>::MyVector(const MyVector<T> &v) {
  this->len = v.len;
  this->space = new T[len];
  for (int i = 0; i < this->len; i++) {
    this->space[i] = v.space[i];
  }
}
template<class T>
MyVector<T>::~MyVector() {
  if (this->space != NULL) {
    delete[] this->space;
    this->space = NULL;
  }
}



template<class T>
T& MyVector<T>::operator[](int index) {
  return this->space[index];
}

template<class T>
MyVector<T>& MyVector<T>::operator=(const MyVector &v) {
  if (this->space != NULL) {
    delete[] this->space;
    this->space = NULL;
    this->len = 0;
  }
  this->len = v.len;
  this->space = new T[this->len];
  
  for (int i = 0; i < this->len; i++) {
    this->space[i] = v.space[i];
  }

  return *this;

}
```
* main.cpp
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include "MyVector.h"

using namespace std;

void test_MyVector_int() {
  MyVector<int> v1(10);
  for (int i = 0; i < 10; i++) {
    v1[i] = i + 10;
  }
  cout << v1 << endl;

  MyVector<int> v2;
  v2 = v1;
  cout << v2 << endl;
  MyVector<int> v3(v2);
  cout << "v3: " << v3 << endl;
}

void test_MyVector_char() {
  

  MyVector<char> vc1(5);
  for (int i = 0; i < 5; i++) {
    vc1[i] = 'a' + i;
  }
  cout << "vc1: " << vc1 << endl;
  MyVector<char> vc2(5);
  vc2 = vc1;
  cout << "vc2: " << vc2 << endl;
  MyVector<char> vc3(vc2);
  cout << "vc3: " << vc3 << endl;
}

class Teacher
{
  
public:
  friend ostream& operator<< (ostream &os, Teacher &t);
  
  Teacher() {
    this->id = 0;
    this->name = NULL;
  }
  Teacher(int id, char *name) {
    this->id = id;
    int len = strlen(name);
    this->name = new char[len + 1];
    strcpy(this->name, name);
  }
  ~Teacher() {
    if (this->name != NULL) {
      delete[] this->name;
      this->name = NULL;
      this->id = 0;
    }
  }

  Teacher(const Teacher &t) {
    this->id = t.id;
    int len = strlen(t.name);
    this->name = new char[len + 1];
    strcpy(this->name, t.name);
  }

  Teacher & operator=(const Teacher &t) {
    if (this->name != NULL) {
      delete[] this->name;
      this->name = NULL;
      this->id = 0;
    }
    this->id = t.id;
    int len = strlen(t.name);
    this->name = new char[len + 1];
    strcpy(this->name, t.name);

    return *this;
  }
  
  void printTeacher() {
    cout << "id: " << this->id << " name: " << this->name << endl;
  }

private:
  int id;
  char *name;
};

ostream& operator<<(ostream &os, Teacher &t) {
  //t.printTeacher();
  os << "id: " << t.id << " name: " << t.name << endl;
  return os;
}

void test_MyVector_Teacher() {

  Teacher t1(1, "zhang1");
  Teacher t2(2, "zhang2");
  Teacher t3(3, "zhang3");
  Teacher t4(4, "zhang4");
  Teacher t5(5, "zhang5");

  MyVector<Teacher> t_array(5);
  t_array[0] = t1;
  t_array[1] = t2;
  t_array[2] = t3;
  t_array[3] = t4;
  t_array[4] = t5;

  for (int i = 0; i < 5; i++) {
    cout << t_array[i] << endl;
  }

}


int main(void)
{
  test_MyVector_int();
  cout << "------" << endl;
  test_MyVector_char();
  cout << "------" << endl;
  test_MyVector_Teacher();

  return 0;
}
```

<a id="55-类型转换"></a>
#### 55. 类型转换
* static_cast<>() 普通类型强转
* `dynamic_cast<Child*>(Parent)`  父类转为子类(弱->强)强转
    *  `dog = dynamic_cast<Dog*> (animal);` 子类指针指向父类对象
    *  转换失败, 返回NULL
* `const_cast<>()` 去掉 const 属性
    * 在栈上开辟的可以修改, 在常量区的依然不能被修改
* `reinterpret_cast<>()` 不同类型之间真正的强转

<a id="56-异常处理"></a>
#### 56. 异常处理
* throw 之后, 函数退出, __相当于 return__
* throw 之后, 所有在栈上开辟的空间都会被释放
* throw出类对象(自定义异常类对象), 使用&引用比较合适
    + 普通元素类型的异常捕获，不能够跟 引用捕获同时存在
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

void divide(int a, int b) throw(int, char, char*)
{
  if (b == 0) {
    cout << "b == 0" << endl;
    throw "分母为0";
  }
  cout << "a / b = " << a / b << endl;
}

void use_divide(int a, int b) {
  try
  {
    divide(a, b);
  }
  catch (...)
  {
    throw;
  }
}

int main(void)
{
  try
  {
    //divide(10, 0);
    use_divide(10, 0);
  }
  catch (char* e)
  {
    cout << e << endl;
  }
  
  return 0;
}
```

<a id="57-explicit"></a>
#### 57. explicit
* 用在class的构造函数中, 明确指出此构造函数禁止隐式转换

<a id="58-exception-标准库异常类"></a>
#### 58. exception 标准库异常类
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdexcept>
#include <string>

using namespace std;

class Teacher
{
public:
  Teacher(int id, string name) {
    if (id > 1000) {
      throw out_of_range("超出1000的范围");
    }

    this->id = id;
    this->name = name;
  }
private:
  int id;
  string name;
};

int main(void)
{
  try
  {
    Teacher t1(100001, "Bryan");
  }
  catch (exception &e)
  {
    cout << e.what() << endl;
  }
  
}
```

<a id="59-自定义异常类"></a>
#### 59. 自定义异常类
* 继承 exception 类, 实现 e.what() 方法
```
virtual const char *what() const{
    cout << "MyException 的异常" << endl;
    return this->m_s;
  }
```

<a id="60-cinputback"></a>
#### 60. cin.putback()
```
void test6() {
  cout << "Please input a number or string:" << endl;
  char ch;
  cin.get(ch);
  if (ch >= '0' && ch <= '9') {
    cout << "inputed number" << endl;
    cin.putback(ch);
    int num;
    cin >> num;
    cout << "num:" << num << endl;
  }
  else
  {
    cout << "inputed string" << endl;
    cin.putback(ch);
    char buf[128] = { 0 };
    cin.getline(buf, 128);
    cout << "buf: " << buf << endl;
  }
}
```

<a id="61-文件流的基本操作"></a>
#### 61. 文件流的基本操作
```
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <fstream>

using namespace std;

void test_write_file() {
  char *file_name = "e:/temp/haha.txt";
  ofstream f2out(file_name, ios::app);
  if (!f2out) {
    cout << "file open fail..." << endl;
    return;
  }
  f2out << "hello...111" << endl;
  f2out << "hello...222" << endl;
  f2out << "hello...333" << endl;

  f2out.close();

}

void test_read_file() {
  char *file_name = "e:/temp/haha.txt";
  ifstream fin(file_name, ios::in);
  if (!fin) {
    cout << "open file fail..." << endl;
    return;
  }
  char ch;
  while (fin.get(ch))
  {
    cout << ch;
  }
  fin.close();
}

class Teacher
{
public:
  Teacher() {};
  Teacher(int id, char * name) {
    this->id = id;
    strcpy(this->name, name);
  }
  void printT() {
    cout << "id: " << id << " name: " << name << endl;
  }
private:
  int id;
  char name[64];
};

void test_write_bin() {
  char * file_name = "e:/temp/bin_teacher.dat";
  Teacher t1(1, "zhang3");
  Teacher t2(2, "li4");

  ofstream fout(file_name, ios::binary);
  if (!fout) {
    cout << "open file fail..." << endl;
    return;
  }
  fout.write((char*)&t1, sizeof(Teacher));
  fout.write((char*)&t2, sizeof(Teacher));

  fout.close();
}

void test_read_bin() {
  char * file_name = "e:/temp/bin_teacher.dat";
  Teacher temp;

  ifstream fin(file_name, ios::binary);
  if (!fin) {
    cout << "open file fail..." << endl;
    return;
  }

  fin.read((char*)&temp, sizeof(Teacher));
  temp.printT();
  fin.read((char*)&temp, sizeof(Teacher));
  temp.printT();

  fin.close();
}

int main(void)
{
  //test_write_file();
  //test_read_file();
  //test_write_bin();
  test_read_bin();
}
```
