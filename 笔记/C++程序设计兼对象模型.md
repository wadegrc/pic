[TOC]



### C++程序设计兼对象模型

### 勿在浮沙筑高台

#### 一、目标

+ 泛型编程

+ 深入探索面向对象之继承关系所形成的的**对象模型**

  

#### 二、学习内容

#### 1、转换函数(conversion function)

```c++
class Fraction
{
public:
	Fraction(int num,int den=1):m_numerator(num),m_denominator(den){}
	operator double() const{//转换函数
		return (double)(m_numerator / m_denominator);
	}
private:
	int m_numerator;//分子
	int m_denominator;//分母
}
Fraction f(3, 5);
double d = 4 + f;
```

可以把这种东西转为别的东西



#### 2、隐式单参数构造器(non-explicit-one-argument ctor)

```C++
class Fraction
{
public:
	Fraction(int num,int den=1):m_numerator(num),m_denominator(den){}
	Fraction operator + (const Fraction& f){
        return Fraction(f.m_numerator,f.m_denominator);
    }
private:
	int m_numerator;//分子
	int m_denominator;//分母
}
Fraction f(3,5);
Fraction d2=f+4;//调用non-explicit ctor 将4转为Fraction(4)
//然后调用operator+
```

可以把别的东西转换为这种东西



#### 3、1（转换函数）和2 (non-explicit)并存

```C++
Fraction f(3,5);
Fraction d2=f+4;//歧义
```



#### 4、显式单参数构造器(explicit-one-argument ctor)

```C++
class Fraction
{
public:
	explicit Fraction(int num,int den=1):m_numerator(num),m_denominator(den){}
	Fraction operator + (const Fraction& f){
        return Fraction(f.m_numerator,f.m_denominator);
    }
private:
	int m_numerator;//分子
	int m_denominator;//分母
}
Fraction f(3,5);
Fraction d2=f+4;//出错，4不会隐式转换为Fraction
```



#### 5、智能指针(pointer-like classes)

```C++
template<class T>
class shared_ptr
{
public:
	T& operator*() const{
		return *px;
	}
	T* operator -> ()const{
		return px;
	}
    shared_prt(T* p):px(p){}
private:
	T* px;
	long* pn;
}
```

```c++
shared_prt<Foo>sp(new Foo);
Foo f(*sp);

sp->method();//=px->method()
//疑问：由sp转到px已经消耗了->符号，为什么还能调用method()方法
//解析:->符号有继续作用的效果
```



#### 6、迭代器

```c++
重载了对指针的操作，如++，--，==，！= 等
```

```c++
reference operator*() const{
	return (*node).data;
}
pointer operator -> () const {
	return &(operator*());//得到一个指针，指向所指对象
}
```



#### 7、仿函数(function-like classes)

**能如函数一般使用()。**

```C++
template <class T>
struct identity:base classes {//标准库中，仿函数所使用的的奇特的base classes
	const T&
	operator()(const T& x)const {return x;}
}

template <class Pair>
struct select1st{
	const typename Pair::first_type&
	operator()(const Pair& x)const {//select1st()()
		return x.first;
	}
}
```



#### 8、namespace

##### 作用: 防止命名冲突

```C++
namespace jj01{
	void test_member_template(){
			····
	}
}
```

#### 9、template

```C++
template<typename T> or template<class T> //class or func template
class_name<int>//class
func_name(int,int)//function
//模板块的编译是一个半成品，还会在实现的地方重新编译
```



##### ①、成员模板(member template)

```c++
template <class T1,class T2>
struct pair{
	typedef T1 first_type;
    typedef T2 second_type;
    
    T1 first;
    T2 second;
    
    pair()
        : first(T1()), second(T2()){}
    pair(const T1& a, const T2& b)
        : first(a), second(b) {}
    template <class U1, class U2>
        pair(const pair<U1, U2>&p)
        :first(p.first),second(p.second){}//不过必须满足赋值动作
}
pair<Derived1,Derived2>p;
pair<Base1,Base2>p2(p);
//等效于pair<Base1,Base2>p2(pair<Derived1,Derived2>())
```

