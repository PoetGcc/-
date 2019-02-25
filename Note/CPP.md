## Hello World

```cpp
#include <iostream>
using namespace std;
int main ()
{
	cout<< "Hello World !" <<endl;
	return 0;
}
```

**结果**

```xml
Hello World !
```

***

## SizeOf

```cpp
#include<iostream>
#include<string>
#include<limits>
using namespace std;

/* bool 基本类型大小 */
int main()
{
	// bool
	cout << "type:\t\t"   << "************size**************" << endl;
	cout << "bool:\t\t"   << "所占字节数:" << sizeof(bool);
	cout << "\t\t最大值:"  << (numeric_limits<bool>::max)();
	cout << "\t\t最小值:"  << (numeric_limits<bool>::min)()    << endl;

	// string
	cout << "string:\t\t" << "所占字节数:" << sizeof(string)    << endl;
	return 0;
}
```

**结果**

```xml
type:		************size**************
bool:		所占字节数:1		最大值:1		最小值:0
string:		所占字节数:24
```

***

## Static

```cpp
#include<iostream>

/* 声明函数 */
void func(void);

/* 全局变量 */
static int count = 10;

int main(int argc, char const *argv[])
{
	// 存储类
	while(count--)
	{
		func();
	}
	return 0;
}

/* 函数定义 */
void func(void)
{
	static int i = 5; // 局部静态变量
	i++;
	std::cout << "变量 i 为\t" << i;
	std::cout << " , 变量 count 为\t" << count << std::endl; 
}
```

**结果**

```xml
变量 i 为	6 , 变量 count 为	9
变量 i 为	7 , 变量 count 为	8
变量 i 为	8 , 变量 count 为	7
变量 i 为	9 , 变量 count 为	6
变量 i 为	10 , 变量 count 为	5
变量 i 为	11 , 变量 count 为	4
变量 i 为	12 , 变量 count 为	3
变量 i 为	13 , 变量 count 为	2
变量 i 为	14 , 变量 count 为	1
变量 i 为	15 , 变量 count 为	0
```
***

## 结构体 Struct

```cpp
#include<iostream>
#include<cstring>
using namespace std;

/* 声明结构体 Book */
struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int  book_id;
};

/* 声明输出 ：结构体为参数 */
void printBook(struct Book book);

/* 声明输出 ：结构体指针为参数 */
void printBook(struct Book *book);

int main()
{
	// 定义
	Book java;
	Book php;

	// java 
	strcpy(java.title,"Thinking in Java");
	strcpy(java.author,"高司令");
	strcpy(java.subject,"编程语言");
	java.book_id = 101;

	// php 
	strcpy(php.title,"深入理解PHP7内核");
	strcpy(php.author,"鸟哥");
	strcpy(php.subject,"编程技术");
	php.book_id = 103;

	// 输出
	printBook(java);
	printBook(php);

	// 通过 &地址
	printBook(&java);
	printBook(&php);
	return 0;
}

/* 打印 : 结构体参数*/
void printBook(struct Book book)
{
	cout << "结构体为参数方式" << endl;
	cout << "书名: " << book.title << endl;
	cout << "作者: " << book.author << endl;
	cout << "类目: " << book.subject << endl;
	cout << "书ID: " << book.book_id << endl;
	cout << endl;
}

/* 打印 : 结构体指针参数 */
void printBook(struct Book *book)
{
	cout << "结构体指针为参数方式" << endl;
	cout << "书名: " << book->title << endl;
	cout << "作者: " << book->author << endl;
	cout << "类目: " << book->subject << endl;
	cout << "书ID: " << book->book_id << endl;
	cout << endl;
}
```

**结果**

```xml
作者: 高司令
类目: 编程语言
书ID: 101

结构体指针为参数方式
书名: 深入理解PHP7内核
作者: 鸟哥
类目: 编程技术
书ID: 103
```
***

## 数组

```cpp
#include<iostream>
#include<ctime>
#include<cstdlib>
#include<iomanip>
using namespace std;
using std::setw;

/* 10个随机数字，setw 格式化 */
int main()
{
	// 10个整数数组
	int nums[10];

	// 设置种子
	srand((unsigned)time(NULL));

	// 初始化
	for (int i = 0; i < 10; i++)
	{
		// 随机数
		nums[i] = rand();
	}
	cout << "Index" << setw(13) << "Value" << endl;

	// 输出
	for (int i = 0; i < 10; i++)
	{
		cout << setw(3) << i << setw(17) << nums[i] << endl;
	}
	return 0;
}

```

**结果**

