# JVM基础知识

Classloader加载一个类并把类型信息保存到方法区后，会创建一个Class对象，存放在堆区的，不是方法区，这点很多人容易犯错，它为程序提供了访问类型信息的方法。



## 双亲委派机制 

作用：如果有人想替换系统级别的类：String.java。篡改它的实现，但是在这种机制下这些系统的类已经被Bootstrap classLoader加载过了，所以并不会再去加载，从一定程度上防止了危险代码的植入。

概念：什么是双亲委派机制？

当某个类加载器需要加载某个`.class`文件时，它首先把这个任务委托给他的上级类加载器，递归这个操作，如果上级的类加载器没有加载，自己才会去加载这个类。

到这里需要了解一下Classloader的类别：

#### BootstrapClassLoader（启动类加载器）

`c++`编写，加载`java`核心库 `java.*`,构造`ExtClassLoader`和`AppClassLoader`。由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不允许直接通过引用进行操作

#### ExtClassLoader （标准扩展类加载器）

`java`编写，加载扩展库，如`classpath`中的`jre` ，`javax.*`或者
 `java.ext.dir` 指定位置中的类，开发者可以直接使用标准扩展类加载器。

#### AppClassLoader（系统类加载器）

```
java`编写，加载程序所在的目录，如`user.dir`所在的位置的`class
```

#### CustomClassLoader（用户自定义类加载器）

`java`编写,用户自定义的类加载器,可加载指定路径的`class`文件

双亲委派机制的流程图：

