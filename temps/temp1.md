针对 Java 考试和基础面试中常见的多线程题型，我为你总结了两套**“万能代码骨架”**。只要看清题意属于哪一类，直接套用并填空即可。

---

### 第一套：并行爆发模板（适用于：下载、计算、不要求顺序的任务）
**题型特征：** 题目要求“并行”、“同时执行”、“改写单线程循环”。（如：你刚才发的那个下载 URL 的题）

#### 🚀 答题模板：
```java
// 1. 确定你的任务逻辑
for (int i = 0; i < 数据长度; i++) {
    // 关键点：抓取循环中的变量，防止多线程竞争导致的索引错乱
    final int index = i; 
    final String data = arr[i];

    // 2. 直接套用匿名内部类开启线程
    new Thread(new Runnable() {
        @Override
        public void run() {
            try {
                // 【填空：把原来循环体里的逻辑原样搬过来】
                // 例如：download(data);
                System.out.println("线程正在处理：" + data);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }).start(); // 3. 别忘了执行 .start()
}
```

---

### 第二套：状态协作模板（适用于：YOLO、生产/销售、按顺序执行）
**题型特征：** 题目要求“生产一个销一个”、“按顺序 A->B->C”、“一个等待另一个”。（如：YOLO 打印题）
$\hat{\theta}$
#### 🚀 答题模板：
这是高分模板，核心是**“一个资源类，多个线程类”**。

**第一步：写资源类（核心大脑）**
```java
class ShareResource {
    private int state = 0; // 状态变量：0代表A，1代表B... (或者用boolean flag)

    // 给每个线程写一个同步方法
    public synchronized void doStepA() throws InterruptedException {
        // 1. 【判断】：不是我的回合就睡（必须用 while）
        while (state != 0) { 
            this.wait(); 
        }
        // 2. 【干活】：打印或生产
        System.out.print("Y");
        // 3. 【更位】：修改状态，指向下一个
        state = 1;
        // 4. 【通知】：叫醒所有人
        this.notifyAll(); 
    }
    
    public synchronized void doStepB() throws InterruptedException {
        while (state != 1) { this.wait(); }
        System.out.print("O");
        state = 2; // 指向下一个
        this.notifyAll();
    }
}
```

**第二步：在 main 中启动线程**
```java
public static void main(String[] args) {
    ShareResource res = new ShareResource(); // 唯一的资源对象（锁）
    
    // 开启多个线程，都操作这一个 res
    new Thread(() -> { try { for(int i=0;i<5;i++) res.doStepA(); } catch(Exception e){} }).start();
    new Thread(() -> { try { for(int i=0;i<5;i++) res.doStepB(); } catch(Exception e){} }).start();
}
```

---

### 💡 绝密“背诵口诀”：多线程答题 4 步走

如果你不确定怎么写，脑子里回想这四个词，代码就出来了：

1.  **锁（Synchronized）**：只要有共享变量，方法前必加 `synchronized`。
2.  **判（Wait）**：只要有顺序要求，必写 `while(条件不符) { wait(); }`。
3.  **作（Work）**：条件满足了，写具体的业务逻辑（打印、加减数量）。
4.  **通（NotifyAll）**：最后一行必写 `this.notifyAll();`。

---

### 🧐 考试避坑指南（写错直接挂）

1.  **start() vs run()**：
    *   写 `t.start()` 才是多线程。
    *   写 `t.run()` 只是普通方法调用，是单线程。
2.  **while vs if**：
    *   在 `wait()` 外面一定要用 `while`。如果写 `if`，老师会认为你基础不扎实，因为 `if` 无法防止虚假唤醒。
3.  **异常处理**：
    *   `wait()` 必须要处理 `InterruptedException`。考试时如果不确定，直接在方法头上 `throws Exception`。
4.  **变量可见性**：
    *   如果是标志位变量（如 `flag`），前面最好加一个 `volatile`，展示你懂内存可见性。

### 总结
*   如果是**“各干各的”**（下载）：用第一套模板（循环 `new Thread`）。
*   如果是**“排队接力”**（YOLO、生产销售）：用第二套模板（`synchronized + wait + notifyAll`）。

只要你能把 **`while-wait-notifyAll`** 这三剑客写出来，多线程的大题基本上就稳拿满分了。