```xml
Index        Value
  0        138980123
  1       1524202972
  2       2094408988
  3       1327403339
  4       1607793537
  5        399246158
  6       1391264278
  7       1176771810
  8       1826905447
  9         78662923
```

***

## 参数

```cpp
#include <iostream>
using namespace std;

int sum(int a, int b = 20);

int main()
{
	int a = 100;
	int b = 200;
	int result ;

	result = sum(a,b);
	cout << "result = " << result << endl;
    
    // 再次计算
	result = sum(a);
	cout << "result = " << result << endl;
	return 0;
}

int sum(int a, int b)
{
	return a + b;
}
```

**结果**

```xml
result = 300
result = 120
```
***

## 随机数

```cpp
#include<iostream>
#include<ctime>
#include<cstdlib>
using namespace std;

int main()
{
	int random;

	// 设置种子
	srand((unsigned)time(NULL));

	// 生成10个随机数字
	for (int i = 0; i < 10; i++)
	{
		// 生成实际随机数
		random = rand();
		cout << "random  : " << random << endl;
	}
	return 0;
}

```

**结果**

```xml
random  : 140728051
random  : 836857810
random  : 1198808467
random  : 682328715
random  : 336038025
random  : 2056578212
random  : 1160710619
random  : 321924185
random  : 1068470502
random  : 525470900
```
***

## 指针

```cpp
#include<iostream>
#include<string>
using namespace std;

/* 地址 */
int main()
{
	int num = 1;
	int *numP = &num;

	string str = "123";
	string *strP = &str;

	char cha[4] = "123";
	char (*chaP)[4] = &cha;

	// num 
	cout << "num 地址 -> " << &num << endl;
	cout << "numP 的值 -> " << *numP << endl;

	// str
	cout << "str 地址 -> " << &str << endl;
	cout << "strP 的值 -> " << *strP << endl;

	// cha
	cout << "cha 地址 -> " << &cha << endl;
	cout << "chaP 的值 -> " << *chaP << endl;
	return 0;
}
```

**结果**

```xml
num 地址 -> 0x7ffee6dfab60
numP 的值 -> 1
str 地址 -> 0x7ffee6dfab40
strP 的值 -> 123
cha 地址 -> 0x7ffee6dfab34
chaP 的值 -> 123
```
***

## 时间

```cpp
#include<iostream>
#include<ctime>
using namespace std;

string  Get_Current_Date();

int main()
{
	// 系统当前时间
	time_t now = time(0);

	// now 字符串
	char *dt = ctime(&now);
	cout << "本地日期和时间: " << dt << endl;

	// 把 now 转换成为 tm 结构
	tm *gmtm = gmtime(&now);
	dt = asctime(gmtm);
	cout << "UTC 日期和时间: "<< dt << endl;

	// 使用结构 tm 格式化时间
	cout << "1970 到目前经过秒数:" << now << endl;
	tm *ltm = localtime(&now);
	// 输出 tm 结构的各个组成部分
	cout << "\n年 : " << 1900 + ltm->tm_year << endl;
	cout << "月 : "   << 1 + ltm->tm_mon     << endl;
	cout << "日 : "   << ltm->tm_mday        << endl;
	cout << "时间 : " << ltm->tm_hour        << ":";
	cout << ltm->tm_min << ":" ;
	cout << ltm->tm_sec << endl;

	// 将当前日期以 20** - ** - ** 格式输出
	cout << "\n" << Get_Current_Date().c_str() << endl;
	return 0;
}

string  Get_Current_Date()
{
    time_t nowtime;  
    nowtime = time(0); //获取日历时间   
    char tmp[64];   
    strftime(tmp,sizeof(tmp),"%Y-%m-%d %H:%M:%S",localtime(&nowtime));   
    return tmp;
}
```

**结果**

```xml
UTC 日期和时间: Wed Feb 20 11:55:39 2019

1970 到目前经过秒数:1550663739

年 : 2019
月 : 2
日 : 20
时间 : 19:55:39

2019-02-20 19:55:39
```
***

## 类

