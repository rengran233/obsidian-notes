（双向队列）
双端数组，可以头插/头删。
中控器维护缓冲区

**构造函数/赋值操作：**
略同于`vector`

**大小：**
略同于`vector`，但没有容量限制

**插入和删除：**
两端插入操作：
`push_back(elem);`          //在容器尾部添加一个数据
`push_front(elem);`        //在容器头部插入一个数据
`pop_back();`                   //删除容器最后一个数据
`pop_front();`                 //删除容器第一个数据

指定位置操作：
`insert(pos,elem);`        //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
`insert(pos,n,elem);`     //在pos位置插入n个elem数据，无返回值。
`insert(pos,beg,end);`    //在pos位置插入\[beg,end)区间的数据，无返回值。
`clear();`                          //清空容器的所有数据
`erase(beg,end);`             //删除\[beg,end)区间的数据，返回下一个数据的位置。
`erase(pos);`                    //删除pos位置的数据，返回下一个数据的位置。

**数据存取：**
略同于`vector`