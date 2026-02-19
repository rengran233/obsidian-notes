`this`指针是一个隐含的指针，用于**指向当前对象的实例**。保证每个对象拥有自己的数据成员，但共享处理这些数据成员的代码
普通成员函数：自动携带
静态成员函数：无`this`指针（不和具体对象绑定）
成员变量：无，但是可以用`this->`指向
1.解决名称冲突
```
class Person
{
public:
	Person(int age)
	{
		age = age; //两个age都是形参
	}
	int age;
};

int main()
{
	Person p(18);
	cout << p.age; //此时输出的是乱码
}
```
用this可以避免
```
class Person
{
public:
	Person(int age)
	{
		this -> age = age;
	}
	int age;
};

int main()
{
	Person p(18);
	cout << p.age;
}
```

2.返回对象本身（\*this）
```
class Person
{
public:
	Person(int age)
	{
		this -> age = age;
	}
	Person &addAge(Person &p)
	//此处如果不返回引用，会创建并返回一个新对象，并非p2 
	{
		this -> age += p.age;
		return *this; //this是指针，*this是对象的本体
	}
	int age;
};
int main()
{
	Person p1(10);
	Person p2(10);
	p2.addAge(p1).addAge(p1).addAge(p1); //链式编程
	cout << p2.age;
}```