`pair`成对出现的数据，利用对组可以返回两个数据

`pair<type, type> p ( value1, value2 );`
`pair<type, type> p = make_pair( value1, value2 );`

`pair.first` //访问前一个元素
`pair.second` //访问后一个元素（均不需要加括号）


`map`不允许重复key值，`multimap`允许
map中所有元素都是pair
pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
所有元素都会根据元素的键值自动排序

**构造和赋值：**
`map<T1, T2> mp;`                     //map默认构造函数: 
`map(const map &mp);`             //拷贝构造函数
`map<int, int> m; m.insert(pair<int, int>(1, 10))`
`map& operator=(const map &mp);`    //重载等号操作符

**大小和交换：**
`size();`          //返回容器中元素的数目（一对pair为一个元素）
`empty();`        //判断容器是否为空
`swap(st);`      //交换两个集合容器

**插入和删除：**
`insert()`  //若键已存在，**不会覆盖原有值**。返回值为一个迭代器和一个布尔值，指示插入是否成功。
```cpp
//四种插入方式
//插入
	map<int, int> m;
	//第一种插入方式
	m.insert(pair<int, int>(1, 10));
	//第二种插入方式
	m.insert(make_pair(2, 20));
	//第三种插入方式
	m.insert(map<int, int>::value_type(3, 30));
	//第四种插入方式
	m[4] = 40; //m[i]如果没有对应的值，会创建一个并赋值为0
	printMap(m);
```
`erase(key)` //删除容器中值为k的元素（一对）
`emplace(T1 a, T2 b)`  //直接构造键值对，无需显式创建 `pair`。避免临时对象的拷贝，效率更高。 若键已存在，**不会覆盖原有值**。
- **推荐 `emplace` 或 `insert`**：
    - 如果不需要覆盖现有键，且希望避免不必要的拷贝（尤其是对象较大时），优先使用 `emplace`。
    - 使用 `insert` 可以更明确地表达意图，且兼容性更好（适用于 C++11 之前的代码）。
- **慎用 `operator[]`**：
    - 仅当需要**覆盖已有键**或**默认插入空值**时使用，因为它可能隐式修改数据


### **1. map**
- **底层实现**：红黑树（自平衡二叉搜索树）。  
- **特性**：  
  - **键唯一**：每个键只能对应一个值。  
  - **自动排序**：按键的升序排列，支持范围查询（如`lower_bound`）。  
- **时间复杂度**：插入、删除、查找均为 **O(log n)**。  
- **适用场景**：需要有序键且键唯一的场景，如字典、配置项管理。  

---

### **2. multimap**
- **底层实现**：红黑树。  
- **特性**：  
  - **允许键重复**：一个键可以对应多个值。  
  - **自动排序**：与`map`相同，按键升序排列。  
- **时间复杂度**：同`map`，均为 **O(log n)**。  
- **适用场景**：需要有序且键可重复的场景，如学生成绩按学号分组统计。  

---

### **3. unordered_map**
- **底层实现**：哈希表（链地址法解决冲突）。  
- **特性**：  
  - **键唯一**：同`map`，键不允许重复。  
  - **无序存储**：元素顺序由哈希函数决定，不保证任何顺序。  
- **时间复杂度**：平均 **O(1)**，最坏情况（哈希冲突严重时） **O(n)**。  
- **适用场景**：需快速查找且无需顺序的场景，如缓存、词频统计。  

---

### **对比总结**
| **特性**   | map      | multimap | unordered_map |     |
| -------- | -------- | -------- | ------------- | --- |
| **键唯一性** | ✅        | ❌（允许多值）  | ✅             |     |
| **顺序性**  | 有序（升序）   | 有序（升序）   | 无序            |     |
| **底层结构** | 红黑树      | 红黑树      | 哈希表           |     |
| **查找效率** | O(log n) | O(log n) | 平均O(1)，最差O(n) |     |

---

### **选择建议**  
- 需要**有序性**且**键唯一** → `map`  
- 需要**有序性**且**键可重复** → `multimap`  
- 需要**极速查找**且不关心顺序 → `unordered_map`  






在C++中，`std::map` 是STL提供的关联容器，基于红黑树实现，用于存储键值对（key-value pairs），并按键**自动排序**。以下是详细使用方法及注意事项：

---

### **一、基本特性**
- **键唯一性**：每个键（key）唯一，重复插入会失败
- **自动排序**：默认按键的升序排列（可通过自定义比较函数修改）
- **时间复杂度**：查找、插入、删除操作均为 **O(log n)**
- **头文件**：`#include <map>`