```cpp
#include<iostream>
using namespace std;

/* 声明 Student 类 */
class Student
{
	public:
		string name;
		int age;
		long id;
		bool is_primary;

	/* 成员方法声明 */
	void setPrimary(bool is);

	/* 信息拼接 */
	string info(void)
	{
		return name + " , " + std::to_string(age) + " , " + std::to_string(id);
	}

	/* 是否小学生 */
	bool isPrimary()
	{
		return is_primary;
	}
};

/* 声明 Print Student  */
void printStudent(Student student);

int main()
{
	// 声明
	Student win, mac;

	// win
	win.name = "张三";
	win.age  = 20;
	win.id   = 12345678;

	// mac
	mac.name = "李四";
	mac.age  = 21;
	mac.id   = 87654321;

	// 打印
	printStudent(win);
	printStudent(mac);

	// info()
	string info_win = win.info();
	cout << "信息:" << info_win << endl;
	string info_mac = mac.info();
	cout << "信息:" << info_mac << endl;
	cout << endl;

	// setPrimary()
	win.setPrimary(false);
	mac.setPrimary(true);
	bool is_win = win.isPrimary();
	bool is_mac = mac.isPrimary();
	// boolalpha 直接 cout true/false
	cout << win.name << " isPrimary : " << boolalpha << is_win << endl;
	cout << mac.name << " isPrimary : " << boolalpha << is_mac << endl;
	return 0;
}

/* Student 成员方法定义 , 范围解析运算符 ::  */
void Student :: setPrimary (bool is)
{
   is_primary = is;
}

/* 打印  */
void printStudent(Student student)
{
	cout << "姓名: " << student.name << endl;
	cout << "年龄: " << student.age << endl;
	cout << "学号: " << student.id << endl;
	cout << endl;
}


```

**结果**

```xml
姓名: 张三
年龄: 20
学号: 12345678

姓名: 李四
年龄: 21
学号: 87654321

信息:张三 , 20 , 12345678
信息:李四 , 21 , 87654321

张三 isPrimary : false
李四 isPrimary : true
```
***

## 构造方法 & 析构函数

```cpp
#include <iostream>

using namespace std;

/* class Line */
class Line
{
	public:
		void setLength(double len);
		double getLength(void);

		// 构造函数
		Line();
		// 析构函数
		~Line();
	
	private:
		double length;
};

/* 成员函数定义，包括构造函数 */
Line :: Line (void)
{
	cout << "Line is being created !!!" << endl;
}

/* 定义析构函数 */
Line :: ~Line (void)
{
	cout << "Line is being destory !!!" << endl;
}

void Line :: setLength(double len)
{
	length = len;
}

double Line :: getLength(void)
{
	return length;
}

int main(int argc, char const *argv[])
{
	Line line;
	line.setLength(6.6);
	cout << "Length of line : " << line.getLength() << endl;
	return 0;
}
```

**结果**

```xml
Line is being created !!!
Length of line : 6.6
Line is being destory !!!
```
***

## 拷贝构造方法

```cpp
#include <iostream>

using namespace std;

/* 
 * 拷贝构造函数 
 * class Line 
 */
class Line
{
	public:
		int getLength(void);
		// 构造函数
		Line(int len);
		// 拷贝构造函数
		Line(const Line &obj);
		// 析构函数
		~Line();
	
	private:
		int *ptr;
};

/* 构造方法 */
Line :: Line(int len)
{
	cout << "调用构造方法" << endl;
	// 为指针分配内存
	ptr = new int;
	*ptr = len;
}

/* 拷贝构造函数 */
Line :: Line(const Line &obj)
{
	cout << "调用拷贝构造方法并为 ptr 分配内存" << endl;
	ptr = new int;
	// 拷贝值
	*ptr = *obj.ptr;

}

/* 析构函数 */
Line :: ~Line (void)
{
	cout << "释放内存" << endl;
	delete ptr;
}

/* Length */
int Line :: getLength(void)
{
	return *ptr;
}

/* 打印 */
void displaly(Line line)
{
 	cout << "Line length : " << line.getLength() << endl;
}

int main(int argc, char const *argv[])
{
	Line line1(101);
	Line line2 = line1;// 这里也调用了拷贝构造函数
	displaly(line1);
	displaly(line2);
	return 0;
}
```

**结果**

```xml
调用构造方法
调用拷贝构造方法并为 ptr 分配内存
调用拷贝构造方法并为 ptr 分配内存
Line length : 101
释放内存
调用拷贝构造方法并为 ptr 分配内存
Line length : 101
释放内存
释放内存
释放内存
```

***

## 友元函数

```cpp
#include <iostream>

using namespace std;

/* 
 * 友元函数 ，没有指针 
 * 类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。
 * 尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数
 * 友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类
 * 在这种情况下，整个类及其所有成员都是友元。
 * 如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 friend
 */

/*
 * 友元函数的使用
 * 因为友元函数没有this指针，则参数要有三种情况：
 * 要访问非static成员时，需要对象做参数；
 * 要访问static成员或全局变量时，则不需要对象做参数；
 * 如果做参数的对象是全局对象，则不需要对象做参数.
 * 可以直接调用友元函数，不需要通过对象或指针
 */

/* Box 类 */
class Box
{
	double width;

public:
	friend void printWidth(Box box);
	void setWidth(double width);
};

/* 成员方法声明 */
void Box :: setWidth(double wid)
{
	width = wid;
}

/* !!! 不是任何类的成员方法 */
void printWidth(Box box)
{
	// printWidth() 是 Box 的友元函数，可以直接访问 Box 的任何成员
	cout << "width of box : " << box.width << endl;
}

int main(int argc, char const *argv[])
{
	Box box;
	box.setWidth(101.1);
	printWidth(box);
	return 0;
}
```

