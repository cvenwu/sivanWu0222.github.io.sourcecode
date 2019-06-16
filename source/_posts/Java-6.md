---
title: '[Java]-6'
date: 2016-12-25 10:55:02
tags: 
categories: 
- Java
- 基础
---



## 常用JAVA开发工具

- MyEclipse：收费软件，不过内部集成了很强大的插件，从而称霸武林(使用MyEclipse出错时，可以使用 ctrl + 1 快捷键来提示错误 )
- Eclipse：开源软件

> 作为一个J2EE工作者，我很希望MyEclipse能够开源，毕竟Eclipse都开源了

## Java中的包

> 说到JAVA中的包，其实类似于我们windows系统下的文件夹

1. 包的作用如下
   1. 不同文件夹下的文件可以重名
   2. 方便对文件进行管理
2. 使用包的注意事项：
   1. 包名要全部小写，经常倒着写，例如，可以用网站名字倒着写
   2. 不建议使用默认包

## lang 包

> lang包作为JAVA语言最基础的包之一，下辖了许多类和方法可以供我们使用，其中最长使用的类是Object类，Math类，以及基本数据类型的包装类，下面我们将对这两个进行讲解

<!--more-->

### Object类


> JAVA中的类都是单继承，这里说的单继承是指每个类只能有一个直接父类，而且JAVA中的Object类虽然是每个类的子类，但是每个类的直接父类并不一定是Object类


```Java
public class Dog extends Animal{
	//例如在这个类中,直接父类是Animal，因此直接父类是Animal，并不是Object
	public void bark()
	{
		
	}
}
```

#### clone() 方法讲解

> API 中这样说道 ：“创建并返回此对象的一个副本。”

##### **使用的注意事项：**

> 1. clone（）返回的是一个Object类型，因此我们需要将类型进行转换，转成我们所需要的类型，作为一个设计人员，因为在写clone（）方法的时候，我们不知道如何该方法返回一个何类型，所以先返回Object类型，再进行强转
> 2. 需要声明异常CloneNotSupportedException
> 3. 在使用clone（）的类中，我们实现接口Cloneable


##### **结果注意事项：**

1. 通过clone方法得到的副本，除了地址不同之外其他与原来的一样，克隆生成的是两个独立的对象，可以通过hashcode（）来进行判断，hashcode（）相同则为同一个对象，因为hashcode就是根据地址来生成的
2. 通过clone方法克隆叫做浅度克隆，也就是只克隆基本数据类型，并且值相等，而引用数据类型，我们将不会克隆，但是在深度克隆中，我们将会将引用数据类型以及基本类型都克隆，从而产生新对象，深度克隆通过JAVA中的流来实现

```Java
public class Dog implements Cloneable {
	int age = 10;
	public static void main(String[] args) throws CloneNotSupportedException {
		Dog dog = new Dog();
		Dog dog2 = (Dog) dog.clone();
		System.out.println(dog2.age);
		//10
		System.out.println(dog == dog2);
		//false
	}
}
```

#### equals()方法与 == 讲解

##### **使用的注意事项：**

1. equals方法在Object类中，比较的是两个对象的地址（因为在其他类中，我们可能会对equals（）方法重写），==永远比较两个对象的地址
2. equals方法在包装类中 比较的是 内容

```Java
public class Dog implements Cloneable {
	int age = 10;
	public static void main(String[] args) throws CloneNotSupportedException {
		Dog dog = new Dog();
		Dog dog2 = (Dog) dog.clone();
		System.out.println(dog.equals(dog2));
		//false
		System.out.println(dog == dog2);
		//false
	}
}
```

#### finalize()

> API中说到：当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。 JAVA中的垃圾内存：没有引用指向的内存
> 通过finalize（）：释放对象，回收内存， 而且JAVA是自动回收内存的，不需要程序员实现，比C/C++痛苦轻多了

##### 使用的注意事项：

1. 需要有垃圾
2. 需要启动垃圾回收器 System.gc();

```Java
public class Dog  {
	@Override
	protected void finalize() throws Throwable {
		// TODO Auto-generated method stub
		super.finalize();
		System.out.println("垃圾被回收了");		//有垃圾
	}
	int age = 10;
	public static void main(String[] args) {
		
		Dog dog = new Dog();   //所谓的垃圾内存
		dog = new Dog();
		System.gc();		//充当了一个垃圾回收人员来主动收垃圾
		//输出：垃圾被回收了
	}
}
```

结果分析：有的由于JDK版本不同，偶尔会输出“垃圾被回收了”，因为这里垃圾回收涉及到了JAVA中的一个线程，而且JAVA中垃圾的回收就是通过一个线程进行管理，线程不会时时刻刻都会处在管理状态


----------


### Math类

1. ceil（）：向上取整（大于该数的最小整数）
2. floor（）：向上取整（大于该数的最小整数）
3. round（）：加上0.5，去掉小数点之后的东西，也就是四舍五入
4. Random（）：随机生成一个【0，1.0）的数字

#### Math中涉及到的关于JAVA中的单例设计模式：
类似于Math类，我们只需要对其构造函数进行私有化，因此我们只能在该类中对其进行实例化，所以称为单例设计模式，也就是只允许有1个对象，因为该类所占空间太大，使用单例设计模式的最直接表现就是私有化该类的构造函数，因此属性和方法我们将会加上static修饰符

```Java
public class Test {
	
	public static void main(String[] args) {
		Math math1 = new Math();//这里会报错
		
	}
}
```

**包装类中，== 在new 和 不 new的时候有区分**

```Java
public class Test {
	
	public static void main(String[] args) {
		Integer integer = 10;
		Integer integer2 = new Integer("10");
		System.out.println(integer == integer2);
		System.out.println(integer.hashCode());
		System.out.println(integer2.hashCode());
		System.out.println(integer.equals(integer2));
		/*
		*false
		*10
		*10
		*true
		*/
	}
}
```


----------


### 包装类

> 包装类：所谓的JAVA中的基本数据类型中所对应，进行包装而成的类

**装箱：基本数据类型转换为对应包装类型的过程**
**拆箱：包装类型转换为基本数据类型的过程**

**包装类中，hashcode根据对象内容生成**

> JDK1.4的时候，需要手动进行拆箱和装箱，并且1.4不支持注解

```Java
public class Test {
	
	public static void main(String[] args) {
		Integer integer = new Integer(10);
		Integer integer1 = new Integer(10);
		System.out.println(integer);
		//10
		System.out.println(integer1);
		//10
		//在JDK1.4中，需要我们手动拆装箱
		int i = integer1;
		System.out.println(i);
		//10
	}
}
```

> 需要注意的是：在JDK5.0之后，JAVA引入了自动装箱和拆箱

```Java
public class Test {
	
	public static void main(String[] args) {
		Integer integer = 10;
		Integer integer1 = 10;
		System.out.println(integer);
		//10
		System.out.println(integer1);
		//10
	}
}
```

#### 使用的注意事项：
看到拆装箱这么自如，是不是很想用包装类呢，但是这里需要注意：能用基本数据类型解决问题，我们就用基本数据类型，因为基本数据类型效率高，占用空间小，比如我们项目中所涉及到的客户端传参