# 类

最重要的OOP特性：抽象；封装和数据隐藏；多态；继承；代码的可重用性；

类是一种将抽象转换为用户定义类型的C++工具，它将数据表示和操纵数据的方法组合成一个整洁的包。

类规范由两个部分组成：

- 类声明：以数据成员的方式描述数据部分，以成员函数（被称为方法）的方式描述公有接口。
- 类方法定义：描述如何实现类成员函数。

## 对接口的理解

接口是一个共享框架，供两个系统交互时使用，对于类，我们说公共接口。在这里，公众（public）是使用类的程序，交互系统由**类对象**组成，而接口由编写类的人提供的**方法**组成。例如，要计算string对象中包含多少个字符，您无需打开对象，而只需使用string类提供的size( )方法。该方法就是类提供的公共接口。

## 访问控制

因此，公有成员函数（public）是程序和对象的私有成员（private）之间的桥梁，提供了对象和程序之间的**接口**。防止程序直接访问数据被称为**数据隐藏**。C++还提供了第三个访问控制关键字protected。

数据隐藏是OOP主要的目标之一，因此数据项通常放在私有部分，组成类接口的成员函数放在公有部分。一般不声明C++默认是私有成员。

## 封装

类设计尽可能将**公有接口**与**实现细节**分开。公有接口表示设计的抽象组件。将实现细节放在一起并将它们与抽象分开被称为**封装**。**数据隐藏**（将数据放在类的私有部分中，使得数据只能被授权的函数访问。）是一种封装，将类函数定义和类声明放在不同的文件中也是一种封装。

用户只需要通过类定义中的公共接口（public成员函数）接受什么样的参数以及返回什么类型的值。而不需要关心其实现的细节，**将实现细节从接口设计中分离出来**。如果以后找到了更好的、实现数据表示或成员函数细节的方法，可以对这些细节进行修改，而无需修改程序接口，这使程序维护起来更容易。

## 类和结构的区别

类是对结构的扩展，结构的默认访问类型是public，而类为private。C++程序员通常使用类来实现类描述，而把结构限制为只表示纯粹的数据对象（常被称为普通老式数据（POD，Plain Old Data）结构）。

## 类函数定义

类声明表明了类的数据结构以及公共接口，而公共接口的实现细节则需要对类函数进行定义。

- 定义成员函数时，使用作用域解析运算符`::`来标识函数所属的类；
- 类方法可以访问类的private组件。

也就是说，私有成员变量或成员函数，只有编写类的人可以使用它，但使用这个类的人不能使用。

## 内联函数 

当一个成员函数十分短小时，可以将其实现直接写在类声明中，此时它将自动成为内联函数。当然，如果不想在类声明中定义，也可以在类声明之外定义成员函数，并使其成为内联函数。为此，只需在类实现部分中定义函数时使用 `inline`限定符即可。

## 客户/服务器模型

OOP程序员常依照客户/服务器模型来讨论程序设计。**客户**只能通过以公有方式定义的接口使用服务器，这意味着客户（客户程序员）唯一的责任是了解该接口。**服务器**（服务器设计人员）的责任是确保服务器根据该接口可靠并准确地执行。服务器设计人员只能修改类设计的实现细节，而不能修改接口。这样程序员独立地对客户和服务器进行改进，对服务器的修改不会客户的行为造成意外的影响。

# 构造函数

由于OPP数据隐藏的特性，程序不能直接访问数据成员，只能通过成员函数来间接访问，因此类对象无法像`int a = 1`这样进行初始化，因此需要设计合适的成员函数，但又不能违背数据隐藏的初衷，C++引入了构造函数，它可以在创建对象的时候对其进行初始化。

构造函数名称与类相同，没有返回值，不需要声明类型。每次创建对象时，C++都使用类的的构造函数，因此构造函数构造出对象之前，对象是不存在的。因此构造函数被用来创建对象，而不能通过对象来调用。

```c++
Student stu1("李华", 21, 98);
stu1.play(); // 可以通过对象调用成员函数
stu1.Student(); // 不可以调用构造函数
```

## 使用构造函数

- 显式地调用构造函数：`Student stu1 = Student("李华", 21, 98);`
- 隐式地调用构造函数：`Student stu1("李华", 21, 98);`

类成员名和构造函数参数名：

构造函数的参数表示的不是类成员，而是赋给类成员的值。因此，参数名不能与类成员相同，为避免这种混乱，一种常见的做法是在数据成员名中使用 `m_` 前缀; 另一种常见的做法是，在成员名中使用后缀`_`

```c++
class Student{
   private:
    	string m_name;
    	int m_age;
        int m_score;
   public:
    	Student(string name,int age,double score);
};
Student::Student(string name,int age, int score)
{
    m_name = name;
    m_age = age;
    m_score = score;
    
}
```

## 默认构造函数

默认构造函数是在未提供显式初始值时，用来创建对象的构造函数：

```c++
Student stu1; // 隐式
Student stu1(){}; // 显式
```

如果没有提供任何构造函数，则C++将自动提供默认构造函数，其不做任何工作，类似于 `int a`；

但是有一个需要注意，**当且仅当**没有定义任何构造函数时，编译器才会提供默认构造函数。但凡程序员定义了构造函数后，就必须为它提供默认构造函数。这样做的原因可能是想禁止创建未初始化的对象。

定义默认构造函数两种方式：

- 给已有构造函数的所有参数提供默认值：

  ```c++
  Student(cosnt string name = "NAME", int age = 0, double score = 0.0);
  ```

- 定义另一个没有参数的构造函数：

  ```c++
  Student();
  ```

由于只能有一个默认构造函数，因此不要同时采用这两种方式。实际上，通常应初始化所有的对象，以确保所有成员一开始就有已知的合理值。因此，用户定义的默认构造函数通常给所有成员提供隐式初始值。

```c++
Student::Student()
{
	cosnt string name = "NAME";
    int age = 0;
    double score = 0.0;
}
```

## 析构函数

析构函数在类对象过期时析构函数将自动被调用，主要是完成清理工作，如果构造函数使用 `new` 来分配内存，则析构函数将使用 `delete `来释放这些内存，和构造函数一样，析构函数也可以没有返回值和声明类型。与构造函数不同的是，析构函数没有参数，因此析构函数的原型如下：

```c++
~Student();
```

如果程序员没有提供析构函数，编译器将隐式地声明一个默认析构函数，并在发现导致对象被删除的代码后，提供默认析构函数的定义。

# `this` 指针

this的应用背景如下:

```c++
Student stu1("李华", 21, 98);
Student stu2("张三", 22, 89);
const Student & Student::topScore(const Student &s) const
{
    if(s.score > score)
        return s;
    else
        return *this;
}
// 调用：
stu1.topScore(stu2);
```

显然，`s.score` 是作为参数传递的对象（`stu2`）的总值，而 `score` 是用来调用该方法的对象（`stu1`）的总值，当需要返回调用该方法的对象（`stu1`）时，使用 `*this` 来表示这个对象；**`this` 指针指向用来调用成员函数的对象**（this被作为隐藏参数传递给方法）。

一般来说，所有的类方法都将this指针设置为调用它的对象的地址。事实上，`topScore( )` 中的`score`  只不过是 `this->score` 的简写。

# 对象数组

如果程序创建未被显式初始化的类对象时，总是调用默认构造函数。

```c++
Student myClass[4]; // 创建一个存储4个Student数组的对象，此处调用默认构造函数
myClass[0].show(); // 调用成员函数
myClass[3].update();
const Student *this = myClass[2].topScore(myClass[1]); // 返回分值更高的学生对象
```

可以用构造函数来初始化数组元素。在这种情况下，必须为每个元素调用构造函数，同样可以对不同的元素使用不同的构造函数：

