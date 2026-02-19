C++ STL 中的 `map` 是一种关联容器，用于存储**键值对（key-value pairs）**，并按键自动排序。以下是其用法的详细说明：

---

### **1. 头文件与声明**
```cpp
#include <map>
using namespace std;

// 基本声明（默认按 key 升序排列）
map<KeyType, ValueType> myMap;

// 自定义排序规则（例如降序）
map<KeyType, ValueType, greater<KeyType>> myMapDesc;
```

---

### **2. 插入元素**
#### (1) `insert()` 方法
```cpp
myMap.insert(make_pair("apple", 3));  // C++98
myMap.insert({"banana", 5});          // C++11 统一初始化
myMap.emplace("cherry", 7);           // C++11 原地构造，效率更高
```

#### (2) `operator[]` 方法
```cpp
myMap["date"] = 9;  // 若 key 不存在，会插入新键值对；存在则覆盖值
```

---

### **3. 访问元素**
#### (1) `operator[]`
```cpp
int count = myMap["apple"];  // 若 key 不存在，插入默认值（如 int 的 0）
```

#### (2) `at()` 方法（C++11）
```cpp
int count = myMap.at("apple");  // 若 key 不存在，抛出 out_of_range 异常
```

---

### **4. 查找元素**
#### (1) `find()` 方法
```cpp
auto it = myMap.find("apple");
if (it != myMap.end()) {
    cout << "Found: " << it->second << endl;
} else {
    cout << "Not found" << endl;
}
```

#### (2) `count()` 方法
```cpp
if (myMap.count("apple") > 0) {
    cout << "Key exists" << endl;
}
```

---

### **5. 删除元素**
#### (1) 通过 `erase()` 删除
```cpp
myMap.erase("apple");         // 通过 key 删除
myMap.erase(it);              // 通过迭代器删除
myMap.erase(it1, it2);        // 删除区间 [it1, it2)
```

#### (2) 清空 map
```cpp
myMap.clear();
```

---

### **6. 遍历元素**
#### (1) 迭代器遍历
```cpp
for (auto it = myMap.begin(); it != myMap.end(); ++it) {
    cout << it->first << ": " << it->second << endl;
}
```

#### (2) 范围 for 循环（C++11）
```cpp
for (const auto& pair : myMap) {
    cout << pair.first << ": " << pair.second << endl;
}
```

#### (3) 结构化绑定（C++17）
```cpp
for (const auto& [key, value] : myMap) {
    cout << key << ": " << value << endl;
}
```

---

### **7. 容量查询**
```cpp
if (myMap.empty()) { /* 判断是否为空 */ }
int size = myMap.size();      // 返回键值对数量
```

---

### **8. 排序与自定义比较**
`map` 默认按 key 升序排列。若需自定义排序规则：
```cpp
// 自定义比较函数对象
struct Compare {
    bool operator()(const KeyType& a, const KeyType& b) const {
        return a > b;  // 降序排列
    }
};

map<KeyType, ValueType, Compare> customMap;
```

---

### **9. 底层实现与性能**
- **数据结构**: 红黑树（自平衡二叉搜索树）。
- **时间复杂度**:
  - 插入、删除、查找：`O(log n)`
  - 遍历：`O(n)`

---

### **10. 应用场景**
- 需要按键自动排序的键值存储。
- 高频查找但无需频繁插入/删除的场景。
- 统计词频、字典类应用等。

---

### **11. 注意事项**
- 使用 `operator[]` 访问不存在的 key 会插入默认值，可能导致意外行为。
- 迭代器在插入/删除后可能失效（但红黑树的稳定性较高）。
- 自定义比较函数需满足严格弱序（Strict Weak Ordering）。

---

### **12. 完整示例**
```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<string, int> fruitCount;

    // 插入元素
    fruitCount.insert({"apple", 3});
    fruitCount.emplace("banana", 5);
    fruitCount["cherry"] = 7;

    // 修改元素
    fruitCount["banana"] = 6;

    // 查找与访问
    if (fruitCount.find("apple") != fruitCount.end()) {
        cout << "Apple count: " << fruitCount.at("apple") << endl;
    }

    // 遍历
    for (const auto& [fruit, count] : fruitCount) {
        cout << fruit << ": " << count << endl;
    }

    // 删除元素
    fruitCount.erase("banana");

    return 0;
}
```

---

### **总结**
`map` 适合需要有序键值对的场景，提供高效的查找和遍历操作。若不需要排序，可考虑 `unordered_map`（哈希表实现，平均 O(1) 时间复杂度）。