**结果**

```xml
width of box : 101.1
```

***

## 内联函数

```cpp
#include <iostream>

using namespace std;

/* 内联函数 */
inline int Max(int x , int y)
{
	return (x > y)? x : y;
}


int main(int argc, char const *argv[])
{
	cout << "Max (20,10): " << Max(20,10) << endl;
	cout << "Max (0,200): " << Max(0,200) << endl;
	cout << "Max (100,1010): " << Max(100,1010) << endl;
	return 0;
}
```

**结果**

```xml
Max (20,10): 20
Max (0,200): 200
Max (100,1010): 1010
```

***

## This

```cpp
#include <iostream>

using namespace std;

/* this 的目的总是指向“这个”对象，是一个常量指针 */
class Box
{
public:
	Box(double wid){
		width = wid;
	}
	~Box(){};
	Box* get_address()
	{
		return this;
	}
	double widthValue()
	{
		return width;
	}
	int compare(Box box)
    {
        return this->widthValue() >= box.widthValue();
    }

private:
	double width;	
};

int main(int argc, char const *argv[])
{
	Box box1(2.3);
	Box box2(3.4);

	// Box* 定义指针p接受对象box的get_address()成员函数的返回值，并打印
	// this 指针的类型可理解为 Box*
    // 得到两个地址分别为 box1 和 box2 对象的地址
	Box* p = box1.get_address();
	cout << p << endl;
	cout << "box1.width = " << p->widthValue() << endl;
	cout << endl;

	p = box2.get_address();
	cout << p << endl; 
	cout << "box2.width = " << p->widthValue() << endl;
	cout << endl;

	// 比较
	if (box1.compare(box2))
	{
		cout << "box1 比 box2 大或相等" << endl;
	} 
	else 
	{
		cout << "box1 比 box2 小" << endl;
	}
	return 0;
}
```

**结果**

```xml
0x7ffeecfbdb88
box1.width = 2.3

0x7ffeecfbdb80
box2.width = 3.4

box1 比 box2 小
```

***

## 静态成员

```cpp
#include <iostream>

using namespace std;

/*静态成员变量*/
class Box
{
public:
	static int nums;
	Box(){
		// 创建一次成员 nums 加一次
		nums ++;
	}
	~Box(){
		nums --;
		cout << "Destory , nums = " << nums << endl;
	}
	static int getObjectNums()
	{
		return nums;
	}
	
};

/* 初始化 Box 的静态成员 */
int Box :: nums = 0;

int main(int argc, char const *argv[])
{

	cout << "Box :: getObjectNums() -> Total objects = " << Box :: getObjectNums() << endl;

	Box box1;
	Box box2;

	cout << "Box :: getObjectNums() -> Total objects = " << Box :: getObjectNums() << endl;

	Box box3;

	cout << "Total objects = " << Box :: nums << endl;
	return 0;
}
```

**结果**

```xml
Box :: getObjectNums() -> Total objects = 0
Box :: getObjectNums() -> Total objects = 2
Total objects = 3
Destory , nums = 2
Destory , nums = 1
Destory , nums = 0
```

***

## 继承

```cpp
#include <iostream>

using namespace std;

/* 基类 */
class BaseShape
{
public:
	void setWidth(int w)
	{
		width = w;
	}
	void setHight(int h)
	{
		hight = h;
	}

/* 派生类可见 */
protected:
	int width;
	int hight;
};

/*长方形*/
class Rectangle : public BaseShape
{
public:
	int getArea()
	{
		return (width * hight);
	}
};

/* 三角形 width 为宽，hight 为高*/
class Triangle : public BaseShape
{
public:
	int getArea()
	{
		return (width * hight / 2);
	}
};

int main(int argc, char const *argv[])
{
	// 长方形
	Rectangle rect;
	rect.setWidth(6);
	rect.setHight(3);
	cout << "Rectangle area = " << rect.getArea() << endl;

	// 三角形
	Triangle tria;
	tria.setWidth(2);
	tria.setHight(4);
	cout << "Triangle area = " << tria.getArea() << endl;
	return 0;
}


```

