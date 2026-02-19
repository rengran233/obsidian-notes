new返回指针
int \*p = new int(10) //10
int \*p = new int\[10] //10数据数组

可以用new关键字创建动态结构
*结构类型*  * p = new *结构类型*
可以用指针和"->"字符定位没有名称的结构体
[[指向结构体成员]]

delete释放堆区数据
delete p //释放
delete [] p //释放数组