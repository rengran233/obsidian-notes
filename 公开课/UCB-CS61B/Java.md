内置数据类型之间赋值: 复制
自定义: 引用
自定义数据的内存空间储存地址, 而赋值操作复制这一地址


如果没有引用指向一块内存空间, Java会自动释放这块内存而不会内存泄漏

the exact memory address is below the level of abstraction accessible to programers in Java

reference type: 64(in earlier JVMs) or 96 bits of pointer

We almost always pass parameters by value

An enclosing class is a class that contains another class inside it.

inner classes and static nested classes both in the field of enclosing class, but inner classes must instantiate by a enclosing class instance while static nested classes can instantiate through field straightly.
```java
OuterClass.InnerClass ic = new OuterClass.InnerClass();
OuterClass oc;
OuterClass.StaticNestedClass snc = oc.new StaticNestedClass();
```

#### 2.4 Arrays
```java
int[] arr = {1, 2, 3};
int[] arr; arr = new int[]{1, 2, 3};


// create an array with 4 null
int[][] matrix = new int[4][];
// create an array with 4 arrays with 4 0-bit
int[][] matrix = new int[4][4];
```

#### 3.1 Test
TDD(test-driven development): write tests before implements
unit test: break down codes into small pieces and test each one
Integration test: higher level of unit test focus on interacting of units
