---
layout: post
title:  "QT考试复习（一）"
date:   2016-04-12 06:14:54
categories: Exam
tags: Qt考试 C++ 
author: Lyon
---
* content
{:toc}

面向某次考试给同学们做的复习资料，只是写出了针对考试需要复习的C++知识点，当前总结这个还是花了点时间，现在放博客里凑文章数。







# C++基础

## 初始化与赋值

***

初始化：创建变量并给变量赋值。

赋    值：擦除对象当前值，再用新值代替。

```c++
int i(10); //等价于 int i =10;
```

"="适用于**基本类型**和**单一值类型** ，如果是**类类型**，不能直接使用"="初始化，必须重载赋值运算符"="。

```c++
class A{
  public:
  A()
  {
  	cout<<"默认构造函数"<<endl;
  }
  
  A(const A&)
  {
  	cout<<"拷贝构造函数"<<endl;
  }
  
  A &operator=(const A&)
  {
  	cout<<"重载赋值操作符"<<endl;
    return *this;
  }
  
  ~A()
  {
  	cout<<"析构函数"<<endl;
  }
}
```

类类型变量初始化比赋值**效率高**，所以能初始化时避免使用=号赋值。

***

## 作用域限定运算符::

***

**用于解决变量的名字冲突**

访问当前作用域内被当前局部变量隐藏的外层全局变量

```c++
int a;
void main(void)
{
  float a;
  	a =5.5;  //当前局部的float类型变量a
  ::a = 3;   //int类型 全局变量a
}
```

***

## 引用类型和const

***

**引用就是变量的另一个名字（别名）**

引用必须用该引用**同类型（或协变类型）**的变量初始化，而且**必须初始化**，并且初始化后**不能再绑定到别的变量**。

```c++
int x =0;
int &x1=x; 	// x1绑定x 等价于 int &x1(x);
int &x2; 	//error 必须初始化
int &x3=10; //error 初始值 必须为一个对象
```

*引用与指针的比较*

- 指针可能（也可能不）指向某个实际变量，当我们使用指针时，一定得确定其值不为NULL，至于引用，必定会代表某个变量，所以不必检查。
- 使用引用的另一个理由是如果我们使用引用，与我们直接使用传值方式相同，使程序书写便利。

*const的用途*

1. 当const类型说明符用于变量时：**常量**

   - 其作用是冻结所定义的变量在定义域范围内为常量。

   - C++要求用const说明变量必须在定义时初始化，以后不允许再有赋值操作。

   - const说明的常量可赋值给变量。

     ```c++
        void main(void)
          {
          int x =1 ;    	//变量 可不初始化
          const int y=2;	//常量 必须初始化 且以后不能再赋值
          x = y ;   //ok
          y = x; 	//error
        }
     ```

2. 当const类型说明符用于说明引用时：**常引用**

   - 则称为常引用，即不能通过引用名来修改原变量的值。

   ```c++
   int i =100; const int &ri=i;
   ```

3. 当const类型说明符用于说明指针类型变量时：**常指针**

   - 冻结所定义的指针变量所指向的数据。int const *p;   const int *p;
   - 冻结指针的地址值。  int * const p;
   - 冻结指针变量所指向的数据和指针的地址值。 const int*  const p;  

4. 当const类型说明符用于**函数的形式参数**时：

   - 冻结实际参数在该函数内为常量，即该参数不允许在函数内被修改。
   - 形参可以是const int * x或const int &rx。

5. 当const说明符用于**函数的返回类型**时：

   - 限定该函数返回的是个常指针或者常引用
   - 形式为const int* func(); 
   - 调用为 const int* p =func();

6. 当const类型说明符用于**类的成员函数**：

   - 不允许改成员函数修改对象的数据成员。
   - 形式为 int classname:: memfunc() const


***

## 抽象、封装、继承、多态

***

1. 抽象

         > 对具体对象进行概括，抽出这一类对象的公共性质并加以描述。

         集中注意力，只关注问题中那些在当前背景下最为重要的部分，不被食物的表象所迷惑。