##### ②、模板特化（specialation）

```c++
template <class Key>//泛化
struct hash { };
//以下都是特化
template<>
struct hash<char>
{
	size_t operator()(char x) const { return x; }
};

template<>
struct hash<int>
{
	size_t operator()( int x ) const{ return x; } 
};

template<>
struct hash<long>
{
    size_t operator()(long x) const { return x; }
};

//样例
/*
其中第一个()用来声明一个临时结构体
第二个()调用重载后的()
此时调用用long类型的模板
*/
cout<< hash<long>()(1000);

```



##### ③、偏特化(partion specialization)

两个角度：

+ 个数上的偏
+ 范围上的偏

1、个数上的偏

```c++
template< typename T, typename Alloc=..>
class vector
{
...
};

//个数上的偏,bool已经指定,必须从左到右，顺序绑定
template<typename Alloc=...>
class vector<bool, Alloc>
{
...
};
```

2、范围的偏

```c++
template <typename T>
class C
{
...
};

template< typename T>
class C<T*>
{
...
};

//or
template< typename U>
class C<U*>
{
	...
};
//样例
/*
本来是任意类型，范围缩小为指针类型
*/
C<string>obj1;
C<string*>obj2;
```

##### ⑤、模板模板参数(template template param)

```c++
template<typename T,
	template<typename T>//容器的模板
	class Container  //容器类型
	>
class XCls
{
private:
	Container<T>c;
public:
    ....
};

template<typename T>
using Lst = list<T,allocator<T>>;

//样例
XCls<string,list>mylst1;//错误
XCls<string,Lst> mylst2;//正确

//2
template<class T,class Sequence = deque<T>>//指定默认值
    class stack
    {
        protected:
        Sequence c; //底层容器
    };
//样例
stack<int> s1;
stack<int,list<int>>s2;//list已经写定成int类型了,所以此处不是模板
```

#### 10、C++模板库

##### ①、variadic templates(数量不定的模板参数)

```c++
void print()//1
{
}

template<typename T, typename... Types>//...表示数量未定
void print(const T& firstArg, const Types&... args)//...表示的参数可以当成一个包
{
    cout<<firstArg<<endl;
    print(args...);//此处...也是包，表示再把包传给自己，将包里面的第一个作为第一个参数
    //当到包中最后一个参数时，如果不指定1位置的print将会报错，因为此处的print必须指定一个或多个参数，指定1处后将会调用1处print
    //sizeof...(args)可以查询包中参数的数量
}

```

##### ②、auto

```c++
list<string>c;
list<string>::iterator ite;
ite = find(c.begin(),c.end(),target);
//or
auto ite = find(c.begin(),c.end(),target);

auto ite;//编译器无法退出ite的类型
ite = find(c.begin(),c.end(),target);
/*
若全部用auto，可取吗？
不可取
1、代码可读性太差
2、你不可能每次申明变量就赋值
*/
```

#####  ③、for each

```c++
//模板
for( decl : coll )
{
    statement
}
//样例1
for(int i : {1,2,3})
{
    cout<< i <<endl;
}
//样例2
vector<double>vec;
...
for( auto elem : vec)//值，copy
{
    cout<<elem<<endl;
}

for( auto& elem:vec)//引用,对引用的修改会影响原位置的值
{
    elem *= 3;
}
```



#### 11、reference(引用)

![c++_reference_1](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_reference_1.png)

```c++
int x=0;
int *p = &x;
int& r = x;//r代表x,现在r,x都是0
int x2 = 5;

r = x2; //r不能重新代表其他物体，现在r,x都是5
int &r2 = r;//现在r2是5(r2代表r,也代表x)
/*Note:特意制造出的假象
*sizeof(r)==sizeof(x)
*&x == &r;
*/
```

![c++_reference_2](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_reference_2.png)

```c++
reference通常用来参数类型和返回类型的描述
//错误用法:这两行不能并存,same signature(签名相同)
double imag(const double& im){...}
double imag(const double im){...}
//const算不算签名一部分？
//不算
```