```c++
const int STUS = 10;
Student students[STUS] = {
    Stock("李华", 21, 98),
    Stock(),
    Stock("张三", 22, 89),
};
```

# 类的作用域

直接成员运算符（`.`）、间接成员运算符（`->`）或作用域解析运算符（`::`）

```c++
int main()
{
    Student *p = new Student;	 // 调用默认构造函数
    Student stu1("李华", 21, 98); // 调用定义构造函数
    stu1.show(); // 直接成员运算符
    p->show();	 // 间接成员运算符
} 
```

## 在类中声明一个常量

比如我们有一个类 `Bakery` ，记录每个月的支出，显然一年12个月，该常量对于所有对象来说都是相同
的，因此可以创建一个由所有对象共享的常量。

```c++
class Bakery{
	private:
    	const int Months = 12; // 错误
    	double costs[Months];
};
```

由于类仅仅是对象的描述，而没有创建对象，因此，在创建对象前，将没有用于存储值的空间，实现方式如下：

- 方式一：类中声明一个枚举

  在类声明中声明的枚举的作用域为整个类，因此可以用枚举为整型常量提供作用域为整个类的符号名
  称。

  ```c++
  class Bakery{
  	private:c++
      	enum {Months = 12}; 
      	double costs[Months];
  };
  ```

  这里使用枚举只是为了创建符号常量，并不打算创建枚举类型的变量，因此不需要提供枚举名。`Months` 只是一个符号名称，在作用域为整个类的代码中遇到它时，编译器将用 `12` 来替换它

- 方式二：使用关键字static

  ```c++
  class Bakery{
  	private:c++
      	static const int Months = 12;
      	double costs[Months];
  };
  ```

  这将创建一个名为 `Months` 的常量，该常量将与其他静态变量存储在一起，而不是存储在对象中。

## 作用域内枚举

传统枚举中的冲突：

```c++
enum egg {Small, Medium, Large};
enum t_shirt {Small, Medium, Large};
```

C++11提供了一种新枚举，其枚举量的作用域为类:

```c++
enum class egg {Small, Medium, Large};
enum class t_shirt {Small, Medium, Large};
```

也可使用关键字`struct` 代替 `class` ，无论使用哪种方式，都需要使用枚举名来限定枚举量：

```c++
egg e_choice = egg::Small;
t_shirt t_choice = t_shirt::Large;
```

# 运算符重载与友元

## 运算符重载

C++允许将运算符重载扩展到用户定义的类型，例如，允许使用 `+` 将两个对象相加。这种简单的加法表示法隐藏了内部机理，并强调了实质，这是OOP的另一个目标。

```c++
Time t1(1,43); // t1(hour,minutes)
Time t2(3,36);
Time sum = t1 + t2; // t1,t2
```

运算符函数的格式如下：

```c++
operator+(); // 重载+运算符
```

例如：实现一个时间相加的重载 `+` 运算符方法：

```c++
Time Time::operator+(const Time &t) const
{
	Time sum; // 需要返回的对象
	sum.minutes = minutes + t.minutes; // minutes是用来调用该方法的对象的成员
	sum.hours = hours + t.hours + sum.minutes / 60; // t.hours是作为参数传入的对象t的成员
	sum.minutes %= 60;
	return sum;
}
```

实现一个时间倍数的重载 `*` 运算符方法：

```c++
Time Time::operator*(double mult) const
{
	Time result;
	long totalminutes = hours*mult*60 + minutes*mult;
	result.hours = totalminutes / 60;
	result.minutes = totalminutes % 60;
	return result;
}
int main(){
    Time adjusted = t1 	* 1.5; //等价于operator*(1.5);
    Time adjusted = 1.5 * t1; // error
}
```

从概念上说， `t1 * 1.5` 应与 `1.5 * t1` 相同，但是`operator*` 要求左边操作数是对象，这客户端并不友好，你可能想到使用非成员函数来定义：

<img src="/home/alivn/Documents/算法与面试/images/Blog/C++PrimerPlus/image-20230226163123885.png" alt="image-20230226163123885" style="zoom:80%;" />

显然，非成员函数不能直接访问类的私有数据，至少常规非成员函数不能访问。因此，需要引入有一类特殊的非成员函数，它可以访问类的私有成员，被称为**友元函数**。

> 此处，有人突发奇想，写出以下代码：
>
> ```c++
> class Time {
> private:
>        int hours;
>        int minutes;
> public:
>        Time operator*(const Time &t, double m);
> };
> ```
>
> 显然这是错误的，因为成员函数内部，你可以访问当前对象的成员变量（即this指针所指向的对象），而传入的参数又是一个对象，还有一个double，这是三个变量，编译器并不知道你在做什么......

## 友元函数

接上文，由于非成员函数不能直接访问类的私有数据，因此我们无法达到预期的目的，友元函数则可以解决这个问题，首先需要明确的是**友元函数是非成员函数，但是其访问权限与成员函数相同**，友元函数定义的位置与成员函数相同：

![image-20230226170213158](/home/alivn/Documents/算法与面试/images/Blog/C++PrimerPlus/image-20230226170213158.png)

然后我们在使用的时候可以这么写：

```c++
int main(){
    Time adjusted = t1 	* 1.5; //等价于operator*(1.5);
    Time adjusted = 1.5 * t1; // vaild! 等价于：Time adjusted = operator*(1.5 ,t1);
}
```

友元机制虽然允许非成员函数访问私有数据，但是并不违反了OOP数据隐藏的原则，应将友元函数看作类的扩展接口的组成部分。

## 常用的友元：`<<`重载

一个很有用的类特性是，可以对`<<`运算符进行重载，使之能与 `cout` 一起来显示对象的内容。

对于一个学生类，我们想显示学生对象的信息，一般这么写：

![image-20230226172348059](/home/alivn/Documents/算法与面试/images/Blog/C++PrimerPlus/image-20230226172348059.png)

然后在调用时直接调用`Show()`函数即可；

但是如果使用 `<< `运算符重载则会方便很多，我们可以直接使用`cout` 打印对象:

```cpp
Student stu1("李华", 21, 98);
cout << stu1;
```

如何重载？`cout` 是一个 `ostream` 类的对象，要使得`Student` 类知道使用`cout` ，因此可以使用使用一个`Student` 成员函数来重载`<<`，`Student` 对象将是第一个操作数，就像使用成员函数重载 `*` 运算符那样。这意味着必须这样使用 `<<`：

```c++
stu1 << cout;
```

这样会令人迷惑。但通过使用友元函数，可以像下面这样重载运算符：

```c++
ostream & operator<<(ostream &os, const Student & stu)
{
    os << "name:" << stu.name 
        << " age: " << stu.age 
        << " score: " << stu.score << endl;
    return os;
}
```

此时就可以使用 `cout << stu1;` 打印对象的数据；另外，该非成员函数返回的是 `ostream` 类型，这样做的好处是可以这样打印：

```c++
cout << "The First Student is" << stu1 << "." << endl;
```

## 重载使用成员or非成员函数

于很多运算符来说，可以选择使用成员函数或非成员函数来实现运算符重载。一般来说，**非成员函数应是友元函数**，这样它才能直接访问类的私有数据。

加法运算符需要两个操作数。对于成员函数版本来说，一个操作数通过`this`指针隐式地传递，另一个操作数作为函数参数显式地传递；对于友元版本来说，两个操作数都作为参数来传递。

# 类和动态内存分配

>  背景：假设要创建一个类，表示一个姓名，开始使用14个字符的数组，后面发现数组太小，更保险的方法是，使用一个40个字符的数组。但是如果创建多个对象时，就会由于字符数组只有部分被使用而浪费大量的内存。

因此，最好是在程序运行时（而不是编译时）确定诸如使用多少内存等问题。通常的C++方法是，在类构造
函数中使用 `new` 运算符在程序运行时分配所需的内存，并且，必须在相应的析构函数中使用 `delete` 来释放内存。如果使用 `new[]`（包括中括号）来分配内存，则应使用 `delete[]` （包括中括号）来释放内存。

