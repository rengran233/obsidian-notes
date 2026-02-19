### **1. `std::list` 概述**
- **数据结构**：基于双向链表实现，每个节点包含指向前后节点的指针。
- **核心特性**：
  - **快速插入/删除**：任意位置插入/删除时间复杂度为 **O(1)**（已知迭代器位置时）。
  - **无随机访问**：不支持通过下标（如 `operator[]`）直接访问元素。
  - **内存非连续**：节点分散在内存中，缓存局部性较差，遍历速度通常慢于 `std::vector`。
- **适用场景**：需要高频中间插入/删除操作，且不需要随机访问。

---

### **2. `std::list` 核心特点**

| **特性**              | **说明**                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| **插入/删除效率**      | 任意位置插入/删除时间复杂度为 O(1)（已知迭代器位置）                      |
| **内存分配**           | 动态分配节点内存，无容量（`capacity`）概念                              |
| **迭代器失效**         | 插入操作不会使迭代器失效（除非指向被删除元素）                            |
| **空间额外开销**       | 每个节点需存储前驱和后继指针，内存占用较高                               |

---

### **3. 常用成员函数**

#### **3.1 构造函数**
```cpp
std::list<int> list1;                // 空列表
std::list<int> list2(5, 10);         // 包含5个值为10的元素
std::list<int> list3(list2.begin(), list2.end()); // 范围构造
std::list<int> list4(list3);         // 拷贝构造
```

#### **3.2 元素访问**
```cpp
std::list<int> lst = {1, 2, 3};
int front_val = lst.front();   // 首元素（1）
int back_val  = lst.back();    // 末元素（3）
```

#### **3.3 修改操作**
```cpp
lst.push_front(0);      // 头部插入元素（0）
lst.push_back(4);       // 尾部插入元素（4）
lst.pop_front();        // 删除头部元素
lst.pop_back();         // 删除尾部元素

auto it = lst.begin();
advance(it, 1);         // 移动迭代器到第2个元素
lst.insert(it, 5);      // 在位置1插入5 → [1,5,2,3]
lst.erase(it);          // 删除元素2 → [1,5,3]

lst.resize(5);          // 调整大小为5，新增元素默认初始化
lst.clear();            // 清空所有元素
```

#### **3.4 大小与容量**
```cpp
bool is_empty = lst.empty();  // 判断是否为空
size_t size = lst.size();     // 元素个数
```

#### **3.5 迭代器操作**
```cpp
auto begin_it = lst.begin();  // 指向首元素的迭代器
auto end_it   = lst.end();    // 指向末尾（尾后位置）的迭代器
auto rbegin_it = lst.rbegin(); // 反向迭代器（从尾部开始）
auto rend_it   = lst.rend();  // 反向尾后迭代器
```

#### **3.6 特殊操作（链表特有）**
```cpp
// 合并两个有序链表（合并后 lst2 为空）
lst.sort();             // 排序（升序）
lst.merge(lst2);        // 合并

// 拼接：将 lst2 的部分或全部元素移动到当前链表
auto pos = lst.begin();
lst.splice(pos, lst2);           // 将 lst2 全部元素插入到 pos 前
lst.splice(pos, lst2, it);       // 移动 lst2 中迭代器 it 指向的元素
lst.splice(pos, lst2, it1, it2); // 移动 lst2 中 [it1, it2) 范围的元素

lst.remove(3);          // 删除所有值为3的元素
lst.remove_if([](int x) { return x % 2 == 0; }); // 删除所有偶数

lst.unique();           // 删除连续重复元素（需先排序）
lst.reverse();          // 反转链表顺序
```

---

### **4. 示例代码**

#### **基本操作示例**
```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> nums = {2, 4, 6};

    // 插入元素
    nums.push_front(0);             // 头部插入 → [0,2,4,6]
    nums.push_back(8);              // 尾部插入 → [0,2,4,6,8]
    
    // 遍历链表
    for (int num : nums) {
        std::cout << num << " ";    // 输出: 0 2 4 6 8
    }

    // 删除中间元素
    auto it = nums.begin();
    std::advance(it, 2);            // 移动到第3个元素（4）
    nums.erase(it);                 // 删除4 → [0,2,6,8]

    // 反转链表
    nums.reverse();                 // → [8,6,2,0]

    return 0;
}
```

#### **合并与排序示例**
```cpp
std::list<int> listA = {5, 3, 1};
std::list<int> listB = {6, 4, 2};

listA.sort();   // listA → [1,3,5]
listB.sort();   // listB → [2,4,6]
listA.merge(listB);  // listA → [1,2,3,4,5,6], listB 为空
```

---

### **5. 注意事项**
1. **迭代器失效**：
   - 删除操作会使指向被删元素的迭代器失效，其他迭代器不受影响。
   - 插入操作不会使任何迭代器失效。

2. **性能对比**：
   - **遍历速度**：慢于 `std::vector`（因内存不连续）。
   - **内存占用**：每个元素额外存储两个指针（前驱和后继）。

3. **选择建议**：
   - 优先使用 `std::vector`，除非需要高频中间插入/删除。
   - 需要随机访问时，避免使用 `std::list`。

---

### **6. 总结**
`std::list` 是处理频繁插入/删除操作的理想选择，尤其适合以下场景：
- 需要维护元素的插入顺序且频繁在中间操作。
- 对迭代器稳定性要求高（如长期持有迭代器）。
- 不依赖随机访问和高速遍历性能。

通过合理利用其特有的成员函数（如 `splice`、`merge`），可以高效实现复杂链表操作。