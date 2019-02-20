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

**-1**