## String案例

- `string.h`

```cpp
#include <iostream>
using namespace std;

#ifndef STRINT_H_
#define STRINT_H_

class String
{
private:
    char *str;                    // 指向存储str数组的指针
    int len;                      // 字符串长度
    static int num_strings;       // 创建的对象个数
    static const int CINLIM = 80; // 输入字符串长度限制
    // 等价于枚举类型： enum {CINLIM = 80} 
public:
    String(const char *s);             // 构造函数
    String();                          // 默认构造函数
    String(const String &sb);          // 复制构造函数
    ~String();                         // 析构函数
    int length() const { return len; } // 内联函数，返回字符串长度

    // 定义友元函数（非成员函数）重载相关运算符
    friend bool operator<(const String &st1, const String &st2);
    friend bool operator>(const String &st1, const String &st2);
    friend bool operator==(const String &st1, const String &st2);
    friend ostream &operator<<(ostream &os, const String &st); // 用于重载<<打印对象信息
    friend istream &operator>>(istream &is, String &st); // 用于重载>>打印对象信息

    // 重载=运算符
    String &operator=(const String &st);
    String &operator=(const char *s);

    char &operator[](int i);             // 重载[] 实现取值
    const char &operator[](int i) const; // 重载[] 实现取值，只读

    // 创建一个静态成员函数，用来表示创建对象的个数
    static int HowMany();
};

#endif
```

- `string.cpp`

```cpp
#include "string.hpp"
#include <cstring>

// 初始化静态类成员
int String::num_strings = 0;

// 构造函数
String::String(const char *s)
{
    len = strlen(s);         // 获取字符串size
    str = new char[len + 1]; // 开辟恰好的内存（包括末尾的空字符，故而+1）
    strcpy(str, s);          // 将传递的字符串s复制到新的内存中
    num_strings++;           // 每创建一个对象，统计对象个数++
}
// 默认构造函数
String::String()
{
    len = 0;
    str = new char[1];
    strcpy(str, "\0");
    num_strings++;
}
// 复制构造函数
String::String(const String &sb)
{
    len = sb.len;
    str = new char[len + 1];
    strcpy(str, sb.str);
    num_strings++;
}
// 析构函数(必须要)
String::~String()
{
    --num_strings;
    delete[] str;
}
/****************其他方法在下文介绍中提到******************/
```

**为什么使用` strcpy(str, s);  ` 而不是 `str = s;` ?**

`str` 和 `s` 都是指针类型，它们存储的是内存地址。`char *str;` 和 `char *s;`声明了两个指向字符的指针变量`str`和`s` ；`strcpy(str, s)`是将`s`指向的字符串复制到`str`指向的字符串中，`str = s`将`s`指针的值赋值给`str`指针，这只保存了地址，而没有创建字符串副本。

如果要使得后者有与前者相同的效果，则需要重载 `=` 运算符了；

## 静态成员变量

**为什么声明静态成员变量？`static int num_strings;`**

静态类成员有一个特点：**无论创建了多少对象，程序都只创建一个静态类变量副本**。也就是说，类的所有对象共享同一个静态成员，就像家中的电话可供全体家庭成员共享一样。

另外需要注意，将静态成员 `num_strings` 的值初始化时，**不能在类声明中初始化静态成员变量**，这是因为声明描述了如何分配内存，但并不分配内存。对于静态类成员，可以在类声明之外使用单独的语句来进行初始化，这是因为**静态类成员是单独存储**的，而不是对象的组成部分。

初始化是在**方法文件(`.cpp`)**中，如：`int String::num_strings = 0;`，而不是在类声明文件(`.h`)中进行的，这是因为类声明位于头文件中，程序可能将头文件包括在其他几个文件中。如果在头文件中进行初始化，将出现多个初始化语句副本，从而引发错误。

例外的情况是在类中声明一个常量时，可以使用`static`或者枚举类型，此时可以在在类声明中初始化。

## 复制构造函数

**按值传递和按引用传递区别：**

按值传递实质上是拷贝现有的对象，然后生成副本，类似于如下代码：

```c++
StringBad bad1 = bad2;//等价于下面：
StringBad bad1 = StringBad(bad2);
```

显然这里调用的不是默认构造函数，而是如下的构造函数（即为**复制构造函数**）：

```c++
// Class_name(const Class_name &);
StringBad(const StringBad & sb);
```

按值传递意味着创建原始变量的一个副本。编译器生成临时对象时，也将使用**复制构造函数**。当您使用一个对象来初始化另一个对象时，编译器将自动生成上述构造函数（称为复制构造函数，因为它创建对象的一个副本）。自动生成的构造函数不知道需要更新静态变量 `num_string`，因此会将计数方案搞乱。

特殊成员函数：

具体地说，C++**自动提供**了下面这些成员函数：

- 默认构造函数，如果没有定义构造函数；
- 默认析构函数，如果没有定义；
- 复制构造函数，如果没有定义；
- 赋值运算符，如果没有定义；
- 地址运算符，如果没有定义。

什么时候调用复制构造函数呢？

**新建一个对象并将其初始化为同类现有对象时，复制构造函数都将被调用。**如下代码，都会调用复制构造函数。

```c++
StringBad ditto(motto);
StringBad metoo = motto;
StringBad also = Stringbad(motto);
StringBad *pStringbad = new Stringbad(motto);
```

**复制构造函数的功能：**

默认的复制构造函数逐个复制非静态成员（成员复制也称为浅复制），复制的是成员的值：

```c++
StringBad metoo = sports;
// 等价于如下代码
Stringbad sailor;
sailor.str = sports.str;
sailor.len = sports.len;
```

因此，解决按值传递出现的问题，就需要自定义一个复制构造函数，来更新静态变量 `num_string`；

```c++
StringBad::StringBad(const StringBad & sb)
{
    len = sb.len;
    str = new char[len + 1];
    strcpy(str, sb.str);
    num_strings++;
    cout << "复制构造函数：" << num_strings << ":\"" << str << "\" object created!\n"; 
}
```

## 重载赋值运算符

潜在的风险：

```c++
// 如下的复制构造函数
StringBad knot = headline1;	
// 等价于:
StringBad knot = StringBad(headline1);	
```

可以这么解读：实现时也可能分两步来处理这条语句，①使用复制构造函数创建一个临时对象，②然后通过赋值将临时对象的值复制到新对象中。这就是说，初始化**总是会调用复制构造函数**，而使用 `=` 运算符时也**可能调用赋值运算符**。如果用户不提供则C++会调用自己提供的构造函数。由于默认赋值运算符不合适而导致的问题，解决办法是提供赋值运算符（进行深度复制）定义。

- 由于目标对象可能引用了以前分配的数据，所以函数应使用 `delete[]` 来释放这些数据。
- 函数应当避免将对象赋给自身；否则，给对象重新赋值前，释放内存操作可能删除对象的内容。
- 函数返回一个指向调用对象的引用。

因此，需要对赋值运算符进行重载：

```c++
// 重载赋值运算符
StringBad & StringBad::operator=(const StringBad & st)
{
    // 1.检查是否为自我复制
    if (this == &st)
        return *this;
    // 2.释放旧的string
    delete[] str;       
    // 3.复制数据而不仅仅是数据的地址
    len = st.len;
    str = new char[len + 1];
    strcpy(str, st.str);
    // 4.返回一个指向调用对象的引用
    return *this;
}
```

> **复制构造函数**用于**在对象创建时进行对象的复制**，而**重载赋值运算符**则用于对象已经存在时的复制操作。两者都是为了实现对象的深拷贝，以避免浅拷贝带来的问题。
>
> 同时使用复制构造函数和重载赋值运算符可以保证对象的复制在任何情况下都能够正确进行，避免了出现对象复制问题的可能。