#### 12、对象模型：

##### 1、关于vptr(虚指针)和vtbl(虚表)

![c++_reference_3](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_vptr_vtbl.png)

```c++
class A
{
public:
	virtual void vfunc1();//A::vfunc1
    virtual void vfunc2();//A::vfunc2
    void func1();
    void func2();
private:
    int m_data1,m_data2;
};

class B : public A
{
public:
    virtual void vfunc1();//B::vfunc1，重写了A::vfunc1
    void func2();
private:
    int m_data3;
};

class C : public B
{
public:
    virtual void vfunc1();//C::vfunc1
    void func2();
private:
    int m_data1,m_data4;
}
/*子类能重写父类的虚函数，
 * 函数的调用：
 * 1、静态调用：通过call指令直接调用·
 * 2、动态调用：满足三个条件，1指针，2向上转型成父类了，3调用的是虚函数
 * */
```

##### 2、关于this

![c++_this_1](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_this_l.png)

```c++
//谁调用函数，谁就是该函数的this

```

##### 3、const

![c++_const_1](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_const_l.png)

```C++
//const object 只能调用const函数
//non-const object只能调用non-const函数
//下面为函数重载,const属于签名
charT
    operator[](size_type pos) const
 {../*不必考虑copy on write*/}
reference 
    operator[](size_type pos)
{..../*必须考虑copy on write*/}
```

##### 4、关于动态绑定(Dynamic Binding)

![c++_db_1](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_db_l.png)

````c++
//a.vfunc1() 通过对象调用的为静态绑定，调用自己的成员函数
````

![c++_db_2](https://github.com/wadegrc/pic/blob/master/%E5%9B%BE%E5%BA%8A/c++_db_2.png)

```c++
A* pa = new B //指针强转
pa->vfunc1() //动态绑定

```

##### 5、重载::operator new,::operator delete ::operator [] new ::operator delete[]

```c++
void* myAlloc(size_t size)
{
	return malloc(size);	
}
void myFree(void *ptr)
{
    return free(ptr);
}

//全局
//他们不可以声明在一个namespace内
inline void* operator new(size_t size)
{cout<<"grc global new() \n"; return myAlloc(size);}
/**/
inline void* operator new[](size_t size)
{cout<<"grc global new[]() \n"; return myAlloc(size);}

inline void* operator delete(void *ptr)
{cout<<"grc global delete() \n"; return myFree(ptr);}

inline void* operator delete[](void *ptr)
{cout<<"grc global delete[]() \n"; return myFree(ptr);}

//members operator new delete重载
//与全局差不多
Foo* pf = ::new Foo;//指定调用全局
::delete pf;
```

 ##### 6、重载new() delete()

**实现了在指定内存地址上用指定类型的构造函数来构造一个对象的功能**

```c++
class Foo
{
public:
    Foo(){cout<<"Foo::Foo()"<<endl;}
    Foo(int){cout<<"Foo::Foo(int)"<<endl;throw exception;}
    //(1)这个是一般的operator new()的重载
    void*  operator new(size_t size)
    {
        return malloc(size);
    }
    //(2)这个就是标准库已提供的placement new()的重载形式
    //所以我也模拟standard placement new, 就只是传回Pointer
    void* operator new(size_t size, void* start)
    {
        return start;
    }
    //（3）这个才是崭新的placement new
    void* operator new(size_t size, long extra)
    {
        return malloc(size+extra);
    }
    //(4)这又是一个placement new
    void* operator new(size_t size, long extra, char init)
    {
        return malloc(size+extra);
    }
}
//我们也可以重载operator delete(),但它们绝对不会被delete调用，只有当ctor抛出exception
//才会调用(对应的)operator delete()。主要用来归还完全创建成功的object所占用的memory
Foo* p1 = new Foo;
Foo*p2 = new(&start)Foo;
Foo*p3= new(100)Foo;
Foo*p4=new(100)Foo(1);//调用对应构造函数

```