2. 封装

   > 将抽象出来的数据成员、代码成员相结合，作为一个整体加以包装。

   封装的目的是增强安全性和简化编程，使用者不必了解具体的实现细节，而只需要通过外部接口，以特定的访问权限使用其中的成员即可。

3. 继承

   > 保持已有类的特性而构造新类的过程。

4. 多态

   > 使编译器能够在运行时决定是使用基类中定义的函数还是派生类中定义的函数。

***

## 构造、拷贝构造、析构、赋值函数

***

1. 构造函数

         定义： 类名::类名(形参总表):成员(参数1),成员(参数2)  (即初始化列表)

         {

         ​	构造函数的构造体

         }//**初始化列表必须按照成员变量声明的次序书写**

         构造函数是**特殊**的成员函数：保证对象的每个数据成员具有合适的初始值

   - 不能由对象调用，能够自动调用。
   - 没有返回值。
   - 有初始化列表(**只有构造函数有初始化列表**)。

     默认构造函数：A a;//类名 对象名;  **不带任何初值** 编译器自动为你调用。

     带默认参数的构造函数：

   ```c++
         A(int x = 0,int y = 0);     //声明
         A::A(int x, int y):m_x(x),m_y(y){}   //定义
   ```

         构造对象的方式:

   - 有名     对象：     A  a 或 A a(参数)

   - 无名对象(堆)：new A 或new A(参数)

     注：A a; 千万不能写成 A a();

2. 拷贝构造函数

   - 使用一个已经存在的对象去初始化同类的一个新对象时调用拷贝构造函数。




- 拷贝构造函数是一种**特殊的构造函数**，其形参为本类对象的引用。
  - 为了规范，建议使用**const引用**。

   三种调用方式：（只有这三种情况）

- 用类的一个**对象去初始化该类的另一个对象**时，系统自动调用拷贝构造函数实现拷贝赋值。

  ```c++
  void main (void)
  {
    A a1(1,2);
    A a2(a1); //拷贝构造函数被调用
  }
  ```

  - 当**函数的形参为类对象**，调用函数时，实参赋值给形参，系统会自动调用拷贝构造函数。

  ```c++
  void func(A a)
  {
    cout<<c.getX()<<endl;
  }

  void main(void)
  {
    A a(1,2);
    func(a);  //拷贝构造函数被调用
  }
  ```

  - 当**函数的返回值是类对象**时，系统自动调用拷贝构造函数。

  ```c++
  A func()
  {
    A a(1,2);
    return a;
  }
  void main(void)
  {
    A a;
    a=func(); //拷贝构造函数被调用 子函数返回了临时对象
  }
  ```





1. 析构函数

   - 用来完成**对象被删除前**的一些**清理工作**。
   - 在**对象的生存期即将结束的时刻**被系统自动调用。
   - 函数名为 ~类名，无参，负责资源清理工作。

     析构函数的调用顺序与构造函数的调用顺序**相反**。

     如果用new分配空间，需要**在delete的时候**才会调用析构函数。

2. 赋值函数

   定义： 类名& operator =(类名 &)；

   使用一个已经存在的对象去对同类的一个对象赋值时调用赋值函数

   ```c++
   A a (1,2);
   A b = a; //调用赋值函数
   ```

**注：当类没有定义以上四个函数时，编译器会自动补偿，如果已经定义则不补，缺什么补什么。**

也就是 class A{};  等同于下面：

```c++
class A
{
  public:
  A();                        	//缺省构造函数
  A(const A& rA);				//拷贝构造函数
  ~A();							//析构函数
  A& operator=(const A& rA);	//赋值函数
};
```

 ***

## 友元 - friend

***

友元机制：

- 允许一个类将对其**非公有成员的访问权**授予指定类或函数。





- 友元声明只能出现在**类定义的内部**，在类中可以出现在任何地方，但通常放在类定义的开始或者结尾
- 友元并不是类的成员，只是一种授予关系，所以不受访问控制符(public private protected)的影响

友元分类：