## 有关返回对象的说明

### 返回指向`const`对象的引用

使用const引用的常见原因是旨在提高效率，但对于何时可以采用这种方式存在一些限制。如果函数返回（通过调用对象的方法或将对象作为参数）传递给它的对象，可以通过返回引用来提高其效率。

举一个例子，编写函数Max()，它返回两个Vector对象中较大的一个

```c++
Vector force1(50,60);
Vector force2(10,70);
Vector max = Max(force1, force2);
// Version1
Vector Max(const Vector & v1, const Vector & v2)
{
    if(v1.magval() > v2.magval())
    	return v1;
    else 
        return v2;
}
// Version2
const Vector & Max(const Vector & v1, const Vector & v2)
{
    if(v1.magval() > v2.magval())
    	return v1;
    else 
        return v2;
}
```

- `Version1` 返回对象将调用复制构造函数，而 `Version2` 返回引用不会，因此，`Version2` 所做的工作更少，效率更高；
- 引用指向的对象应该在调用函数执行时存在（不能是临时对象），显然，引用指向 `force1` 或 `force2`，它们都是在调用函数中定义的，因此满足这种条件;
- `v1` 和 `v2` 都被声明为 `const` 引用，因此返回类型必须为 `const`，这样才匹配。 

### 返回指向非`const`对象的引用

两种常见的返回非 `const` 对象情形是，重载赋值运算符以及重载与 `cout`一起使用的 `<<` 运算符。前者这样做旨在提高效率，而后者必须这样做。

```c++
String s1("Good stuff");
String s2, s3;
s3 = s2 = s1;
```

在这个例子中，返回类型不是`const`，因为方法`operator=()`返回一个指向`s2`的引用，可以对其进行修改。

`operator<<()`的返回值用于串接输出：

```c++
String s1("Good stuff");
cout << s1 << "is coming!";
```

`ostream` 没有公有的复制构造函数，因此返回类型不能是`ostream`，而必须是`ostream &`，因为返回对象将调用复制构造函数，而返回对象的引用则不会。

### 返回对象

如果被返回的对象是被调用函数中的**局部变量**，则不应按引用方式返回它，因为在被调用函数执行完毕时，局部对象将调用其析构函数。因此，当控制权回到调用函数时，引用指向的对象将不再存在。在这种情况下，应返回对象而不是引用。通常，被重载的算术运算符属于这一类。

```c++
Vector force1(50,60);
Vector force2(10,70);
Vector net = force1 + force2;
```

返回的不是`force1`，也不是`force2`，`force1`和`force2`在这个过程中应该保持不变。

在`Vector::operator+( )`中计算得到的两个矢量的和被存储在一个新的临时对象中，该函数也不应返回指向该临时对象的引用，而应该返回实际的`Vector`对象；

```c++
Vector Vector::operator+(const Vector &b) const
{
    return Vector(x + b.x, y + b.y);
}
```

在这种情况下，存在调用复制构造函数来创建被返回的对象的开销，然而这是**无法避免**的；

### 返回`const`对象

上述代码返回的是对象，可能会被修改或者滥用，为了避免这种情况的产生，可以返回`const` 对象：

```C++
const Vector Vector::operator+(const Vector &b) const
{
    return Vector(x + b.x, y + b.y);
}
```

则此时，`Vector net = force1 + force2;` 中的`net` 后续则不会被修改；

## 指向对象的指针

1. 声明指向类对象的指针：

   ```c++
   String * glamour;
   ```

2. 将指针初始化为已有对象：

   ```c++
   String *first = &sayings[0];
   ```

3. 使用 `new` 和默认构造函数对指针进行初始化：

   ```c++
   String *gleep = new String;
   ```

4. 使用 `new` 和 `String(const char*)` 类构造函数对指针进行初始化：

   ```c++
   String *glop = new String("my my my");
   ```

5. 使用 `new` 和 `String(const string&)` 类构造函数对指针进行初始化：

   ```c++
   String *glop = new String(sayings[0]);
   ```

6. 使用 `->` 操作符通过指针访问类方法：

   ```c++
   if(sayings[i].length < shortest->length())
   ```

7. 使用 `*` 解除引用操作符从指针获得对象：

   ```c++
   if(sayings[i] < *first) // 比较对象之间的大小
   ```

## 定位new运算符

```c++
#include <iostream>
using namespace std;
const int BUF = 512;

class JustTesting
{
private:
    string words;
    int number;

public:
    JustTesting(const string &s = "Just Testing", int n = 0)
    {
        words = s;
        number = n;
        cout << words << " constructed\n";
    }
    ~JustTesting()
    {
        cout << words << " destoryed\n";
    }
    void Show() const
    {
        cout << words << ", " << number << endl;
    }
};

int main()
{
    char *buffer = new char[BUF]; // 创建一块512的内存区

    JustTesting *pc1, *pc2;

    pc1 = new (buffer) JustTesting; // 将对象放置缓存区
    pc2 = new JustTesting("Heap1", 20);

    cout << "Memory block addresses: \n"
         << "buffer: " << (void *)buffer
         << "  heap: " << pc2 << endl;
    cout << pc1 << ": ";
    pc1->Show();
    cout << pc2 << ": ";
    pc2->Show();

    JustTesting *pc3, *pc4;
    // 定位new运算符使用一个新对象来覆盖用于pc1的内存单元
    pc3 = new (buffer) JustTesting("Bad Idea", 6);
    pc4 = new JustTesting("Heap2", 10);
    cout << "Memory contents: \n";
    cout << pc3 << ": ";
    pc3->Show();
    cout << pc4 << ": ";
    pc4->Show();
	
    // delete用于pc2和pc4时，将自动调用为pc2和pc4指向的对象调用析构函数
    delete pc2;
    delete pc4;
    // 将delete[]用于buffer时，不会为使用定位new运算符创建的对象调用析构函数
    delete[] buffer;
    return 0;
}
======打印结果========
Just Testing constructed
Heap1 constructed
Memory block addresses: 
buffer: 0x561b5c114eb0  heap: 0x561b5c1160d0
0x561b5c114eb0: Just Testing, 0
0x561b5c1160d0: Heap1, 20
Bad Idea constructed
Heap2 constructed
Memory contents: 
0x561b5c114eb0: Bad Idea, 6
0x561b5c116100: Heap2, 10
Heap1 destoryed
Heap2 destoryed
```

1. 为了确保这两个内存单元不重叠，程序员需要提供两个位于缓冲区的不同地址：

   ```Diff
   -pc3 = new (buffer) JustTesting("Bad Idea", 6);
   +pc3 = new (buffer+sizeof(JustTesting)) JustTesting("Bad Idea", 6);
   ```

   这里让对象存放在buffer偏移正好一个对象大小的内存区，这样就不会与`pc1`重叠；

