![image-20240928161226433](../tyrecording/assets/image-20240928161226433-1727573667634-1-1728826314494-1.png)

> ✏️ 关于专栏：专栏用于记录 c++知识学习  
> 😭 补救 😭

[TOC]

## C++入门

### C++关键字

总计63个关键字

<img src="https://gitee.com/Black_aura/picture/raw/master/img/image-20241013105240721.png" alt="image-20241013105240721" style="zoom:67%;" />

### 命名空间

在C/C++中，变量、函数和后面要学到的类都是大量存在的，这些变量、函数和类的名称将都存 在于全局作用域中，可能会导致很多冲突。使用命名空间的目的是对标识符的名称进行本地化， 以避免命名冲突或名字污染，namespace关键字的出现就是针对这种问题的。

#### 命名空间定义

$namespace + 命名空间名字 + \{\}$​

- 命名空间可以嵌套
- 同一个工程中存在多个相同名称的命名空间，编译器最后会合成一个命名空间

```c++
namespace wneosy{
    //命名空间中可以定义变量/函数/类型
    int a = 10;
    int Add(int a,int b){
        return a+b;
    }
    struct Node{
        struct Node* next;
        int val;
    };
}
namespace N1{
    int a;
    namespace N2{
        int a;
    }
}
```

一个命名空间就定义了一个新的作用域，命名空间中的所有内容都局限于该命名空间中

#### 命名空间的使用

- 加命名空间名称及作用域限定符$::$

```c++
cout<<wneosy::a<<endl;
```

- 使用using将命名空间中某个成员引入

```c++
using wneosy::b;
int main(){
	cout<<wneosy::a<<endl;
    cout<<b<<endl;
    return 0;
}
```

- 使用$using\ namespace\ 命名空间名称$​ 引入

```c++
using namespace wnoesy:
int main(){
    cout<<wneosy::a<<endl;
    cout<<b<<endl;
   	return 0;
}
```

### 输入&输出

```c++
#include<iostream>//头文件
using namespace std;
int main(){
    cout<<"hello"<<endl;
    //cout和cin都是全局的流对象，endl是特殊的c++符号，表示换行输出
    //<<是流插入运算符，>>是流提取运算符
    return 0;
}
```

### 缺省参数

#### 缺省参数概念

缺省参数是声明或定义函数时为函数的参数指定一个缺省值。在调用该函数时，如果没有指定实 参则采用该形参的缺省值，否则使用指定的实参。

```c++
void Func(int a = 0){
	cout<<a<<endl;
}
int main({
	Func();//没有传参时，使用参数的默认值
    Func(10);//传参时，使用指定的参数
}
```

#### 缺省参数分类

- 全缺省参数

```c++
void Func(int a = 10,int b = 20,int c = 30){
    cout<<"a = "<<a<<endl;
    cout<<"b = "<<b<<endl;
    cout<<"c = "<<c<<endl;
}
```

- 半缺省参数

````c++
void Func(int a,int b = 10,int c = 20){
    cout<<"a = "<<a<<endl;
    cout<<"b = "<<b<<endl;
    cout<<"c = "<<c<<endl;
}
````

半缺省参数必须**从右往左连续**依次给出，不能间隔着给

缺省参数不能在函数声明和定义中同时出现

### 函数重载

#### 函数重载概念

函数重载：是函数的一种特殊情况，C++允许在同一作用域中声明几个功能类似的同名函数，这 些同名函数的形参列表(参数个数 或 类型 或 类型顺序)不同，常用来处理实现功能类似数据类型 不同的问题。

```c++
#include<iostream>
using namespace std;
//1.参数类型不同
int Add(int left, int right) {
	cout << "int Add(int left,int right)" << endl;
	return left + right;
}
double Add(double left, double right) {
	cout << "double Add(double left,double right)" << endl;
	return left + right;
}
//2.参数个数不同
void f() {
	cout << "f()" << endl;
}
void  f(int a) {
	cout <<"f(int a)" << endl;
}
//3.参数类型顺序不同
void f(int a, char b) {
	cout << "f(int a,char b)" << endl;
}
void f(char b, int a) {
	cout << "f(char b,int a)" << endl;
}
int main() {
	Add(10, 20);
	Add(10.1, 20.2);
	f();
	f(10);
	f(10, 'a');
	f('a', 10);
	return 0;
}
```

#### C++支持函数重载的原理-名字修饰

c语言没办法支持重载，是因为同名函数没办法区分。而c++是通过函数修饰规则来区分，只要参数不同，修饰出来的名字就不一样，就支持了重载。

如果两个函数函数名和参数是一样的，返回值不同是不构成重载的，因为调用时编译器没办法区分。

### 引用

引用不是新定义一个变量，而是给已存在变量取了一个别名，编译器不会为引用变量开辟内存空 间，它和它引用的变量共用同一块内存空间。

$类型\& 引用变量名（对象名）=引用实体;$

引用类型必须和引用实体是同种类型的

```c++
void TestRef(){
	int a = 10;
    int& ra = a;//定义引用类型
    printf("%p\n", &a);//%p：格式说明符，专门用于输出指针类型的数据
	printf("%p\n", &ra);
}
```

可以看到得到了一样的地址。

<img src="../tyrecording/assets/image-20241013204611522.png" alt="image-20241013204611522" style="zoom: 67%;" />

#### 引用特性

1. 引用在定义时必须初始化
2.  一个变量可以有多个引用
3. 引用一旦引用一个实体，再不能引用其他实体

```c++
int a = 1;
int& c = a;
int b = 2;
c = b; //这里是将b赋值给了c,c和a的值都变成了b值，c和a的地址还是一样
```

#### 常引用

```c++
void TestConstRef(){
    const int a = 10;
    //int& c = a; //错误，c的类型是int,编译不通过，a是只读，c的类型是int,也就是可读可写
    const int& c = a;
    int b = 1;
    const int& d = b;//可以，b是可读可写，d变成别名只读
    //总结：引用取别名是，权限可以缩小，但不能放大
    int i = 0；
    //double& j = i;//错误
    const double& j = i;//可以
    //隐式类型转换，临时变量相当于常量，所以加上const可以，double相当于可读可写引用可读，所以不行
}
```

关于引用剩下内容参见基础第二篇~~