- 声明整个类为友元

  ```c++
  friend class A;  //A 为当前类的友元
  ```

- 声明类中的某个成员函数为友元

  ```c++
  friend void A::func(B& b); //A::func函数为当前类B的友元
  ```

- 声明一个非成员函数为友元

  ```c++
  friend void func(B& b); //非成员函数func为当前类B的友元
  ```

***

## 类成员的补充

***

1. const数据成员

   ```c++
         const int a;
   ```

         对象诞生后 其const数据成员不能被改变。

2. const成员函数

   - const修饰this指针

     this不再是：Type* const this  而是：const Type* const this

   - const需要在声明和定义中写出

   - const成员函数内部无法修改数据成员

   - const数据内部无法调用non-const成员函数

   |               | const对象 | non-const对象 |
   | ------------- | ------- | ----------- |
   | const成员函数     | 可以访问    | 可以访问        |
   | non-const成员函数 | 不可以访问   | 可以访问        |

3. static数据成员

   - 类属性 该类的所有对象维护着同一份拷贝
   - 不能在构造函数中初始化，必须在类外单独进行初始化，在程序运行之前完成，并且用::来指明所从属的类名

4. static成员函数

   - 类外代码使用类名::来调用静态成员函数
   - 只能访问属于该类的静态数据成员或静态成员函数
   - 没有this指针，所以不能为常函数

   **声明需要加static修饰，定义则不需要**。

***

## 运算符重载

***

> 对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型。

声明形式：

​	函数类型 operator 运算符 (形参表){}

- 重载为类成员函数时：

  - 形参个数 = 原操作数 - 1 (后置++、--除外) 

  ```c++
  const complex complex::operator +(complex c2)
  {
    return complex(c2.real+real,c2.imag+imag);
  }
  ```

  赋值操作符=、下标[]、调用()和成员访问箭头->必须定义为**成员函数**。

- 重载为友元函数时：

  - 形参个数=原操作数 (后置++、--除外)

  ```c++
  friend complex operator +(complex c1,complex c2); //声明
  complex operator +(complex c1,complex c2)         //定义
  {
    return complex(c1.real+c2.real,c1,imag+c2.imag);
  }
  ```

  如果一个运算符想用某个基本类型的数据调用，它必须定义成**全局的友元函数**。

  输入输出流>><<操作符定义为**友元函数**。

  为了区分前置后置运算，后置单目运算符++和--的重载函数，其形参表中要**加一个int**，但不必写形参名。

***

## 继承

***

> 保持已有类的特性而构造新类的过程：吸收、改造、新增。

继承方式：

1. 公有继承 public （原封不动）

2. 受保护继承 protected （折中）

   public、protected->protected  private不变

3. 私有继承private （化公为私）

   public、protected->private  原private被隐藏

通过对象访问私有和保护成员，只能在**本类作用域**或者**友元**中访问。

派生类只能通过**派生类对象**访问其基类的protected成员，基类无访问权限。



派生类构造函数：

- 基类的构造函数**不被继承**，派生类需要重新定义自己的构造函数
- 派生类构造函数只需要对**本类新增的成员**进行初始化
- 继承来的基类成员的初始化工作由基类构造函数完成，派生类要用**初始化列表**对有参基类构造函数给出实参
- 不能直接初始化**继承下来的成员**
- 只初始化自己的**直接基类**


派生类拷贝构造函数：

- 派生类对象调用缺省拷贝构造函数时，编译器将**自动**调用基类的缺省拷贝构造函数
- 若基类有显式拷贝构造函数需要传递参数时，最好为派生类编写拷贝构造函数，以便为基类相应的拷贝构造函数传递参数（派生类对象作为参数，向上兼容）

派生类析构函数：

- 编译器**自动连锁调用**
- 先析构派生类部分再析构基类部分

类型兼容规则：

> 在需要基类对象的**任何**地方，都可以使用公有派生类对象替代

- 派生类对象可以赋值给基类对象   （真正发生了**对象切割**）
- 派生类对象可以初始化基类的引用
- 基类的指针可以指向派生类对象

作用域深入讨论：

