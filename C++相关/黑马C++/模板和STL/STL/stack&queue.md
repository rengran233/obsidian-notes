`stack`栈，先进后出
构造函数：
* `stack<T> stk;`                                 //stack采用模板类实现， stack对象的默认构造形式
* `stack(const stack &stk);`            //拷贝构造函数
赋值操作：
* `stack& operator=(const stack &stk);`           //重载等号操作符
数据存取：
* `push(elem);`      //向栈顶添加元素
* `pop();`                //从栈顶移除第一个元素
* `top(); `                //返回栈顶元素
大小操作：
* `empty();`            //判断堆栈是否为空
* `size(); `              //返回栈的大小

`queue`队列，先进先出
数据存取：
- `push(elem);`                             //往队尾添加元素
- `pop();`                                      //从队头移除第一个元素
- `back();`                                    //返回最后一个元素
- `front(); `                                  //返回第一个元素
大小操作：
- `empty();`            //判断堆栈是否为空
- `size(); `              //返回栈的大小

栈和队列不允许遍历