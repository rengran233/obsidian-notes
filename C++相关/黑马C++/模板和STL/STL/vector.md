`vector`是一种**动态数组**，`vector`和普通数组的区别是**动态扩展**，可以找到一块更大的新空间，拷贝原数据后释放原空间

`vector`支持随机访问


**构造函数：**
`vector<T> v; `               		            //采用模板实现类实现，默认构造函数
`vector(v.begin(), v.end());   `  //将v\[begin(), end())区间中的元素拷贝给本身。
`vector(n, elem);`                            //构造函数将n个elem拷贝给本身。
`vector(n)`                                         //初始化空间（默认填充n个0）
// 如果想不填充元素预留空间，使用`reserve()`函数
`// vector<vector<char>> temp(n, vector<char>(n));` 内嵌容器预留空间
`vector(const vector &vec);`         //拷贝构造函数。

**赋值操作：**
`vector& operator=(const vector &vec);`//重载等号操作符
`assign(beg, end);`       //将\[beg, end)区间中的数据拷贝赋值给本身。
`assign(n, elem);`        //将n个elem拷贝赋值给本身。

**容量和大小：**
`empty(); `                            //判断容器是否为空，为空返回真
`capacity();`                      //容器的容量
`size();`                              //返回容器中元素的个数
`resize(int num);`            
//重新指定容器的长度为num，若容器变长，则以默认值0填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
`resize(int num, elem);`  
//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除

**插入和删除：**
`push_back(ele);`                                         //尾部插入元素ele
`pop_back();`                                                //删除最后一个元素
`insert(const_iterator pos, ele);`        //迭代器指向位置pos插入元素ele
`insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
`erase(const_iterator pos);`                     //删除迭代器指向的元素
`erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
`clear();`                                                        //删除容器中所有元素

**数据存取：**
`at(int idx); `     //返回索引idx所指的数据
`operator[]; `       //返回索引idx所指的数据
`front(); `            //返回容器中第一个数据元素
`back();`              //返回容器中最后一个数据元素
访问容器无元素部分将导致未定义行为

**容器互换：**
`swap(vec);`  // 将vec与本身的元素互换
用途：收缩内存
`vector<int>(v).swap(v)` //创建匿名对象，容量与v的大小相同

**预留空间：**
`reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。
可以避免因容量不足在动态扩展时频繁开辟空间

**其他：**
`begin()` //指向第一个元素
`end()` //指向最后一个元素后的位置
`rbegin()` //指向最后一个元素
`rend()` //指向第一个元素前的位置
//反向迭代器重载`++`和`--`，`++`向开始方向移动，相当于普通迭代器的`--`