- 作用域遮掩关系

  ```c++
  class Base()
  {
  public:
    void f1();
    void f1(int);
    void f2();
  };

  class Derived:public Base
  {
  public:
    void f1();  //遮掩了Base::f1()和Base::f1(int)  同名遮掩与参数无关
    void f3();
  };  		  //故只有f1(),Base::f2(),f3()三个函数
  ```





- 使用using声明来继承重载函数

  ```c++
  //class Base 不变
  class Derived:public Base
  {
  public:
    using Base:f1; //把f1引入到作用域内
    void f1();     //同名遮掩
    void f3();
  }                //含有f1 f1(int) Base::f2() f3()四个函数
  ```

- 使用转交函数（部分老式编译器不支持using声明）

  ```c++
  //class Base 不变
  class Derived:public Base
  {
  public:
    void f1();     //同名遮掩
    void f1(int i)  
    {
    	Base::f1(i); //转交给基类函数
    }
    void f3();
  }
  ```


两种二义性：

- 同名二义性：基类之间、基类与派生类之间发生成员同名

  **同名隐藏规则**

  派生类屏蔽基类同名成员，如要使用基类同名成员则需要加基类名和::限定符

  解决方法：

  1. 类名来限定

     ```c++
           c.A::f();   //使用基类A的函数f()
           c.B::f();	//使用基类B的函数f()
           c.f();		//使用派生类自己的函数f()
     ```

  2. 同名覆盖，再造接口

     ```c++
     void C::f()
     {
       A::f();
       B::f();   //根据需要来调用基类A或者基类B的函数f()

     ```

- 路径二义性：派生类从多个基类派生，而这些基类又从同一个基类派生，在访问此共同基类中的成员时产生

  解决方法：虚基类

  - 用virtual修饰说明共同的直接基类

    ```c++
    class B1:virtual public B
    ```

  - 为最远派生类提供了唯一的基类成员，而不产生多个重复副本

    ```c++
    class B 
    {
    public:
      int b;
    } //存储了成员b
    class B1:virtual public B
    {
    public:
      int b1;
    } //存储了新增成员b1 和基类继承的b的地址
    class B2:virtual public B
    {
    public:
      int b2;
    } //存储了新增成员b2 和基类继承的b的地址

    class C:public B1,public B2
    {
    public:
      int c;
    } //存储了新增成员c 和基类继承的b 直接基类继承的b1、b2 
    ```

***

## 多态

***

> 向不同对象发生同一个消息时，不同的对象在接收时会产生不同的行为。

多态：

- 专用多态
  - 强制多态
    - const_cast<> 、static_cast<>、reinterpret_cast<>、dynamic_cast<>、C方式的()
  - 重载多态
    - 函数重载、运算符重载
- 通用多态
  - 参数多态
    - 函数模版、类模版
  - 包含多态
    - 虚函数机制

虚函数：

- 只有类的non-static成员函数可以定义为virtual，并且构造函数不能为virtual
- 这样定义是希望这个函数的实现**在派生类中被修改**





- 一旦在基类定义为了虚函数就一直是虚函数，派生类重新定义是可以省略virtual关键字





- 通过**引用**或者**指针**调用虚函数时，编译器将产生代码，在运行时确定调用哪个函数，被调用的是与**动态类型相对应的函数**

产生多态条件：

- 必须有继承产生的类族
- 必须是公有继承（类型兼容）
- 基类的某成员函数使用了virtual
- 派生类的成员函数要重写该虚函数
- 派生类对象需要用**指针**或**引用**来调用虚成员函数（类型兼容防止截断）

虚函数实现机制：

- 每个类会维护一个vtable，用来登记虚函数
- vfptr 非静态数据成员

纯虚函数、抽象类：

```c++
virtual void func()=0;  //简单定义
```

- 含有（或继承）一个或多个纯虚函数的类是抽象基类
- 直到该纯虚函数被重写且定义为非纯虚函数，否则它一直为纯虚函数
- 不能用抽象类定义对象
- 纯虚函数可以带有默认实现

***