**结果**

```xml
Rectangle area = 18
Triangle area = 4
```

***

## 重载

```cpp
#include <iostream>

using namespace std;

/* 重载 + 运算符 */
class Box
{
public:
	void setWidth(double wid)
	{
		width = wid;
	}

	double getWidth()
	{
		return width;
	}
	/* 重载+运算符，将两个Box对象加起来 */
	Box operator+(const Box& b)
	{
		Box box;
		box.width = this->width + b.width;
		return box;
	}

private:
	double width;
};

int main(int argc, char const *argv[])
{
	Box mac;
	mac.setWidth(1.1);
	Box win;
	win.setWidth(1.2);
	Box android;
	android = win + mac;
	cout << "Width of android : " << android.getWidth() << endl;
	return 0;
}
```

**结果**

```xml
Width of android : 2.3
```

***

## 多态

```cpp
#include <iostream>

using namespace std;

/* 多态 : C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数 */

/* 基类 */
class Shape
{
protected:
	int width,height;
public:
	Shape(int w = 0, int h = 0)
	{
		width = w;
		height = h;
	}

    /* 虚函数 : 在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数 */
	virtual void area()
	{
		cout << "Parent class area :"<< 0 <<endl;
	}
	
};

/* 矩形 */
class Rectangle : public Shape
{
public:
	Rectangle(int w = 0, int h = 0 ) : Shape(w,h) { }
	void area ()
    { 
        cout << "Rectangle class area : "<< (width * height) <<endl;
    }
};

/* 三角形 */
class Triangle : public Shape
{
public:
	Triangle(int w = 0, int h = 0 ) : Shape(w,h) { }
	void area ()
    { 
        cout << "Triangle  class area : " << (width * height / 2) <<endl;
    }
};

int main(int argc, char const *argv[])
{
	Shape *shape;
	Rectangle rect(10,7);
	Triangle tria(10,8);

	// 利用矩形地址
	shape = &rect;
	shape->area();

	// 利用三角形地址
	shape = &tria;
	shape->area();
	return 0;
}
```

**结果**

```xml
Rectangle class area : 70
Triangle  class area : 40
```

***

## 异常

```cpp
#include <iostream>
using namespace std;

/* 异常：除法 */
double division(double a, double b);

int main(int argc, char const *argv[])
{
	double d = division(5.6, 2.8);
	cout << d << endl;

	try
	{
		d = division(5.6, 0);
	}
	catch(const char* msg)
	{
		cout << msg << endl;
	}
	return 0;
}

/* a/b, b = 0 时抛异常 */
double division(double a, double b)
{
	if (b == 0)
	{
		throw "除数不能为 0 !!!";
	}
	return (a / b);
}
```

**结果**

```xml
2
除数不能为 0 !!!
```

***

## 动态内存

```cpp
#include <iostream>

using namespace std;

class Dynamic
{
public:
	Dynamic()
	{
		cout << "Dynamic -> 调用构造函数！" <<endl; 
	}
	~Dynamic()
	{
		cout << "Dynamic -> 调用析造函数！" <<endl; 
	}
};

/*动态分配内存 : new , delete */
int main(int argc, char const *argv[])
{
	// 初始化 NULL 指针
	double *d = NULL;
	// 为变量请求内存
	d = new double;
	// 在分配的地址存值
	*d = 3.1415926;
	cout << "Value of d = " << *d << endl;
	// 释放内存
	delete d;

	// 对象
	Dynamic* dynamicArray  = new Dynamic[4];
	delete [] dynamicArray;
	return 0;
}
```

**结果**

```xml
Value of d = 3.14159
Dynamic -> 调用构造函数！
Dynamic -> 调用构造函数！
Dynamic -> 调用构造函数！
Dynamic -> 调用构造函数！
Dynamic -> 调用析造函数！
Dynamic -> 调用析造函数！
Dynamic -> 调用析造函数！
Dynamic -> 调用析造函数！
```

***

## 命名空间

```cpp
#include <iostream>

using namespace std;

/* 第 1 个命名空间 */
namespace first_space
{
   void func()
   {
      cout << "This is first_space" << endl;
   }
}

/* 第 2 个命名空间 */
namespace second_space
{
   void func()
   {
      cout << "This is second_space" << endl;
   }
}

int main(int argc, char const *argv[])
{
	// 1
	first_space :: func();
	// 2 
	second_space :: func();
	return 0;
}
```

**结果**

```xml
This is first_space
This is second_space
```

***

## **-1**

```cpp

```

**结果**

```xml

```



