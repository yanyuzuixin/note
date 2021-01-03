# 设计模式学习笔记

[代码链接]: https://github.com/yanyuzuixin/DesignPattern

## 设计模式七大原则

设计模式原则，其实就是程序员在编程时，应当遵守的原则，也是各种设计模式的基础（即：设计模式为什么这样设计的依据）

设计模式常用的七大原则有

1. 单一职责原则
2. 接口隔离原则
3. 依赖倒转原则
4. 理氏替换原则
5. 开闭原则
6. 迪米特原则
7. 合成复用原则

### 单一职责原则

#### 基本介绍

对类来说的，即一个类应该只负责一项职责。如类 A 负责两个不同职责：职责 1，职责 2。当职责 1 需求变更而改变 A 时，可能造成职责 2 执行错误，所以需要将类 A 的粒度分解为 A1，A2

#### 注意事项和细节

1. 降低类的复杂度，一个类只负责一项职责
2. 提高类的可读性，可维护性
3. 降低变更引起的风险
4. 通常情况下，应该遵守单一职责原则，只有逻辑足够简单，才可以在代码级违反单一职责原则：只有类的方法数量足够少，可以在方法级别保持单一职责原则

### 接口隔离原则

#### 基本介绍

客户端不应该依赖它不需要的接口，即一个类对另一个类的依赖应该建立在最小的接口上

#### 应用实例

类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D

Interface1有5个方法，类A只用到1.2.3方法，类C只用到1.4.5方法，UML类图如下：

![image-20210102150248033](./assets/image-20210102150248033.png)

```mermaid
classDiagram
class Interface1 {
	+operation1()
	+operation2()
	+operation3()
	+operation4()
	+operation5()
}
<<interface>> Interface1

B ..|> Interface1
D ..|> Interface1
Interface1 <.. A
Interface1 <.. C
```



##### 分析

类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须要去实现他们不需要的方法，违背了接口隔离原则

##### 改进方案

将接口Interface1拆分为几个独立的接口，类A和类C分别与他们需要的接口建立依赖关系，如下图：

![image-20210102150937372](./assets/image-20210102150937372.png)

```mermaid
classDiagram
class Interface1 {
	<<interface>>
	+operation1()
}
class Interface2 {
	<<interface>>
	+operation2()
	+operation3()
}
class Interface3 {
	<<interface>>
	+operation4()
	+operation5()
}
B ..|> Interface1
B ..|> Interface2
D ..|> Interface1
D ..|> Interface3
Interface1 <.. A
Interface2 <.. A
Interface1 <.. C
Interface3 <.. C
```





### 依赖倒转原则

#### 基本介绍

1. 高层模块不应该依赖低层模块，二者都应该依赖其抽象
2. 抽象不应该依赖细节，细节应该依赖抽象
3. 依赖倒转的中心思想是面向接口编程
4. 依赖倒转原则是基于这样的理念：相对于细节的多变性，抽象的东西要稳定的多。以抽象为基础搭建的架构比以细节为基础搭建的架构要稳定的多。在java中，抽象指的是借口或抽象类，细节就是具体的实现类
5. 使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成

#### 应用实例

todo：要根据代码实现画图

#### 依赖关系传递的三种方式和应用案例

##### 1 接口传递

```java
// 开关的接口
interface IOpenAndClose {
    public void open(ITV itv); // 抽象方法，依赖ITV接口
}

interface ITV {
    public void play();
}

class ChangHong implements ITV {
    @Override
    public void play() {
        System.out.println("长虹电视 打开");
    }
}

// 实现接口
class OpenAndClose implements IOpenAndClose {
    @Override
    public void open(ITV itv) {
        itv.play();
    }
}
```

##### 2 构造方法传递

```java
interface IOpenAndClose {
    public void open();
}

interface ITV {
    public void play();
}

class ChangHong implements ITV {
    @Override
    public void play() {
        System.out.println("长虹电视 打开");
    }
}

class OpenAndClose implements IOpenAndClose {
    public ITV tv;
    public OpenAndClose(ITV tv) {
        this.tv = tv;
    }

    @Override
    public void open() {
        this.tv.play();
    }
}
```

##### 3 setter方式传递

```java
interface IOpenAndClose {
    public void open();
    public void setTv(ITV tv);
}

interface ITV {
    public void play();
}

class ChangHong implements ITV {
    @Override
    public void play() {
        System.out.println("长虹电视 打开");
    }
}

class OpenAndClose implements IOpenAndClose {
    private ITV tv;

    @Override
    public void setTv(ITV tv) {
        this.tv = tv;
    }

    @Override
    public void open() {
        this.tv.play();
    }
}
```

#### 依赖倒转原则的注意事项和细节

1. 底层模块尽量都要有抽象类和接口，或者两者都有，程序稳定性更好
2. 变量的声明类型尽量是抽象类或接口，这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化
3. 继承时遵循里氏替换原则

### 里氏替换原则

#### OO中的继承性的思考和说明

1. 继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是来设定规范和契约，虽然它不强制要求所有的子类必须遵循这些契约，但是如果子类对这些已经实现的方法任意修改，就会对整个继承体系造成破坏。
2. 继承在给程序设计带来便利的同时，也带来了弊端。比如继承会给程序带来侵入性，程序的可移植性降低，增加对象间的耦合性，如果一个类被其他的类继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能发生故障。
3. 里氏替换原则指导我们怎么正确使用继承。

#### 基本介绍

1. 里氏替换原则是在1988年，由麻省理工学院的一位姓李的女性提出的。
2. 如果对每个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都代换成o2时，程序P的行为没有发生变化，那么类型T2是类型T1的子类型。换句话说，**所有引用基类的地方必须能透明的使用其子类对象**。
3. 在使用继承时，遵循里氏替换原则，**在子类中尽量不要重写父类的方法**
4. 里氏替换原则告诉我们，继承实际上让两个类的耦合性增强了，在适当的情况下，可以通过**聚合、组合、依赖**来解决问题

#### 应用实例