---

### **二、声明与初始化**
#### 1. 基本声明
```cpp
std::map<int, std::string> m1;              // 空map
std::map<std::string, double> m2 = {        // C++11统一初始化
    {"apple", 4.5},
    {"banana", 3.2},
    {"cherry", 6.7}
};
```

#### 2. 自定义排序规则
```cpp
// 按键降序排列
struct CompareGreater {
    bool operator()(const int& a, const int& b) const {
        return a > b;
    }
};
std::map<int, std::string, CompareGreater> m3; 

// C++11 Lambda表达式方式
auto cmp = [](const int& a, const int& b) { return a > b; };
std::map<int, std::string, decltype(cmp)> m4(cmp);
```

---

### **三、核心操作**
#### 1. 插入元素
```cpp
// 方式1：insert + pair
m1.insert(std::pair<int, std::string>(3, "three"));

// 方式2：emplace（C++11更高效）
m1.emplace(4, "four");  

// 方式3：operator[]（若key不存在则创建）
m1[5] = "five";  
```

#### 2. 遍历元素
```cpp
// 迭代器遍历
for (auto it = m1.begin(); it != m1.end(); ++it) {
    std::cout << "Key: " << it->first 
              << ", Value: " << it->second << std::endl;
}

// C++11 范围for循环
for (const auto& [key, value] : m1) {  // C++17结构化绑定
    std::cout << key << ": " << value << std::endl;
}
```

#### 3. 查找与访问
```cpp
// 查找元素是否存在
if (m1.find(3) != m1.end()) {
    std::cout << "Key 3 found: " << m1[3] << std::endl;
}

// 统计键出现的次数（map中结果为0或1）
if (m1.count(3) > 0) { /* ... */ }

// 安全访问（避免operator[]自动插入）
auto it = m1.find(100);
if (it != m1.end()) {
    std::cout << it->second;  // 正确方式
}
```

#### 4. 删除元素
```cpp
m1.erase(3);               // 通过键删除
auto it = m1.find(4);
if (it != m1.end()) {
    m1.erase(it);          // 通过迭代器删除
}
m1.erase(m1.begin(), m1.find(5));  // 删除范围[begin, 5)
```

---

### **四、高级用法**
#### 1. 合并两个map
```cpp
std::map<int, std::string> m5 {{1, "A"}, {2, "B"}};
m1.merge(m5);  // C++17特性，重复键保留原map内容
```

#### 2. 获取边界
```cpp
auto lower = m1.lower_bound(3);  // 第一个>=3的迭代器
auto upper = m1.upper_bound(8); // 第一个>8的迭代器
for (auto it = lower; it != upper; ++it) {
    // 处理范围[3, 8]内的元素
}
```

#### 3. 自定义键类型
```cpp
struct Point {
    int x, y;
    bool operator<(const Point& p) const {  // 必须重载<
        return x < p.x || (x == p.x && y < p.y);
    }
};
std::map<Point, std::string> pointMap;
```

---

### **五、性能分析**
| 操作                | 时间复杂度 | 说明                      |
|---------------------|------------|---------------------------|
| `insert`/`emplace`  | O(log n)   | 红黑树插入节点             |
| `erase`             | O(log n)   | 红黑树删除节点             |
| `find`/`count`      | O(log n)   | 二分查找树结构             |
| 遍历                | O(n)       | 中序遍历所有节点           |

---

### **六、常见问题**
1. **键的唯一性冲突**  
   若需要允许重复键，应使用 `std::multimap`

2. **迭代器失效**  
   删除元素时，指向被删元素的迭代器会失效，其他迭代器不受影响

3. **自定义键类型必须可比较**  
   若使用自定义类型作为键，必须提供有效的比较函数（通过重载`operator<`或自定义比较器）

---

### **七、最佳实践**
1. **优先使用`emplace`代替`insert`**  
   `emplace` 直接在容器内构造元素，避免临时对象拷贝

2. **避免频繁的`operator[]`查询**  
   若键可能不存在，先用`find`检查，防止意外插入空值

3. **利用有序特性优化查询**  
   使用`lower_bound`/`upper_bound`实现范围查询

---

通过合理使用 `std::map`，可以高效管理需要按键排序的键值对数据。当需要更高性能的无序存储时，可考虑 `std::unordered_map`（哈希表实现）。

