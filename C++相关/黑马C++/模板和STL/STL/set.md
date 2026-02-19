集合容器，自动排序,`set`不允许 重复元素，插入重复元素会无视；`multiset`允许

**插入和删除：**
插入数据只有`insert()`方法，没有push
`insert(elem);`           //在容器中插入元素。
`clear();`                    //清除所有元素
`erase(pos);`               //删除pos迭代器所指的元素，返回下一个元素的迭代器。
`erase(beg, end);`     //删除区间\[beg,end)的所有元素 ，返回下一个元素的迭代器。
`erase(elem);`             //删除容器中值为elem的元素。

**查找和统计：**
`find(key)` //查找key是否存在，存在返回其迭代器，不存在返回`set.end()`
`count(key)` //统计key个数

**`<set>`容器类型：**
1. **set**
    - `set` 是一个关联容器，内部使用红黑树实现。
    - 它存储唯一的元素，不允许重复。
    - 元素是按照一定的顺序（默认是升序）进行存储的。
    - 插入、删除和查找操作的时间复杂度为O(log n)。
2. **multiset**
    - `multiset` 与 `set` 类似，但允许存储重复的元素。
    - 它也使用红黑树实现，因此元素也是有序的。
    - 插入、删除和查找操作的时间复杂度同样为O(log n)。
3. **unordered_set**
    - `unordered_set` 是一个哈希表实现的容器。
    - 它存储唯一的元素，不允许重复。
    - 与 `set` 不同，`unordered_set` 中的元素是无序的，插入、删除和查找操作的时间复杂度为O(1)（平均情况下）。
    - 由于使用哈希表，查找速度通常比 `set` 快，但不保证元素的顺序。

**改变排序规则：（仿函数）**
```cpp
class my_compare
{
public:
	bool operator()(const int v1, const int v2) const
	{
		return v1 > v2;
	}
};

int main()
{
	set<int, my_compare> s;
	s.insert(1);
	s.insert(5);
	s.insert(3);
	for (int n : s) cout << n << ' ';
}
```
```cpp
//自定义数据类型排序
class Person
{
public:
	string name;
	int age;
	Person(string n, int a) : name(n), age(a) {};
};

class my_compare
{
public:
	bool operator()(const Person& p1, const Person& p2) const
	{
		return p1.age < p2.age;
	}
};

int main()
{
	set<Person, my_compare> s;
	Person p1("awa", 10);
	Person p2("qaq", 30);
	Person p3("owo", 20);
	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	for (Person n : s) cout << n.name << ' ' << n.age << '\n';
}

```