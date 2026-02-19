vector存放内置数据类型
```
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

void print(int a)
{
	cout << a << endl;
}

int main()
{
	vector<int> v;
	v.push_back(10); //存放数据
	v.push_back(20);
	v.push_back(30);
	vector<int>::iterator pb = v.begin(); //指向第一个元素
	vector<int>::iterator pe = v.end(); //指向最后一个元素之后
	//单独定义pb、pe无必要
	while (pb != pe)
	{
		cout << *pb << endl;
		pb++;
	}
	cout << "----------------" << endl;
	for_each(v.begin(), v.end(), print); //#include <algorithm>
	
	return 0;
}
```
可以通过初始化减少push_back的使用（C++11）
`std::vector<int> v = {10, 20, 30};`


vector存放自定义数据类型
```
#include <iostream>
#include <algorithm>
#include <vector>

class Person
{
public:
	std::string m_Name;
	int m_Age;
	Person(std::string n, int a)
	{
		this->m_Name = n;
		this->m_Age = a;
	}
};

int main()
{
	Person p1("awa", 10), p2("qaq", 20);
	std::vector<Person> v1 = { p1, p2 };
	for (std::vector<Person>::iterator it = v1.begin(); it != v1.end(); it++)
	{
		std::cout << (*it).m_Name << ' ' << it->m_Age << '\n'; 
		//类似于指针
	}
	std::vector<Person*> v2 = { &p1, &p2 };
	for (auto it = v2.begin(); it != v2.end(); it++) //使用auto推导
	{
		std::cout << (*it)->m_Name << ' ' << (*it)->m_Age << '\n';
	}

	return 0;
}
```


容器嵌套
```
#include <iostream>
#include <vector>

int main()
{
	std::vector<int> v1 = { 1, 2, 3, 4 }, v2 = { 3, 5, 7, 8 };
	std::vector<std::vector<int>> v = { v1, v2 };
	for (auto it = v.begin(); it != v.end(); it++)
	{
		for (auto vit = (*it).begin(); vit != (*it).end(); vit++)
		{
			std::cout << (*vit) << ' ';
		}
		std::cout << '\n';
	}

	return 0;
}
```