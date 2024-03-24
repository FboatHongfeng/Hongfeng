---
title: 2Hello World
top: 111111
menu: 123
tags: 
	- tags2
---
# C++学习 之 成员初始化列表

&emsp;&emsp;在类中，我们经常使用构造函数去初始化我们的成员变量以及进行一系列初始化操作，比如这样：
```C++
class Entity
{
    public:
        int m_X,m_Y;
        std::string m_Name;
    public:
        Entity()
        {

        }
        Entity(std::string& name)
        {
            int m_X = 1;
            int m_Y = 1;
            m_Name = name;
        }
}
int main()
{
    Entity e("LSJ");
    std::cout<< e.m_Name <<endl;
}
```
&emsp;&emsp;我们也可以使用另一种初始化方法，那就是成员初始化列表，方法如下：
```C++
class Entity
{
    public:
        int m_X,m_Y;
        std::string m_Name;
    public:
        Entity()
        {

        }
        Entity(std::string& name)
            :m_X(1),m_Y(1),m_Name(name)//just like this
        {
        }
}
```
&emsp;&emsp;显然，我们在这样做的时候必须遵守成员变量声明的顺序进行初始化，以防止出现任何意外的错误。那么，这样的方法除了能让初始化变量与初始化处理等一系列操作从代码里分离方便阅读，还有什么其他的优点？
&emsp;&emsp;事实上，在C++类的其他类类型的成员声明的时候，会为类类型的成员主动地进行初始化，在构造函数中又会再次为这个已经初始化的类类型赋值，这样会造成性能上的损耗。比如用下面的方法去做：
```C++
class NewClass
{
public:
	int m_X;
public:
	NewClass()
	{
		FDF::print("number is created");
        //这里使用的FDF命名空间是我自己定义的头文件中的
        //实际使用起来的效果和cout输出流没有太大区别，本身也是把cout封装实现的效果
	}
	NewClass(const int& number)
	{
		m_X = number;
		FDF::print("number is created with",number);
	}
};
class Entity
{
    public:
        NewCLass m_Member;
        int m_X;
    public:
        Entity(){}
        Entity(const int number)
        {
            m_Member = NewClass(8);
            m_X = 0;
        }
}
int main()
{
    Entity e(8);
}
```
&emsp;&emsp;上述代码就会同时出现内容"number is created"和内容"number is created with 8"，这意味着m_Member被初始化了两次，而通过初始化成员列表的方法就不会出现这样的重复初始化的情况。
&emsp;&emsp;所以我们在写构造函数的时候最好应该使用初始化成员列表，虽然有时候重复初始化的情况不一定发生(当成员变量都是基础类型的时候就不会重复初始化)，但是最好还是防患于未然，使用如下的构造函数写法：
```C++
class Entity
{
    public:
        NewCLass m_Member;
        int m_X;
    public:
        Entity(){}
        Entity(const int number)
            :m_Member(8),m_X(1)
        {
        }
}
```