2. **如果使用定位 `new` 运算符来为对象分配内存，必须确保其析构函数被调用：**

   **注意：**不可以直接`delete pc1;` 或 `delete pc3;`原因在于 `delete` 可与常规`new` 运算符配合使用，但不能与定位 `new` 运算符配合使用。指针 `pc1` 指向的地址与 `buffer` 相同，但 `buffer` 是使用 `new []` 初始化的，因此必须使用 `delete []` 而不是 `delete` 来释放。

   ```c++
   delete[] buffer;
   ```

   该程序确实释放了`buffer`，但是但它没有为定位 `new` 运算符在该内存块中创建的对象调用析构函数，这种问题的解决方案是，**显式地为使用定位 `new` 运算符创建的对象调用析构函数**：

   ```diff
   + pc3->~JustTesting();
   + pc1->~JustTesting();
   delete[] buffer;
   ```

   ## 成员初始化列表

   ```c++
   class Example {
   private:
       int m_number;
       string m_name;
       const int m_value;
   public:
       Example();
       Example(int value);
       ~Example();
   };
   Example::Example(int value)
   {
       m_number = 0;
       m_name = "C++";
       m_value = value; // 报错：表达式必须是可修改的左值
   }
   ```

   对于类成员中的 `const` 常量，由于 `const` 常量一旦初始化后就不能再修改，必须在执行到构造函数体之前，即创建对象时进行初始化。C++提供了一种特殊的语法来完成上述工作，它叫做成员初始化列表：

   ```diff
   -Example::Example(int value)
   +Example::Example(int value):m_value(value)
   {
       m_number = 0;
       m_name = "C++";
   -   m_value = value; // 报错：表达式必须是可修改的左值
   }
   ```

   另外，对于被声明为引用的类成员，也必须使用这种语法：

   ```c++
   class Example {
   public:
       Example(int& value): m_value(value) {} // m_value 为 reference 引用，必须使用成员初始化列表
   private:
       int& m_value;
   };
   ```

   这是因为引用与`const`数据类似，只能在被创建时进行初始化。

成员初始化列表的语法：
如果 `Classy` 是一个类，而 `mem1、mem2` 和 `mem3` 都是这个类的数据成员，则类构造函数可以使用如下的语法来初始化数据成员：

```c++
Classy::Classy(int n, int m) : mem1(n),  mem2(0),  mem3(n*m + 2){}
```

成员初始化列表注意事项：

- 这种格式只能用于构造函数；
- 必须用这种格式来初始化非静态 `const` 数据成员（至少在C++11之前是这样的）；
- 必须用这种格式来初始化引用数据成员。

# 类继承

面向对象编程的主要目的之一是提供**可重用**的代码。开发新项目，尤其是当项目十分庞大时，重用经过测试的代码比重新编写代码要好得多。C++提供了比修改代码更好的方法来扩展和修改类。这种方法叫作**类继承**，它能够从已有的类派生出新的类，而**派生类**不仅继承了原有类（称为**基类**）的特征，包括方法，还拥有自己的特性。

<img src="./images/Blog/C++PrimerPlus/image-20230304094953711.png" alt="image-20230304094953711" style="zoom:50%;" />

> 我们发现，定义这些类时，下级别的成员除了拥有上一级的共性，还有自己的特性。这个时候我们就可以考虑利用继承的技术，减少重复代码

**定义：**

```c++
#ifndef TABTENN0_H_
#define TABTENN0_H_

#include <string>
#include <iostream>
using namespace std;
// 基类：表示某个会员是否有球桌
class TableTennisPlayer
{
private:
    string firstname;
    string lastname;
    bool hasTable;

public:
    // 默认构造函数
    TableTennisPlayer(const string &fn = "none", const string &la = "none", bool ht = false);
    void Name() const;
    bool HasTable() const { return hasTable; }
    void ResetTable(bool v) { hasTable = v; }
};
// 派生类：表示每个会员的比分
class RatedPlayer : public TableTennisPlayer
{
private:
    unsigned int rating;

public:
    RatedPlayer(unsigned int r = 0, const string &fn = "none", const string &la = "none", bool ht = false);
    RatedPlayer(unsigned int r, const TableTennisPlayer & tp);
    unsigned int Rating() const { return rating; }
    void ResetRating(unsigned int r){ rating = r; }
};
#endif
```

冒号指出 `RatedPlayer` 类的基类是 `TableTennisplayer` 类。上述特殊的声明头表明 `TableTennisPlayer` 是一个**公有基类**，这被称为**公有派生**。**派生类对象包含基类对象**。使用公有派生，基类的公有成员将成为派生类的公有成员；基类的私有部分也将成为派生类的一部分，但只能通过基类的公有和保护方法访问（稍后将介绍保护员）。

- 派生类对象存储了基类的数据成员（派生类继承了基类的实现）；
- 派生类对象可以使用基类的方法（派生类继承了基类的接口）。
- 派生类需要自己的构造函数。
- 派生类可以根据需要添加额外的数据成员和成员函数。

**构造函数必须给新成员（如果有的话）和继承的成员提供数据**。在第一个 `RatedPlayer` 构造函数中，每个成员对应一个形参；而第二个 `Ratedplayer` 构造函数使用一个` TableTennisPlayer` 参数，该参数包括 `firstname、lastname` 和 `hasTable`。

## 构造函数

派生类不能直接访问基类的私有成员，而必须通过基类方法进行访问。具体地说，**派生类构造函数必须使用基类构造函数**。创建派生类对象时，程序首先创建基类对象。这意味着**基类对象应当在程序进入派生类构造函数之前被创建**。C++使用成员**初始化列表**语法来完成这种工作。

```c++
RatedPlayer::RatedPlayer(unsigned int r, const string &fn, const string &ln, bool ht) : TableTennisPlayer(fn, ln, ht)
{
	rating = r;
}
```

其中：`TableTennisPlayer(fn,ln,ht)` 是成员初始化列表。它是可执行的代码，调用 `TableTennisPlayer` 构造函数。

事实上，**如果省略成员初始化列表，C++将使用默认的基类构造函数**，因为必须首先创建基类对象，调用基类构造函数，省略后等价于：

```c++
RatedPlayer::RatedPlayer(unsigned int r, const string &fn, const string &ln, bool ht) : TableTennisPlayer() // 调用基类的默认构造函数
{    
    rating = r;
}
```

因此：**除非要使用默认构造函数，否则应显式调用正确的基类构造函数。**

- 创建派生类对象时，程序首先调用基类构造函数，然后再调用派生类构造函数。

- 基类构造函数负责初始化继承的数据成员；派生类构造函数主要用于初始化新增的数据成员。

- 派生类的构造函数总是调用一个基类构造函数。可以使用初始化器列表语法指明要使用的基类构造函
  数，否则将使用默认的基类构造函数。

- 派生类对象过期时，程序将首先调用派生类析构函数，然后再调用基类析构函数。

**实现：**

```c++
#include "tabtenn1.h"
// 基类的默认构造函数
TableTennisPlayer::TableTennisPlayer(const string &fn, const string &ln, bool ht)
{
	firstname = fn;
	lastname = ln;
	hasTable = ht;
}
// 基类的方法
void TableTennisPlayer::Name() const
{
	cout << lastname << ", " << firstname;
}
// 派生类的构造函数
RatedPlayer::RatedPlayer(unsigned int r, const string &fn, const string &ln, bool ht) : TableTennisPlayer(fn, ln, ht)
{
	rating = r;
}
// 派生类的构造函数
RatedPlayer::RatedPlayer(unsigned int r, const TableTennisPlayer &tp) : TableTennisPlayer(tp), rating(r) // 这里没有显示提供复制构造函数，将使用默认的复制构造函数
{
}
```

## 派生类和基类的关系

1. **派生类对象可以使用基类的非私有方法：**

   ```c++
   RatedPlayer rplayer1(1140, "Mallory", "Duck", false);
   rplayer1.Name();
   ```

2. **基类指针**可以在<u>不进行显式类型转换</u>的情况下**指向**派生类对象；
   **基类引用**可以在<u>不进行显式类型转换</u>的情况下**引用**派生类对象。

   ```c++
   RatedPlayer rplayer1(1140, "Mallory", "Duck", false);
   TableTennisPlayer & rt =  rplayer1;		// 基类引用引用派生类对象
   TableTennisPlayer * pt = &rplayer1;		// 基类指针指向派生类对象
   rt.Name();	 // 基类指针或引用只能用于调用基类方法
   pt->Name();
   ```

   并且：基类指针或引用只能用于调用基类方法，因此，不能使用 `rt` 或 `pt` 来调用派生类的 `ResetRanking` 方法。记住：派生类包括基类，只能用大范围指向小范围，而不能反过来。

   ```c++
   TableTennisPlayer player1("Tara", "Boomdea", false);
   RatedPlayer & rr =  player1; // error
   RatedPlayer * pr = &player1; // error
   ```

