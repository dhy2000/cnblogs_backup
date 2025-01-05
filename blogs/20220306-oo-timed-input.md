# OO-定时投放输入的方法

2022-03-06

搬运自 2021 年（去年）面向对象课程讨论区本人编写的帖子，并进行了一些修订。

致谢:

- [BUAA-Wander](https://www.cnblogs.com/BUAA-Wander/) 与 [BUAADreamer](https://www.cnblogs.com/BUAADreamer/) 回帖补充注意事项（这两位大佬均已成为今年的 OO 助教）
- [BUAA-Stargazer](https://www.cnblogs.com/BUAA-Stargazer/) 与 [supercalifragilistic](https://www.cnblogs.com/supercalifragilistic/) 指出代码中的笔误

该内容为去年本人学习 OO 课程时编写，贴出的代码有些简陋，可能并非能完整运行，或者需要一些修改，仅作为解决思路的提示/引导。

完整可运行的定时输入投放工具已于今年重制并开源在 [GitHub](https://github.com/dhy2000/TimeInput)，另有 [Coekjan](https://www.cnblogs.com/coekjan/) 开源的一份[相似工具](https://github.com/Coekjan/Trivial-TimedInput)，可以直接使用，并且无需在作业中引入无关代码。（请勿将上述辅助工具的源代码或二进制**提交到作业仓库**中，如因此导致被查重，**后果自负**）

------

在本次作业中对输入输出的要求是**实时交互**，采用的输入方式为通过控制台 (stdin) 定时投放输入，并且由于程序是多线程，除了输入以外，正在运行的（电梯等）线程也会实时向控制台输出内容，因此**定时投放输入**成为了本次作业测试过程中的一个小难点。本帖给出若干种定时投放输入的调试方法，并欢迎<del>大佬</del>同学们积极补充！

## 手动输入

### 描述

手动在控制台 (console) 中直接输入电梯请求。

### 实现

略。

### 优劣

优点：最原始，不需要额外的工具作为辅助。

缺点：难以精准控制时间；难以投放较多输入；输入会受到输出的干扰（stdin 和 stdout 共用一个控制台）

是否推荐：不推荐。

## 输入发射器+管道

### 描述

编写一个可以定时输出的发射器，利用管道向电梯程序发射输入内容。

### 实现

#### 发射器

发射器从标准输入或者文件中读取**含有时间**的输入信息，并根据读入的时间**定时**将内容输出到标准输出中。

在本帖给出一种较为简陋的 C 语言实现：

```c
// timeInput.c
#include <stdio.h>
#include <time.h>

#ifdef WIN32
#include <windows.h>
#define SLEEP_MS(x) Sleep((x))
#else
#include <unistd.h>
#define SLEEP_MS(x) usleep(1000*(x))
#endif

char buf[1005];
long curMillis;

int main()
{
    long millis;
    while (scanf("%ld:", &millis) != EOF) {
        gets(buf);
        SLEEP_MS(millis - curMillis);
        puts(buf);
        fflush(stdout);
        curMillis = millis;
    }

    return 0;
}
```

对应的输入格式为 `时间:一行内容` ，其中 `时间` 为从开始到投放该行内容的毫秒数，样例：

```
0:Start
1000:Now is 1 second
1500:Now is 1.5 second
3000:Now is 3 second
```

#### 运行方法

将电梯程序打包为 jar （例如 `Elevator.jar` ），并在命令行中运行：

```
timeInput.exe < input.txt | java -jar Elevator.jar
```

### 优劣

优点：采用命令行，便于自动化测试与批量测试

缺点：离开了 IDEA 环境，难以打断点调试。

是否推荐：本地编写评测机批量自测或互测时推荐。

### 注意事项

来自 [BUAA-Wander](https://www.cnblogs.com/BUAA-Wander/) 的回帖：

> 【Tips】第二个方法一定要及时 `fflush`，否则只有当所有输出都写完之后，管道才会把内容流给运行中的 jar

来自 [BUAADreamer](https://www.cnblogs.com/BUAADreamer/) 的回帖:

> 第二个方法里如果遇到了 `bash: timeInput.exe: command not found` 的问题记得使用一下相对路径即： `./timeInput.exe < input.txt | java -jar Elevator.jar`

## 定时任务+管道流

### 描述

采用 Java 的定时任务，从标准输入中直接读取**含时间戳**的输入内容，并按照时间通过**管道流**投放给标准输入接口（ `ElevatorInput` ）

### 实现

#### 概念介绍

- [定时任务](https://www.jb51.net/article/97382.htm)
  
    采用 Java 中的 `Timer` （定时器）类来添加定时任务，通过定时任务来向标准输入接口投放输入。

- [管道流](https://blog.csdn.net/sk199048/article/details/51222719)
  
    Java 中提供了一种用于线程之间相互通讯的流——管道流，分为管道输入流 `PipedInputStream` 和 `PipedOutputStream` ，二者通常成对出现（通过 `connect` 方法进行连接）并分别在不同的线程中使用。

#### 实现思路

首先观察一下官方输入包的源代码，可以看到其提供了两个构造方法：

```java
public ElevatorInput() {...} // 采用 System.in 标准输入流
public ElevatorInput(InputStream inputStream) {...} // 自行指定输入流
```

为了实现可控地向该输入接口定时投放内容，在实例化输入接口时不再采用 `System.in` ，而是采用**管道输入流** （ `PipedInputStream` ）。同时为了让管道输入流能够正常工作，还需要实例化一个与之配对的**管道输出流**并将二者连接。

管道流初始化：

```java
// 实例化一对管道流
PipedOutputStream myPipeOut = new PipedOutputStream();
PipedInputStream myPipeIn = new PipedInputStream();
// 将二者连接起来, PipedOutputStream的connect方法会抛出IOException
try {
    myPipeOut.connect(myPipeIn); 
} catch (IOException e) {
    throw new AssertionError(e); // Never happen
}
```

官方输入包初始化：

```java
ElevatorInput elevatorInput;
if (DEBUG) { // 调试开关, 提交评测时务必关掉它！
    elevatorInput = new ElevatorInput(myPipeIn);
} else {
    elevatorInput = new ElevatorInput(System.in);
}
```

然后我们就可以利用管道输出流对象 `myPipeOut` 向官方包投放输入内容了。

为了避免实时交互的麻烦，我们可以预先将含有时间信息的输入内容准备好，然后开一个线程读取准备好的输入内容，解析出时间信息并通过 `Timer` 的 `schedule` 方法添加定时任务，定时任务的内容为将输入内容输出到管道流中。

```java
public class DebugInput implements Runnable {

    private final OutputStream outputStream;

    public DebugInput(OutputStream outputStream) {
        this.outputStream = outputStream;
    }

    @Override
    public void run() {
        Scanner scanner = new Scanner(System.in);
        Timer timer = new Timer(); // 初始化一个定时器
        long currentMillis = System.currentTimeMillis();
        long maxMillis = 0; // 记录最后一条输入的时间
        while (scanner.hasNext()) {
            long millis = scanner.nextLong(); // 先读取时间
            String input = scanner.next(); // 读取输入内容(不能有空格)
            maxMillis = millis; // 更新maxMillis
            timer.schedule(new TimerTask() { // 创建定时任务
                @Override
                public void run() {
                    try {
                        outputStream.write(input.getBytes());
                        outputStream.write(0x0a); // 换行符
                        outputStream.flush(); // 强制刷新
                    } catch (IOException e) {
                        throw new AssertionError(e);
                    }
                }
            }, new Date(currentMillis + millis));
        }
        // 最后别忘了关闭管道流以及关闭定时器，添加最后一个定时任务
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                try {
                    outputStream.close();
                } catch (IOException e) {
                    throw new AssertionError(e);
                }
                timer.cancel(); // 关闭定时器，不加这句则定时器可能无限等待
            }
        }, new Date(currentMillis + maxMillis + 1));
    }
}
```

注：上面这段代码是从 stdin 中读取含有时间的输入信息，为了方便调试，可以将输入内容存放在文件中，并在 IDEA 配置 Redirect Input。

输入格式: `毫秒数 输入内容` （其中输入内容不能有空格）

```
0 Random
1000 1-FROM-1-TO-2
1500 2-FROM-2-TO-1
```

启动刚才写好的定时投放输入的线程：

```java
new Thread(new DebugInput(myPipeOut)).start();
```

#### 其他要点

采用此方法调试建议加一个**调试开关**，在本地调试时打开，在提交评测时关闭，无需修改代码。（务必保证提交的代码中关闭此调试开关）

### 优劣

优点：便于在 IDEA 中打断点调试，也可以打包成 jar 直接重定向标准输入来测试。

缺点：需要自己在工程中添加类，实现比较麻烦。

是否推荐：本地调试或者本地评测机自测时推荐。

### 方法改进

帖子中给出的输入格式与实际样例的时间格式并不一致，采用了此方法的同学可以考虑如何支持样例给出的时间格式，例如：

```
[0.0]Random
[1.0]1-FROM-1-TO-2
[2.5]2-FROM-5-TO-1
```

（提示: 修改 16 到 17 行代码，可以采用预习作业与第一单元学过的**正则表达式**来提取中括号内的时间与中括号后的输入内容）