![img](https://upload-images.jianshu.io/upload_images/7634245-7b7882e1f4ea5d7d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

流程简述：

1. 类加载器收到类加载的请求
2. 将这个请求向上委托给父类加载器完成，一直到启动类加载器（根加载器），
3. 根加载器检查是否能加载当前类，嫩能加载就使用当前加载器，否则抛出异常，通知子加载器进行加载，
4. 重复步骤三

顺序：根加载器-->扩展加载器-->统类加载器-->用户自定义类加载器



## JVM内存模型

![img](https://img-blog.csdnimg.cn/20200518153026798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU5NjcyNQ==,size_16,color_FFFFFF,t_70)

**类装载器**（ClassLoader）（用来装载.class文件，加载到运行时数据区）
**执行引擎**（执行字节码，或者执行本地方法）
**运行时数据区**（方法区、堆、虚拟机栈（java栈）、本地方法栈、程序计数器）

| **名称**     | **特征**                                                 | **作用**                                                     | **配置参数**                       | **异常**                           |
| ------------ | -------------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------- | ---------------------------------- |
| 程序计数器   | 占用内存小，线程私有，生命周期与线程相同                 | 大致为字节码行号指示器                                       | 无                                 | 无                                 |
| 虚拟机栈     | 线程私有，生命周期与线程相同，使用连续的内存空间         | Java 方法执行的内存模型，存储局部变量表、操作栈、动态链接、方法出口等信息 | -Xss                               | StackOverflowErrorOutOfMemoryError |
| java堆       | 线程共享，生命周期与虚拟机相同，可以不使用连续的内存地址 | 保存对象实例，所有对象实例（包括数组）都要在堆上分配         | -Xms-Xsx-Xmn                       | OutOfMemoryError                   |
| 方法区       | 线程共享，生命周期与虚拟机相同，可以不使用连续的内存地址 | 存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据 | -XX:PermSize:16M-XX:MaxPermSize64M | OutOfMemoryError                   |
| 运行时常量池 | 方法区的一部分，具有动态性                               | 存放字面量及符号引用                                         |                                    |                                    |

## GC机制

### 发生地点：

由于程序计数器，虚拟机栈，本地方法栈，这三个区域随线程而生，随线程而死，这几个区域内存分配和回收都具有确定性，这几个区域不用过多的考虑回收，因为线程结束，或者方法结束时，内存自然就回收了，而java的堆和方法区不一样，一个接口中多个实现类所需要的内存可能不一样，一个方法中的多个分支，需要的内存也可能不一样，我们只有在程序运行期间，才知道会创建那些对象，这部分的内存分配和回收都是动态的，垃圾回收器主要关注这个部分。

因此==发生GC回收的地方是堆和方法区==



### 如何知道对象是否可回收

- **引用计数法**
  给对象加一个引用计数器，每当有地方引用他时就加一，引用失效时就减一，任何时刻，引用计数为0 就是没有被引用的，但他很难解决，对象互相引用的情况，所以虚拟机并没有使用这种方法。
- **可达性算法**
  在主流的实现中都是采用可达性算法，来判定对象是否存活，这个算法的基本思路是通过一个“GC Root” 的对象作为起点，从这些起点向下搜索，搜索做过的路径叫做引用链，当一个对象到“GC Root” 没有任何引用链时，说明此对象是不可用的

**在java语言中，可作为GC Root 的对象包括以下几种**

- 虚拟机栈中的引用对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法占中jin引用的对象

### 垃圾回收算法

- **标记-清除算法**
  该算法分为标记和清除俩个阶段，首先要标记出需要回收的各个对象，在标记完成后统一回收被标记的对象， 他有俩个不足
  1 效率问题：标记清除俩个过程效率都不是很高
  2 空间问题：会产生大量的不连续空间
  ![这里写图片描述](https://img-blog.csdn.net/20180705172026432?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzYwNTA4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- **复制算法**
  为了解决效率问题，他可以将内存划分为完全相同的俩块，每次只使用其中的一块，当这块用完了，就把还存活的复制到另一块上去，然后把已经使用过的内存空间一次清理掉，不足，会把内存缩小为原来的一半
  ![这里写图片描述](https://img-blog.csdn.net/20180705172107789?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzYwNTA4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- **标记整理算法**
  复制算法在存活较多的情况下，效率较低，而且会浪费掉50%的空间 ，所以老年代不能选择这种算法，根据老年代存活率特别高的特点，又提出一种 标记整理的算法，标记过程和“标记清除” 一样，但后续步骤不是对可回收对象进行清理，而是让所有存活的对象，向一端移动，然后直接清理掉端以外的内存
  ![这里写图片描述](https://img-blog.csdn.net/20180705172533125?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzYwNTA4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- **分代收集算法**
  当前虚拟机大部分采用，分代收集算法，这种算法并没有特别思想，只是根据对象的存活周期不同把内存划分为几块，一般是吧java堆分为新生代和老年代，这样就可以根据年代的特点采用不同的算法，提高效率，新生代每次垃圾回收都会有大量的对象死去，少量存活，那就用复制算法，老年代存活率较低，那就使用标记-清除，或标记-整理法

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190821141912419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzYwNTA4,size_16,color_FFFFFF,t_70)

- **新生代（Young Generation**）
  大多数对象在新生代中创建，其中很多对象的生命周期很短，每次新生代的垃圾回收（又称 **Minor GC**），只有少量对象存活，所以选择复制算法，因为少量的复制成本就可以完成
  新生代又分为三个区，一个**Eden**区，两个**Survivor**区（一般而言），大部分对象在**Eden**区中生成，当Eden区满了之后，还存活的对象复制到Survivor区中的一个，当这个Survivor区满了之后，此区存活但不满足**晋升**条件的对象，复制到另一个Survivor区，对象每一次Minor GC年龄加一，达到年龄的阈值后，晋升老年区，默认的阈值为15岁
- **老年代（Old Generation）**
  新生代经历n次垃圾回收，还存活的对象就会被放到老年代，此区域中对象存活率高，老年代的垃圾回收，通常用标记清理和标记整理的方法，整堆包括新生代和老年代的垃圾回收称为**Full GC**
- **永久代（Perm Generation）**
  主要存放元数据，如Class何Method的元数据，与垃圾回收对象的关系不大，相对于新生代和老年代来说，该区划分对垃圾回收影响较小

### 什么时候回收

**GC的类型：**

- 分配内存不够引起的GC，会stop world，是并发GC，其他线程都会停止，直到GC完成
  - 当你new一个新的对象时，如果内存不够会进行**Full GC**
- 内存达到一定的阈值，进行的GC，是一个后台GC，不会stop world
  - 新生代 GC（Minor GC）：指发生在新生代的垃圾收集动作，因为 Java 对象大多都具备朝生夕灭的特性，所以 Minor GC 非常频繁，一般回收速度也比较快。
  - Minor GC触发条件：当Eden区满时，触发Minor GC。
  - 老年代 GC（Full GC ）：指发生在老年代的 GC，经常会伴随至少一次的 Minor GC 。MajorGC 的速度一般会比 Minor GC 慢 10倍以上。
  - Full GC触发条件：（1）老年代空间不足（2）升到老年代的对象大于老年代剩余空间
- 显示调用时进行的GC，显式调用System.gc时会调用Full GC