## 多态公有继承

对于继承而言，下级别的成员除了拥有上一级的共性，还有自己的特性。即**希望同一个方法在派生类和基类中的行为是不同的**。换句话来说，**方法的行为应取决于调用该方法的对象**。这种较复杂的行为称为多态——具有多种形态，即同一个方法的行为随上下文而异。有两种重要的机制可用于实现多态公有继承：

- 在派生类中重新定义基类的方法。
- 使用虚方法。

## 存取款机案例

定义一个 `Brass` 类，可以实现存款、取款、账号信息查询等，然后创建一个`BrassPlus` 类继承`Brass`类的公有接口，然后它还增加了信用卡可透支的功能。

`brass.h`

```c++
#ifndef __BRASS_H__
#define __BRASS_H__

#include <iostream>
#include <string>
using namespace std;

class Brass
{
private:
	string fullName; // 用户姓名
	long acctNum;	 // 账号
	double balance;	 // 结余

public:
	// 默认构造函数
	Brass(const string &s = "Nullbody", long an = -1, double bal = 0.0);
	void Deposit(double amt);		   // 存款
    double Balance() const;			   // 结余
	virtual void Withdraw(double amt); // 取款
	virtual void ViewAcct() const;	   // 显示账号信息
	virtual ~Brass() {}				   // 析构函数
};
// 派生类继承基类
class BrassPlus : public Brass
{
private:
	double maxLoan;	 // 最多可透支的上限
	double rate;	 // 利率
	double owesBank; // 欠银行总额

public:
	BrassPlus(const string &s = "Nullbody", long an = -1, double bal = 0.0, double ml = 500.0, double rate = 0.11125);
	BrassPlus(const Brass &ba, double ml = 500.0, double rate = 0.11125);
	virtual void Withdraw(double amt); // 取款
	virtual void ViewAcct() const;	   // 显示账号信息
};
#endif
```

`brass.cpp`

```cpp
#include "brass.h"
// 基类的默认构造函数
Brass::Brass(const string &s, long an, double bal)
{
	fullName = s;
	acctNum = an;
	balance = bal;
}
// 基类:存款
void Brass::Deposit(double amt)
{
	if (amt < 0)
		cout << "Negative deposit is not allowed!" << endl;
	else
		balance += amt;
}
// 基类:取款
void Brass::Withdraw(double amt)
{
	if (amt < 0)
		cout << "Withdraw amount must be positive." << endl;
	else if (amt <= balance)
		balance -= amt;
	else
		cout << "Withdraw amount exceeded your balance!" << endl;
}

void Brass::ViewAcct() const
{
	cout << "Client: " << fullName << endl;
	cout << "Account number: " << acctNum << endl;
	cout << "Balance: $" << balance << endl;
}
// 派生类的默认构造函数
BrassPlus::BrassPlus(const string &s, long an, double bal, double ml, double r) : Brass(s, an, bal)
{
	maxLoan = ml;
	rate = r;
	owesBank = 0.0;
}

BrassPlus::BrassPlus(const Brass &ba, double ml, double r) : Brass(ba)
{
	maxLoan = ml;
	rate = r;
	owesBank = 0.0;
}

void BrassPlus::ViewAcct() const
{
	Brass::ViewAcct();
	cout << "Maxium load: $" << maxLoan << endl;
	cout << "Loan Rate: " << rate << endl;
	cout << "Owed to bank: $" << owesBank << endl;
}

void BrassPlus::Withdraw(double amt)
{
	double bal = Balance();
	if (amt <= bal) // 小于账户余额
		Brass::Withdraw(amt);
	else if (amt <= bal + maxLoan - owesBank) // 小于可透支余额
	{
		double advance = amt - bal;		   // 可透支金额
		owesBank = advance * (1.0 + rate); // 欠银行总额
		cout << "Bank Advance: $" << advance << endl;
		cout << "Finance charge: $" << advance * rate << endl;
		Deposit(advance);	  // 将透支的金额“存入”账户（相当于银行帮你存进去一部分，借给你）
		Brass::Withdraw(amt); // 取钱
	}
	else
		cout << "Credit limit exceeded!" << endl;
}
```

### 使用虚函数

`Brass` 类和 `BrassPlus` 类都声明了 `ViewAcct( )` 和 `Withdraw( )` 方法，但 `BrassPlus` 对象和 `Brass` 对象的这些方法的行为是不同的；因此，在这两个方法欠使用了关键字 `virtual`，用于将这些方法被称为**虚方法(virtual method)**;

因此，经常在基类中**将派生类会重新定义的方法声明为虚方法**。方法在基类中被声明为虚的后，它在派生类中将自动成为虚方法。然而，在派生类声明中使用关键字 `virtual` 来指出哪些函数是虚函数也不失为一个好办法。

将基类方法声明为虚的，程序将**根据对象类型而不是引用或指针的类型来选择方法版本**。例如：可以创建指向 `Brass` 的指针数组。这样，每个元素的类型都相同，但由于使用的是公有继承模型，因此 `Brass` 指针既可以指向 `Brass` 对象，也可以指向 `BrassPlus` 对象

```c++
//以下代码仅仅为了演示，无法直接运行
Brass *p_clients[CLIENTS];	// 客户数组
for(int i = 0; i < CLIENTS; i++)
{
    if(kind == 1)
        p_clients[i] = new Brass(temp, tempnum, tempbal);
    else
    {
        double tmax, trate;
        cout << "Enter the overdraft limit: $";
        cin >> tmax;
        cout << "Enter the rate: ";
        cin >> trate;
        p_clients[i] = new BrassPlus(temp, tempnum, tempbal, tmax, trate);
    }
}
```

显然`*p_clients` 的类型为 `Brass`，但是它可以用来指向基类也可以指向派生类，而当调用对象的方法时，将根据对象类型调用相应的方法，这样更便于管理基类和派生类对象，这都得益于虚函数的作用。

```cpp
for(int i = 0; i < CLIENTS; i++)
{
    p_clients[i]->ViewAcct(); // 可能是基类的ViewAcct，也可能是派生类的
    cout << endl;
}
```

## 虚函数和动态绑定

**静态绑定（static binding）：**又称为早期联编（early binding），是指编译器**在编译时就能够确定调用哪个函数**，通常使用函数名和参数类型来确定。在静态联编的情况下，函数调用过程的开销比较小，但是无法实现运行时多态性。

**动态绑定（dynamic binding）：**又称为晚期联编（late binding），是指**在运行时确定调用哪个函数**，通常使用**虚函数**来实现。在动态联编的情况下，函数调用过程的开销比较大，但是可以实现运行时多态性。

为使程序能够在运行阶段进行决策，**必须采取一些方法来跟踪基类指针或引用指向的对象类型**，这增加了额外的处理开销（稍后将介绍一种动态联编方法）。例如，如果类不会用作基类，则不需要动态联编。同样，如果派生类（如RatedPlayer）不重新定义基类的任何方法，也不需要使用动态联编。在这些情况下，使用静态联编更合理，效率也更高。**因此，如果要在派生类中重新定义基类的方法，则将它设置为虚方法；否则，设置为非虚方法。**

### 虚函数工作原理

C++中的虚函数是实现多态性（Polymorphism）的重要机制，它的原理可以简单地概括为“动态绑定”（Dynamic Binding）或“后期绑定”（Late Binding），即在运行时根据实际对象的类型来决定调用哪个函数。

