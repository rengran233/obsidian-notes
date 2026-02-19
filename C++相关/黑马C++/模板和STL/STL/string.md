`char*`：指针
`string`：类内部封装`char*`，管理这个字符串
用`cin`向`string`对象输入，会以`' '`分界，而不是只以`'\n'`分界

**构造函数：**
`string();`          				           //创建一个空的字符串 例如: string str;
`string(const char* s);`	           //使用字符串s初始化
`string(const string& str);`    //使用一个string对象初始化另一个string对象
`string(int n, char c);`            //使用n个字符c初始化 

**赋值操作：**
`string& operator=(const char* s);`             //char*类型字符串 赋值给当前的字符串
`string& operator=(const string &s);`         //把字符串s赋给当前的字符串
`string& operator=(char c);`                          //字符赋值给当前的字符串
`string& assign(const char *s);`                  //把字符串s赋给当前的字符串
`string& assign(const char *s, int n);`   //把字符串s的前n个字符赋给当前的字符串
`string& assign(const string &s);`              //把字符串s赋给当前字符串
`string& assign(int n, char c);`                  //用n个字符c赋给当前字符串

**字符串拼接：**
`string& operator+=(const char* str);`     //重载+=操作符
`string& operator+=(const char c);`          //重载+=操作符
`string& operator+=(const string& str);` //重载+=操作符
`string& append(const char *s); `             //把字符串s连接到当前字符串结尾
`string& append(const char *s, int n);`  //把字符串s的前n个字符连接到当前字符串结尾
`string& append(const string &s);`            //同operator+=(const string& str)
`string& append(const string &s, int pos, int n);`
//字符串s中从pos开始的n个字符连接到字符串结尾
```
string str1 = "Hello";
str1 += " awa ";
string str2 = "World";
str1 += str2 += ':'; //str1 = "Hello awa World:"
string str3;
str3.append(str1, 0, 6).append(str2, 0, 5); //str3 = "Hello World"
```
`+=`运算符可以拼接字符，`append()`函数只能拼接字符串

**查找和替换：**
`int find(const string& str, int pos = 0) const;`             
//查找str第一次出现位置,从pos开始查找
`int find(const char* s, int pos = 0) const; `                    
//查找s第一次出现位置,从pos开始查找
`int find(const char* s, int pos, int n) const; `              
//从pos位置查找s的前n个字符第一次位置
`int find(const char c, int pos = 0) const; `                      
//查找字符c第一次出现位置
`int rfind(const string& str, int pos = npos) const;`     
//查找str最后一次位置,从pos开始查找
`int rfind(const char* s, int pos = npos) const;`             
//查找s最后一次出现位置,从pos开始查找
`int rfind(const char* s, int pos, int n) const;`              
//从pos查找s的前n个字符最后一次位置
`int rfind(const char c, int pos = 0) const;  `                      
//查找字符c最后一次出现位置
`string& replace(int pos, int n, const string& str); `       
//替换从pos开始n个字符为字符串str
`string& replace(int pos, int n,const char* s); `                
//替换从pos开始的n个字符为字符串s
```cpp
string str = "awa awa awa";
int pos = str.find("awa"); //查找起始位置默认为0
int rpos = str.rfind("awa"); //pos = 3, rpos = 8
str.replace(4, 3, "qaq"); //str = "awa qaq awa"
```
`str.find()`如果没有找到目标，不会返回-1，而是回返回`string::npos`（类型为 `size_t`，通常为无符号最大值）
可以用以下代码来判断
```cpp
size_t pos = str.find(s);
if (pos != string::npos)
    cout << pos << '\n';
else cout << -1 << '\n';
```

**字符串比较：**
`int compare(const string &s) const; `  //与字符串s比较
`int compare(const char *s) const;`      //与字符串s比较
`str1.compare(str2)`逐字符比较ASCII码，相等返回0

**字符存取：**
`char& operator[](int n); `     //通过\[]方式取字符
`char& at(int n);   `                    //通过at方法获取字符
访问单个字符：`str[i]`等效于`str.at(i)`，`[]`性能更高，`at()`有边界检查

**字符串插入和删除：**
`string& insert(int pos, const char* s);  `                //插入字符串
`string& insert(int pos, const string& str); `        //插入字符串
`string& insert(int pos, int n, char c);`                //在指定位置插入n个字符c
`string& erase(int pos, int n = npos);`                    //删除从Pos开始的n个字符 
```
string str = "awa";
str.insert(1, "111"); //str = "a111wa"
str.erase(1, 3); //str = "awa"
```

**求子串：**
`string substr(int pos = 0, int n = npos) const;`   
//返回由pos开始的n个字符组成的字符串
```
string str = { "awa qaq awa" };
string subStr = str.substr(str.find('q'), 3); //subStr = "qaq"
```

**其他：**
`str.size()`返回字符串长度
`cout << string(n, ' ');`
`resize(n, ele)`默认填充为`'\0'`空字符，而非`'0'`