在C++中，虚函数通过在函数声明前面加上“virtual”关键字来声明。在一个类中，如果一个函数被声明为虚函数，在类的继承体系中，所有派生类都可以重写（Override）这个虚函数。当**一个函数被声明为虚函数时，编译器会在对象的内存布局中添加一个指向虚函数表（Virtual Function Table，简称VTable）的指针，这个指针指向了存储了该类所有虚函数地址的表。**

在调用虚函数时，编译器会**根据对象的实际类型来访问虚函数表，从而决定调用哪个虚函数**。当一个虚函数被重写时，子类会覆盖父类的虚函数表中相应的虚函数地址，从而实现了多态性。

需要注意的是，C++中的虚函数**仅适用于通过指针或引用访问对象的情况**，如果直接使用对象来调用虚函数，则编译器会在编译期决定调用哪个函数，这称为“静态绑定”（Static Binding）或“早期绑定”（Early Binding）。

总之，C++中的虚函数通过动态绑定实现了多态性，它的实现依赖于虚函数表和指向虚函数表的指针，同时也使得C++具有了很强的灵活性和可扩展性。

但是，使用虚函数时，在内存和执行速度方面有一定的成本，包括：

- 每个对象都将增大，增大量为存储地址的空间；
- 对于每个类，编译器都创建一个虚函数地址表（数组）；
- 对于每个函数调用，都需要执行一项额外的操作，即到表中查找地址。

### 虚函数的注意事项

1. **构造函数不可以是虚函数**

2. **析构函数应当是虚函数，除非类不用做基类**。如果基类的析构函数不是虚函数，当我们通过基类指针或引用调用delete运算符时，只会调用基类的析构函数，而不会调用派生类的析构函数。这可能导致由派生类的成员变量分配的内存泄漏。

   ```c++
   class Employee
   {
   public:
       Employee() {cout << "Employee constructor called." << endl;}
       ~Employee(){cout << "Employee destructor called." << endl;}
       virtual void showInfo(){cout << "I am an employee." << endl;}
   };
   
   class Singer : public Employee
   {
   private:
       char *name;
   public:
       Singer(const char *p = "")
       {
           name = new char[strlen(p) + 1];
           strcpy(name, p);
           cout << "Singer constructor called." << endl;
       }
       virtual ~Singer()
       {
           delete[] name;
           cout << "Singer destructor called." << endl;
       }
       virtual void showInfo(){cout << "I am a singer, my name is " << name << endl;}
   };
   
   int main()
   {
       Employee *emp = new Singer("Taylor Swift");	// 使用基类指针指向派生类对象
       emp->showInfo();
       delete emp;			// 调用析构函数
       return 0;
   }
   ```

   上述代码，基类的析构函数并没有声明为虚析构函数，打印结果如下：

   ```apl
   Employee constructor called.
   Singer constructor called.
   I am a singer, my name is Taylor Swift
   Employee destructor called.
   ```

   因此：当我们通过基类指针或引用调用delete运算符时，只会调用基类的析构函数，而不会调用派生类的析构函数。这样会导致派生类中开辟的内存无法得到释放，从而造成内存泄露，正确应该将基类中的析构函数声明为虚析构函数：

   ```diff
   -~Employee(){cout << "Employee destructor called." << endl;}
   + virtual ~Employee(){cout << "Employee destructor called." << endl;}
   ```

   打印结果如下：

   ```apl
   Employee constructor called.
   Singer constructor called.
   I am a singer, my name is Taylor Swift
   Singer destructor called.
   Employee destructor called.
   ```

3. **友元不能是虚函数**，因为友元不是类成员，而只有成员才能是虚函数。

4. **重新定义将隐藏方法**

   重新定义继承的方法并不是重载。如果在派生类中重新定义函数，将不是使用相同的函数特征标覆盖基类声明，而是**隐藏同名的基类方法**，不管参数特征标如何。

   ```c++
   #include <iostream>
   using namespace std;
   
   class Dwelling
   {
   public:
       virtual void showperks(int a) const { cout << "Dwelling: int a" << endl; }
       virtual void showperks(double x) const { cout << "Dwelling: double x" << endl; }
       virtual void showperks() const { cout << "Dwelling: void" << endl; }
   };
   
   class Hovel : public Dwelling
   {
   public:
       // virtual void showperks(int a) const { cout << "Hovel: int a" << endl; }
       // virtual void showperks(double x) const { cout << "Hovel: double x" << endl; }
       virtual void showperks() const { cout << "Hovel: void" << endl; }
   };
   
   int main()
   {
       Hovel trump;
       trump.showperks();  // valid
       trump.showperks(2); // invalid
       return 0;
   }
   ```

   新定义将`showperks()` 定义为一个不接受任何参数的函数。重新定义不会生成基类的三个重载版本的函数，而是隐藏了接受 `int` 参数和 `double`参数的两个基类版本的函数。**如果需要保证含参的函数不被覆盖掉，则必须在派生类中重新定义所有的基类版本。**

上面我们说的是如果重新定义继承的方法，应确保与原来的原型完全相同，即参数、函数名、返回值等必须相同，但如果返回类型是基类引用或指针，则可以修改为指向派生类的引用或指针（这种例外是新出现的）。这种特性被称为**返回类型协变（covariance of return type）**，因为允许返回类型随类类型的变化而变化：

> **注意，这种例外只适用于返回值，而不适用于参数。**

```c++
#include <iostream>
using namespace std;

class Dwelling
{
public:
    virtual Dwelling & build(int n) const;

class Hovel : public Dwelling
{
public:
    virtual Hovel & build(int n) const;
};

int main()
{
    Hovel trump;
    trump.build(5);
    return 0;
}
```

## 抽象基类

以一个案例为例：

> 假设您正在开发一个图形程序，该程序会显示圆和椭圆等。圆是椭圆的一个特殊情况——长轴和短轴等长的椭圆。因此，所有的圆都是椭圆，可以从Ellipse类派生出Circle类。

```c++
class Ellips
{
private:
    double x;     // 圆心横坐标
    double y;     // 圆心纵坐标
    double a;     // 长半轴 [对圆来说仅需一个半径]
    double b;     // 短半轴 [对圆来说仅需一个半径]
    double angle; // 方向角（水平坐标轴与长轴之间的角度） [对圆来说没有意义]

public:
    // 移动椭圆
    void Move(int nx, int ny){ x = nx; y = ny; }       
    // 求椭圆面积   
    virtual double Area() const { return 3.14159 * a * b; }
    // 对椭圆进行旋转 [对圆来说没有意义]
    virtual void Rotate(double ang) { angle += ang; }
    // 对椭圆进行放缩 [对圆来说，很容易把圆变成椭圆]
    virtual void Scale(double sa, double sb) { a *= sa; b *= sb; }
};
class Circle : public Ellips
{
};
```

既然直接继承，很多变量是没必要的，那单独定义 `Circle`  反而更简单，但是这样就违背了 `Circle` 和 `Ellipse` 类有很多共同点的事实，效率变低了。这里就考虑，从 `Circle` 和 `Ellipse` 中抽象出来它们公共的特性并放在 `ABC` 类中，然后从`ABC` 类中派生出 `Circle` 和 `Ellipse`类。这里的`ABC` 类即：抽象基类（abstract base class，ABC）。

可以发现，对于这两个类，共同点是中心坐标， `Move()` 方法（对于这两个类是相同的），`Area()`方法（对于这两个类来说，是不同的），由于 `Area()` 方法，因为它没有包含必要的数据成员（长短轴或半径），无法在 ABC 类中实现，**C++通过使用纯虚函数（pure virtual function）提供未实现的函数**。纯虚函数声明的结尾处为 `=0`.

```c++
class BaseEllips
{
private:
    double x;     // 圆心横坐标
    double y;     // 圆心纵坐标

public:
    BaseEllips(double x0 = 0, double y0 = 0): x(x0), y(y0){}
    virtual ~BaseEllips(){}
    void Move(int nx, int ny){ x = nx; y = ny; }    // 移动椭圆
    virtual double Area() const = 0;                // 求椭圆面积（纯虚函数）  
};
```

如上代码，**当类声明中包含纯虚函数时，则不能创建该类的对象。**这里的理念是，包含纯虚函数的类只用作基类。**抽象基类必须至少包含一个纯虚函数**。原型中的 `=0` 使虚函数成为纯虚函数。因此，即使所有方法都可以在抽象基类中实现，但是为了让他称为真正的抽象基类，必须设置至少一个函数为纯虚函数。

ABC **要求具体派生类覆盖其纯虚函数——迫使派生类遵循ABC设置的接口规则**。这种模型在基于组件的编程模式中很常见，在这种情况下，使用ABC使得组件设计人员能够制定“接口约定”，这样确保了从 ABC 派生的所有组件都至少支持 ABC 指定的功能。

## 继承中的动态内存分配

- 派生类不适用 `new`

```c++
class baseDMA
{
private:
    char * label;
    int rating;
public:
    baseDMA(const char * l = "null", int r = 0);
    baseDMA(const baseDMA & rs);
    baseDMA & operator=(const baseDMA & rs);
    virtual ~baseDMA();
};

class lacksDMA : public baseDMA
{
private:
    char color[40];
};
```

由于派生类中没有使用动态内存分配，因此，派生类中不需要重新定义析构函数、复制构造函数以及重载复制运算符。但是当派生类使用 `new` 的话，如：

```c++
class hasDMA : public baseDMA
{
private:
    char * style;
};
```

在这种情况下，**必须为派生类定义显式析构函数、复制构造函数和赋值运算符**。下面依次考虑这些方法。

1. **析构函数**

派生类析构函数自动调用基类的析构函数，故其自身的职责是对派生类构造函数执行工作的进行清理。因此，`hasDMA` 析构函数必须释放指针 `style` 管理的内存，并依赖于 `baseDMA` 的析构函数来释放指针 `label` 管理的内存。

```c++
baseDMA::~baseDMA()
{
    delete[] label;
}
hasDMA::~hasDMA()
{
    delete[] style;
}
```

2. **复制构造函数**

`BaseDMA` 的复制构造函数遵循用于 `char` 数组的常规模式，即使用 `strlen()` 来获悉存储C-风格字符串所需的空间、分配足够的内存（**字符数加上存储空字符所需的1字节**）并使用函数 `strcpy()` 将原始字符串复制到目的地：

```cpp
// 基类复制构造函数
baseDMA::baseDMA(const baseDMA & rs)
{
    label = new char[strlen(rs.label) + 1];
    strcpy(label, rs.label);
    rating = rs.rating;
}
// 派生类复制构造函数
hasDMA::hasDMA(const hasDMA & hs) : baseDMA(hs)
{
	style = new char[strlen(hs.style) + 1];    
	strcpy(style, hs.style);
}
```

这里需要注意：派生类使用成员初始化列表的方式，将一个 `hasDMA` 引用传递给`baseDMA` 构造函数，但是基类中并没有参数类型为 `hasDMA` 引用的 `baseDMA` 构造函数，事实上这样是可以的。因为复制构造函数 `baseDMA` 有一个 `baseDMA` 引用参数，而**基类引用可以指向派生类型**。因此，`baseDMA` 复制构造函数将使用 `hasDMA` 参数的`baseDMA` 部分来构造新对象的 `baseDMA` 部分。

3. **重载赋值运算符**

```cpp
// 重载基类赋值运算符
baseDMA &baseDMA::operator=(const baseDMA & rs)
{
    if(this == &rs) 	// 1.检查是否为自我复制
        return *this;
    delete[] label;		// 2.释放旧的string	
    label = new char[strlen(rs.label) + 1];	 // 3.复制数据而不仅仅是数据的地址
    strcpy(label, rs.label);
    rating = rs.rating;
    return * this;		// 4.返回一个指向调用对象的引用
}
// 重载派生类类赋值运算符
hasDMA &hasDMA::operator=(const hasDMA & hs)
{
    if(this == &hs)
        return *this;
    delete[] style;
    style = new char[strlen(hs.style) + 1];
    strcpy(style, hs.style);
    return *this;
}
```

**当基类和派生类都采用动态内存分配时，派生类的析构函数、复制构造函数、赋值运算符都必须使用相应的基类方法来处理基类元素。**



```c++
#ifndef STUDENTC_H_
#define STUDENTC_H_

#include <iostream>
#include <string>
#include <valarray>

using namespace std;
class Student
{
private:
    typedef valarray<double> ArrayDb; // 为了简化，使用重定义
    string name;       // 姓名
    ArrayDb scores;    // 各科分数

public:
    Student() : name("NULL"), scores() {}
    explicit Student(const string & s) : name(s), scores() {}
    explicit Student(int n) : name("NULL"), scores(n) {}
    Student(const string & s, int n) : name(s), scores(n) {}
    Student(const string & s, const ArrayDb & a) : name(s), scores(a) {}
    Student(const char * str, const double * pd, int n) : name(str), scores(pd, n) {}
    ~Student();

    double Average() const;
    const string & Name() const;
    double & operator[](int i);
    double operator[](int i) const;

    friend istream & operator>>(istream & is, Student & stu);
    friend istream & getline(istream & is, Student & stu);

    friend ostream & operator<<(ostream & os, Student & stu);

};
```

**为什么使用`explicit`  关键词？**

```c++
Student stu1("Alvin", 10);
stu1 = 5;     
```

使用 `explicit` 防止单参数构造函数的隐式转换。上述代码，如果不使用 `explicit` ，则 `stu1 = 5;` 是合法的，相当于调用构造函数： `Student(int n):name("NULL"),scores(n){}` ， 用了`explicit` 后，编译器将认为上述赋值运算符是错误的。









## 私有继承

在声明一个派生类时将基类的继承方式指定为 `private` 的，称为**私有继承**，用私有继承方式建立的派生类称为**私有派生类**，其基类称为**私有基类**。

使用公有继承，基类的公有方法将成为派生类的公有方法。总之，派生类将继承基类的接口。使用私有继承，基类的公有方法将成为派生类的私有方法。

# 类模板

类模板的声明：

```cpp
template <class Type>
class Stack
{
	private:
		enum {MAX = 10};
		Type items[MAX];
		int top;
	public:
		Stack();
		bool isempty() const;
		bool isfull() const;
		bool push(Type &item);
		bool pop(Type &item);
};
```

函数模板的定义：

```cpp
template <class Type>
bool Stack<Type>::isfull() const
{
	return top == MAX;
}
```

使用模板类：

```cpp
Stack<int> kernels; // 使用 int 替换模板中所有的 Type
Stack<string> kernels; // 使用 string 替换模板中所有的 Type
```



隐式实例化

```cpp
#include <iostream>
using namespace std;

template<class T1, class T2>
class A
{
public:
    	void show();	
};
// 显式具体化：是特定类型（用于替换模板中的泛型）的定义
template<>
class A<int, int>
{
public:
    void show();	
};
// 部分具体化：部分指定类型
template<class T1>
class A<T1, int>
{
    public:
        void show();	
};

template<class T1, class T2>
void A<T1, T2>::show()
{
    cout << "use general definition" << endl;
}
// 显式具体化
void A<int, int>::show()
{
    cout << "use specialized definition" << endl;
}
// 部分具体化
template<class T1>
void A<T1, int>::show()
{
    cout << "use partial specialized definition" << endl;
}

// 显示实例化：虽然没有创建或提及类对象，编译器也将生成类声明
template class A<double, double>; // 显示实例化

int main()
{
    A<char, char> a1; // 隐式实例化：在需要对象的时候才会实例化
    a1.show();
    A<int, int> a2;
    a2.show();
    A<char, int> a3;
    a3.show();
}
```



