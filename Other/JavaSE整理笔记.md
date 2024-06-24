常见数据结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/46a023cecf3446f9934c6dc6084df69f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA57eR5rC06ZW35rWBKno=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)



# 面向对象

### 面向对象

对现实中的事务都抽象为对象。每个对象是唯一的，且都可以拥有它的属性与行为。我们可以通过调用这些对象的方法、属性去解决问题。面向对象的基本特征：封装、 继承、多态。

封装：隐藏对象内部比较敏感的内容，只对外暴露出简单的接口。体现：把类中的属性私有化，只提供公共的方法来get set。

继承：让某个子类获得父类的属性和方法，关键字：extends 。

多态：多态是在不同继承关系的类对象，去调用同一函数，产生了不同的行为。
多态存在的三个必要条件：继承、重写、父类引用指向子类对象：**Parent p = new Child();**（向上转型）

```
面向对象的三大特性：封装、继承、多态
面向对象的五大基本原则："单一职责原则"、"开放封闭原则"、"里氏替换原则"、"依赖倒置原则"、"接口分离原则"
1.单一职责原则：每一个类应该专注于做一件事情。
2.里氏替换原则：在一个基类对象替换成子类对象，程序不会产生任何错误，反过来不成立。
3.依赖倒置原则：具体依赖抽象，上层依赖下层。实现尽量依赖抽象，不依赖具体实现。
4接口隔离原则：应当为客户端提供尽可能小的单独的接口，而不是提供大的总的接口。
5.开闭原则：面向扩展开放，面向修改关闭。
```





### 方法的重载(overload)

```java
/*
 * 方法的重载(overload) loading...
 * 
 * 1.定义:在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可。
 * 	
 * 		“两同一不同”:同一个类、相同方法名
 * 				  参数列表不同：参数个数不同，参数类型不同  注意：返回类型不影响
 * 
 * 2.举例:
 * 		Arrays类中重载的sort() / binarySearch()
 * 
 * 3.判断是否重载
 * 		与方法的返回值类型、权限修饰符、形参变量名、方法体都无关。只跟“两同一不同”
 * 
 * 4.在通过对象调用方法时，如何确定某一个指定的方法：
 * 		方法名---》参数列表
 */
public class OverLoadTest {
	
	public static void main(String[] args) {
		OverLoadTest test = new OverLoadTest();
		test.getSum(1, 2);	//调用的第一个，输出1
	}

	//如下的四个方法构成了重载
	public void getSum(int i,int j){
		System.out.println("1");
	}
	public void getSum(double d1,double d2){
		System.out.println("2");
	}
	public void getSum(String s,int i){
		System.out.println("3");
	}
	
	public void getSum(int i,String s){
		
	}
	//以下3个是错误的重载
//	public int getSum(int i,int j){
//		return 0;
//	}
	
//	public void getSum(int m,int n){
//		
//	}
	
//	private void getSum(int i,int j){
//		
//	}
}
```

### 方法参数的值传递机制

*如果参数是基本数据类型，此时实参赋值给形参的是实参真是存储的数据值。*
*如果参数是引用数据类型，此时实参赋值给形参的是实参存储数据的地址值。*
注意：String是不可变

### 基本数据类型和引用数据类型

一共是8种基本数据类型

4种整数型
2种浮点型
1种字符型
1种布尔型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201123154950471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2OTg4OTM1,size_16,color_FFFFFF,t_70#pic_center)

char 为2 字节（byte），boolean 4字节

### String类型

注意String不属于基本数据类型！！！
new String()和new String(“”)都是申明一个新的空字符串，是空串不是null；

**创建字符串和new方式创建字符串的区别**
以“ ”方式创建的字符串，只要字符内容相同，无论在程序代码中出现几次，[JVM](https://so.csdn.net/so/search?q=JVM&spm=1001.2101.3001.7020) 都只会建立一个 String 对象，并在字符串常量池中维护。
字符串常量池：当使用双引号创建字符串对象的时候，系统会检查该字符串是否在字符串常量池中存在

- 不存在：创建
- 存在：不会重新创建，而是直接复用
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201107193639185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dra2xqeQ==,size_16,color_FFFFFF,t_70#pic_center)

new方式创建字符串
通过 new 创建的字符串对象，每一次 new 都会申请一个内存空间，虽然内容相同，但是地址值不同
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201107193708349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dra2xqeQ==,size_16,color_FFFFFF,t_70#pic_center)

### this的使用

```Java
/*
 * this 关键字的使用
 * 1.this 用来修饰、调用：属性、方法、构造器
 * 
 * 2.this 修饰属性和方法:
 * 		this 理解为：当前对象,或当前正在创建的对象。
 * 	 
 *  2.1 在类的方法中，我们可以使用"this.属性"或"this.方法"的方式，调用当前对象属性和方法。
 *  	通常情况下，我们都选择省略“this.”。特殊情况下，如果方法的形参和类的属性同名，我们必须显式
 *  	的使用"this.变量"的方式，表明此变量是属性，而非形参。
 * 
 *  2.2 在类的构造器中，我们可以使用"this.属性"或"this.方法"的方式，调用正在创建的对象属性和方法。
 *  	但是，通常情况下，我们都选择省略“this.”。特殊情况下，如果构造器的形参和类的属性同名，我们必须显式
 *  	的使用"this.变量"的方式，表明此变量是属性，而非形参。
 *  
 *  3.this 调用构造器
 *  	① 我们可以在类的构造器中，显式的使用"this(形参列表)"的方式，调用本类中重载的其他的构造器！
 *  	② 构造器中不能通过"this(形参列表)"的方式调用自己。
 *  	③ 如果一个类中声明了n个构造器，则最多有n -1个构造器中使用了"this(形参列表)"。
 *  	④ "this(形参列表)"必须声明在类的构造器的首行！
 *  	⑤ 在类的一个构造器中，最多只能声明一个"this(形参列表)"。
 */
public class PersonTest {

	public static void main(String[] args) {
		Person p1 = new Person();
		
		p1.setAge(1);
		System.out.println(p1.getAge());
		
		p1.eat();
		System.out.println();
		
		Person p2 = new Person("jerry" ,20);
		System.out.println(p2.getAge());
	}
}
class Person{
	
	private String name;
	private int age;
	
	public Person(){
		this.eat();
		String info = "Person 初始化时，需要考虑如下的 1,2,3,4...(共 40 行代码)";
		System.out.println(info);
	}
	
	public Person(String name){
		this();
		this.name = name;
	}
	
	public Person(int age){
		this();
		this.age = age;
	}
	
	public Person(String name,int age){
		this(age);	//调用构造器的一种方式
		this.name = name;
	}
	
	public void setNmea(String name){
		this.name = name;
	}
	public String getName(){
		return this.name;
	}
	public void setAge(int age){
		this.age = age;
	}
	public int getAge(){
		return this.age;
	}
	public void eat(){
		System.out.println("人吃饭");
		this.study();
	}
	public void study(){
		System.out.println("学习");
	}
}
```

### 重载和重写区别-多态

```java
/*
 * 方法的重写(override/overwrite)
 * 
 * 1.重写：子类继承父类以后，可以对父类中的方法进行覆盖操作。
 * 2.应用：重写以后，当创建子类对象以后，通过子类对象去调用子父类中同名同参数方法时，执行的是子类重写父类的方法。
 *   即在程序执行时，子类的方法将覆盖父类的方法。
 * 
 * 面试题：区分方法的重载与重写(有的书也叫做“覆盖”)
 * 		答：方法的重写Overriding和重载Overloading是Java多态性的不同表现。
 * 		重写Overriding是父类与子类之间多态性的一种表现，重载Overloading是一个类中多态性的一种表现。
 * 		如果在子类中定义某方法与其父类有相同的名称和参数，我们说该方法被重写 (Overriding)。
 * 		如果在一个类中定义了多个同名的方法，它们或有不同的参数个数或有不同的参数类型，则称为方法的重载。
 * 		重写要求参数列表相同；重载要求参数列表不同。 重载方法可以改变返回值的类型，覆盖方法不能改变返回值的类型。
 */
```

![image-20220818203337652](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220818203337652.png)

### super关键字

```java
/*
 * super关键字的使用
 * 1.super理解为:父类的
 * 2.super可以用来调用:属性、方法、构造器 
 * 
 * 3.super的使用
 * 		3.1 我们可以在子类的方法或构造器中，通过"super.属性"或"super.方法"的方式，显式的调用
 * 	父类中声明的属性或方法。但是，通常情况下，我们习惯去省略这个"super."
 * 		3.2 特殊情况:当子类和父类中定义了同名的属性时，我们要想在子类中调用父类中声明的属性，则必须显式的 
 *  使用"super.属性"的方式，表明调用的是父类中声明的属性。
 *  	3.3 特殊情况:当子类重写了父类中的方法后，我们想在子类的方法中调用父类中被重写的方法时，必须显式的
 *  使用"super.方法"的方式，表明调用的是父类中被重写的方法。
 * 
 * 4.super调用构造器
 * 	  4.1  我们可以在子类的构造器中显式的使用"super(形参列表)"的方式,调用父类中声明的指定的构造器
 * 	  4.2 "super(形参列表)"的使用，必须声明在子类构造器的首行！
 *    4.3 我们在类的构造器中，针对于"this(形参列表)"或"super(形参列表)"只能二选一，不能同时出现。
 *    4.4 在构造器的首行，既没有显式的声明"this(形参列表)"或"super(形参列表)",则默认的调用的是父类中的空参构造器。super()
 *    4.5 在类的多个构造器中，至少有一个类的构造器使用了"super(形参列表)",调用父类中的构造器。 
 */
```

### final，finally，finalize的区别

```
final关键字
  1、final用于修饰变量：表示为一个常量，类型和值都不能变化
  2、final用于修饰方法：表示该方法不能被子类重写
  3、final用于修饰类：表示该类不能被继承，因此final 不能修饰抽象类。
  4、static final：表示全局常量，所有对象共享的变量，命名规则一般为全大写，在类加载（1.类声明时初始化2.静态代码块）时初始化，效率较高，通过类名调用访问。
  
final是怎么保证不变性呢？
设置final变量的原理
final 变量的赋值也会通过 putfield 指令来完成，同样在这条指令之后也会加入写屏障，保证在其它线程读到它的值时不会出现为 0 的情况
获取final变量的原理
获取 final 变量时，根据字节码指令， 数字较小，复制到自己的栈中读取，较大，超出短整型，在常量池读取。是直接把final变量的值，复制到该（方法）线程的操作数栈中，即复制到其他类中，没有共享的操作。
  
 finally
  用于Java异常体系，和try-catch搭配使用，其中所放的是无论程序是否出现异常都会执行的代码块。

 finalize
  Object类中的方法，所有Java类都可以调用的方法，表示在进行判断对象是否已死时，当JVM首次调用finalize()方法时，且在finalize()方法中添加该对象的引用，则该对象会起死回生，但该方法只会被调用一次。
```

String 为啥不可变

`String` 类中使用 `final` 关键字修饰字符数组来保存字符串。

`String` 真正不可变有下面几点原因：

1. 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。
2. `String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。





### ==操作符与[equals](https://so.csdn.net/so/search?q=equals&spm=1001.2101.3001.7020)方法

```java
/*
 * 面试题: ==和equals的区别
 * 
 * 一、回顾==的使用
 * == : 运算符
 * 1.可以使用在基本数据类型变量和引用数据类型变量中
 * 2.如果比较的是基本数据类型变量：比较两个变量保存的数据是否相等。(不一定类型要相同)
 * 	 如果比较的是引用数据类型变量：比较两个对象的地址值是否相同,即两个引用是否指向同一个对象实体
 *  补充: == 符号引用数据类型变量使用时，必须保证符号左右两边的变量类型一致。
 *
 * 二、equals()方法的使用
 * 1.是一个方法，而非运算符
 * 2.只能适用于引用数据类型。
 * 3.Object类中equals()的定义：
 * 		public boolean equals(Object obj){
 * 			return (this == obj);
 * 		}
 * 说明：Object类中定义的equals()的作用是比较两个对象的地址值是否相同。
 * 4.像String、Date、File、包装类等都重写了Object类中的equals()方法.重写成比较内容是否相等

```

### hashcode 与equals

```
1.hashcode是什么?
hashcode代表对象的地址在hash表中的位置, 
作用：主要是为了查找的快捷性，

equals方法和hashcode的关系？
1、如果两个对象equals相等，那么这两个对象的HashCode一定也相同
2、如果两个对象的HashCode相同，不代表两个对象就相同，只能说明这两个对象在散列存储结构中，存放于同一个位置
```

![image-20230418154724291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230418154724291.png)

### static关键字

```java
 1.static:静态的。
 * 2.static 可以用来修饰:属性、方法、代码块、内部类。
 * 
 * 3.使用 static 修饰属性:静态变量(或类变量)。
 * 		3.1  属性:是否使用 static 修饰，又分为:静态属性 VS 非静态属性(实例变量)
 * 		   实例变量:我们创建了类的多个对象，每个对象都独立的拥有了一套类中的非静态属性。
 * 				当修改其中一个非静态属性时，不会导致其他对象中同样的属性值的修饰。
 * 		   静态变量:我们创建了类的多个对象，多个对象共享同一个静态变量。当通过静态变量去修改某一个变量时，
 * 				会导致其他对象调用此静态变量时，是修改过的。
 * 		3.2 static 修饰属性的其他说明:
 * 			① 静态变量随着类的加载而加载。可以通过"类.静态变量"的方式进行调用。
 * 			② 静态变量的加载要早于对象的创建。
 * 			③ 由于类只会加载一次，则静态变量在内存中也只会存在一次。存在方法区的静态域中。
static方法是属于类的，非实例对象，在JVM加载类时，就已经存在内存中，不会被虚拟机GC回收掉，这样内存负荷会很大，但是非static方法会在运行完毕后被虚拟机GC掉，减轻内存压力
```

### 静态代码块与动态代码块

```java
/*
 * 类的成员之四:代码块（或初始化块）
 * 1.代码块的作用：用来初始化类、对象的
 * 2.代码块如果有修饰的话，只能使用 static
 * 3.分类:静态代码块 vs 非静态代码块
 * 4.静态代码块
 * 	》内部可以有输出语句
 *  》随着类的加载而执行,而且只执行一次
 *  》作用:初始化类的信息
 *  》如果一个类中，定义了多个静态代码块，则按照声明的先后顺序执行
 *  》静态代码块的执行，优先于非静态代码块的执行
 *  》静态代码块内只能调用静态的属性、静态的方法，不能调用非静态的结构
 * 5.非静态代码块
 *  >内部可以有输出语句
 *  >随着对象的创建而执行
 *  >每创建一个对象，就执行一次非静态代码块。
 *  >作用:可以在创建对象时，对对象的属性等进行初始化。
 *  >如果一个类中，定义了多个非静态代码块，则按照声明的先后顺序执行
 *  >非静态代码块内可以调用静态的属性、静态的方法，或非静态的属性、非静态的方法。
```

### 抽象类与接口类对比

```
抽象类特点 
1、抽象类不能被实例化，即不能使用new关键字来实例化对象，只能被继承；
2、包含抽象方法的一定是抽象类，但是抽象类不一定含有抽象方法；
3、抽象类中的抽象方法的修饰符只能为public或者protected，默认为public；
4、抽象类中的抽象方法只有方法体，没有具体实现；
5、如果一个子类实现了父类（抽象类）的所有抽象方法，那么该子类可以不必是抽象类，否则就是抽象类；
6、抽象类可以包含属性、方法、构造方法，但是构造方法不能用于实例化，主要用途是被子类调用。

接口类的特点
1、接口可以包含变量、方法；变量被隐式指定为public static final，方法被隐式指定为public abstract
2、接口支持多继承，即一个接口可以extends多个接口，间接的解决了Java中类的单继承问题；
3、一个类可以实现多个接口；接口不能被实例化

抽象类和接口类的相同点
1.都不能被实例化
2.都可以包含抽象方法。

抽象类和接口类的区别
区别1：定义关键字不同     接口使用关键字 interface                 	抽象类使用关键字 abstract
区别2：继承或实现的不同    接口类的子类使用implements   			抽象类的子类使用extends继承
区别3：子类扩展的数量不同    一个子类可以实现多个接口     				一个子类只能继承一个父类
区别4：属性访问控制符不同    接口中的属性访问控制符只能是public  			 而抽象类的属性随意
区别5：方法控制符不同        一样是只能为public      				抽象类的方法控制符无限制

```

### 继承和实现

```
继承和实现的区别：
1、数量不同：java只支持接口的多实现，不支持多继承，继承在java中具有单根性，子类只能继承一个父类。总结就是：单继承，多实现。
2、修饰不同：继承：extends;实现：iimplements
3、属性不同：在接口中只能定义全局变量和无实现的方法。而在继承中可以定义属性方法，变量，常量等。
4、调用不同：当接口被类实现时，在类中一定要实现接口中的抽象方法；而继承想调用哪个方法就调用哪个方法。
```

### 堆和栈内存

```
栈(Stack)是操作系统在创建进程或者线程时候自动为其分配的内存空间，存放基本数据类型变量和对象的引用变量，以及方法的调用返回等数据。
而堆(Heap)是应用程序在运行时请求操作系统分配给自己的内存空间，堆内存用于存放由 new 创建的对象和数组

维护机制不同：栈是自行维护。堆受垃圾处理器GC管理
当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁。

当创建一个对象时，这个对象讲被保存到堆内存。只有这个对象不被其他对象引用时，才会被垃圾回收器回收。
```

### swtich作用类型

```
switch可以作用于：byte,short,int,char,String,枚举类型上，其余类型是不允许的。
```



# 异常

```java
 一、java异常体系结构
 * 
 * java.lang.Throwable
 * 		|----java.lang.Error:一般不编写针对性的代码进行处理
 * 		|----java.lang.Exception:可以进行异常处理
 * 			|----编译时异常(checked)
 * 				|----IOEXception
 * 					|----FileNotFoundException
 * 				|----ClassNotFoundException
 * 			|----运行时异常(unchecked)
 * 				|----NullPointerException
 * 				|----ArrayIndexOutOfBoundsException  数组越界
 * 				|----ClassCaseException              类型转换错误
 * 				|----NumberFormatException           数字格式化异常
 * 				|----InputMismatchException          输入不匹配异常
 * 				|----ArithmaticException             出现异常的运算条件
        |-----Runtime Exception 运行时异常
        用户自定义异常
     面试题:常见的异常有哪些？举例说明
         
 * 1. "throws + 异常类型"写在方法的声明处。指明此方法执行时，可能会抛出的异常类型。
 *     一旦当方法体执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会被抛出。异常代码后续的代码，就不再执行！
 *     关于异常对象的产生:① 系统自动生成的异常对象
 * 					② 手动生成一个异常对象，并抛出(throw)
 *     
 * 2. 体会：try-catch-finally:真正的将异常给处理掉了。
 *        throws的方式只是将异常抛给了方法的调用者。  并没有真正将异常处理掉。  
```

```Java
面试题：
1. try-catch-finally 中哪个部分可以省略？
	答： catch和finally可以省略其中一个 ， catch和finally不能同时省略
	注意:格式上允许省略catch块, 但是发生异常时就不会捕获异常了,我们在开发中也不会这样去写代码.
2. try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？
	答：finally中的代码会执行
	详解：
	执行流程：
	(1) 先计算返回值， 并将返回值存储起来， 等待返回
	(2) 执行finally代码块
	(3) 将之前存储的返回值， 返回出去；
	需注意：
	(1) 返回值是在finally运算之前就确定了 ，并且缓存了，不管finally对该值做任何的改变，返回的值都不
	会改变
	(2) finally代码中不建议包含return，因为程序会在上述的流程中提前退出，也就是说返回的值不是try或
	catch中的值。如果finally包含return，那么返回的值就是finally设置返回的值
	(3) 如果在try或catch中停止了JVM,则finally不会执行.例如停电- -, 或通过如下代码退出
	JVM:System.exit(0);
```

```Java
public class Demo1 {
    public static void main(String[] args) {
        int a  = haha();
        System.out.println(a);
    }
    public static int haha(){
        int a = 10;
        try {
            return a;   //被保存起来
        }catch (Exception E){
            return 0;
        }finally {
            a = 20;
        }
    }
}     //结果为10
```

什么时候使用异常抛出和异常处理？





## 自定义异常类

```java
/*
 * 如何自定义异常类？
 * 1.继承于现有的异常结构：RuntimeException 、Exception
 * 2.提供全局常量：serialVersionUID
 * 3.提供重载的构造器
 */
public class MyException extends RuntimeException{
	static final long serialVersionUID = -7034897193246939L;
	public MyException(){
	}
	public MyException(String msg){
		super(msg);
	}
}
抛出自定义异常：throw MyException("不能输入负数")
```

## 项目统一异常处理

1.使用一个枚举类来定义异常，例如逻辑异常，数据异常

```Java
public enum ErrorEnum {
    /**
     * 错误类型枚举
     */
    SUCCESS("0", "成功"),

    DATA_ERROR("AP0001", "数据异常"),

    NO_PERMISSION("AP0002", "当前用户无权限"),
    /**
     * 错误码
     */
    private final String code;

    /**
     * 错误描述
     */
    private final String desc;

    ErrorEnum(String code, String desc) {
        this.code = code;
        this.desc = desc;
    }
}
```

2.自定义异常处理类

我们需要定制化的异常处理类，用于封装后端到前端的异常信息。该异常类至少需要包含2个字段：错误码(errorCode)、错误信息(errorMsg)。此外，该类同时需要拓展RuntimeException，表示这是个异常类，并可以通过代码主动抛出。

```Java
public class AppTransException extends RuntimeException{
    private String errorCode;
    private String msg;
    public AppTransException(ErrorEnum e, String msg){
        super(msg);
        this.msg = msg;
        this.errorCode = e.getCode();
    }
    public AppTransException(ErrorEnum e){
        super(e.getDesc());
        this.msg = e.getDesc();
        this.errorCode = e.getCode();
    }
}
```

3.在业务代码中处理异常



# 多线程

## 进程与线程的关系

```
进程：
一个在内存中运行的应用程序。每个进程都有自己独立的一块内存空间，一个进程可以有多个线程，比如在Windows系统中，一个运行的xx.exe就是一个进程。
线程：
1.线程是进程当中的一条执行流程。
2.线程是调度和执行的单位，每个线程拥有独立的运行栈和程序计数器(pc)，线程切换的开销小
3.一个进程中的多个线程除了共享进程的堆和方法区（1.8之后的元空间），还拥有自己的程序计数器，虚拟机栈，本地方法栈。

最大区别：线程是调度的基本单位，而进程则是资源拥有的基本单位

线程相比进程能减少开销，体现在：
1.线程的创建时间比进程快，因为进程在创建的过程中，还需要资源管理信息，比如内存管理信息、文件管理信息，而线程在创建的过程中，不会涉及这些资源管理信息，而是共享它们；
2.线程的终止时间比进程快，因为线程释放的资源相比进程少很多；
3.同一个进程内的线程切换比进程切换快。
```

从 JVM 角度说进程和线程之间的关系
<img src="https://img-blog.csdnimg.cn/20191105205545651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly90aGlua3dvbi5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom: 50%;" />

从上图可以看出：一个进程中可以有多个线程，多个线程共享进程的**堆**和**方法区 (JDK1.8 之后的元空间)*资源，但是每个线程有自己的程序计数器**、**虚拟机栈** 和 **本地方法栈**。

![image-20220819223120701](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819223120701.png)

![image-20220819223013299](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819223013299.png)

本地方法栈-Native方法：说明Java程序想要调用系统底层非Java写的程序。

## 线程的创建方式-4种

- JDK1.5之前创建新执行线程有两种方法：

  - 继承`Thread`类的方式
  - 实现`Runnable`接口的方式

  JDK1.5新增：实现Callable接口

  第四种：使用线程池

```Java
多线程的创建，方式一：继承于Thread类
 * 1.创建一个继承于Thread类的子类    class son1 extends Thread
 * 2.重写Thread的run()方法 ---> 将此线程的方法声明在run()中
 * 3.创建Thread类的子对象
 * 4.通过此对象调用start() 如:MyThread.start();
优点：编写简单  缺点：已经继承了Thread，所以无法继承其他父类

 创建多线程的方式二：实现Runnable接口
 * 1.创建一个实现了Runnable接口类    class son2 implements Runnable 
 * 2.实现类去实现Runnable中的抽象方法:run()
 * 3.创建实现类的对象                                            MyThread m1 = new MyThread()
 * 4.将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象   Thread thread = new Thread(m1)
 * 5.通过Thread类的对象调用start()                               thread.start()
 优点：还能继承其他类。
     
Java 8 以后可以使用 lambda 精简代码
Runnable task2 = () -> log.debug("hello");  
    
 Thread 与 Runnable 的关系 
 Runnable是一个接口，Thread是Runnable的子类  
     
 创建多线程的方式三：实现Callable接口                     一个继承Thread 两个实现接口
 *1.创建一个实现Callable接口的实现类
 *2.实现类去实现call()方法
 *3.创建实现类的对象  
 *4.将此对象作为参数传递到FutureTask类的构造器中，创建FutureTask的对象    new FutureTask(thread);
 *5.将FutureTask的对象作为参数传递到Thread类的构造器中，并调用start()    new Thread(futureTask).start();
 好处：1.call()有返回值，可以抛出异常，被外面捕获；2Callable是支持泛型的；3借助FutureTask类，比如获取返回结果
ExecutorService executor = Executors.newFixedThreadPool(4); 
// 定义任务:
Callable<String> task = new Task();
// 提交任务并获得Future:
Future<String> future = executor.submit(task);
// 从Future获取异步执行返回的结果:
String result = future.get(); // 可能阻塞
  
     
 创建多线程的方式四：使用线程池
 好处： 1.提高响应速度（减少了创建新线程的时间）
 *      2.降低资源消耗（重复利用线程池中线程，不需要每次都创建）
 *      3.便于线程管理
     引出线程池的核心参数，工作方式
 *          ● corePoolSize 核心线程数目 (最多保留的线程数) 
            ● maximumPoolSize 最大线程数目 
            ● keepAliveTime 生存时间 - 针对救急线程 
            ● unit 时间单位 - 针对救急线程 
            ● workQueue 阻塞队列 
            ● threadFactory 线程工厂 - 可以为线程创建时起个好名字 
            ● handler 拒绝策略
     
创建线程池 
通过 ThreadPoolExecutor 创建的线程池；Java自带的
通过 Executors 创建的线程池。
     
```

## 线程的优先级

```
线程的优先级等级
 *   - MAX_PRIORITY：10
 *   - MIN _PRIORITY：1
 *   - NORM_PRIORITY：5 --->默认优先级
 * - 涉及的方法
 *   - getPriority() ：返回线程优先值
 *   - setPriority(intnewPriority) ：改变线程的优先级
 *
 *   说明:高优先级的线程要抢占低优先级线程cpu的执行权。
 *       但是只是从概率上讲，高优先级的线程高概率的情况下被执行。
 *       并不意味着只有当高优先级的线程执行完以后，低优先级的线程才会被执行。
```

## 线程的生命周期

新建：创建一个新的线程对象
就绪：调用线程的start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源
运行：当就绪的线程被调度并获得CPU资源时,便进入运行状态，run()方法定义了线程的操作和功能
阻塞：在某种特殊情况下，被人为挂起或执行输入输出操作时，让出CPU并临时中止自己的执行，进入阻塞状态
死亡：线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束

![img](https://img-blog.csdnimg.cn/img_convert/b29133ad93d3839d259de57bbfa1397a.png)

**join()方法的作用**

`join()`是 Thread 类中的一个方法，当我们需要让线程按照自己指定的顺序执行的时候，就可以利用这个方法。**「`Thread.join()`方法表示调用此方法的线程被阻塞，仅当该方法完成以后，才能继续运行」**。从源码来看，实际上`join`方法就是调用了`wait`方法来使得线程阻塞



![image-20230225232921676](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230225232921676.png)

## *synchronized*

```java
方式一：同步代码块
 *  synchronized(同步监视器){
 *      //需要被同步的代码
 *  }
 *  说明：1.操作共享数据的代码，即为需要被同步的代码 --->不能包含代码多了，也不能包含代码少了。
 *       2.共享数据：多个线程共同操作的变量。比如：ticket就是共享数据
 *       3.同步监视器，俗称：锁。任何一个类的对象，都可以来充当锁。
 *          要求：多个线程必须要共用同一把锁。可以使用Object.monitor充当 监视器 
 *       补充：在实现Runnable接口创建多线程的方式中，我们可以考虑使用this充当同步监视器。
 *		
方式二：同步方法  分为修饰普通方法 和 修饰静态方法
 *    如果操作共享数据的代码完整的声明在一个方法中，我们不妨将此方法声明同步的
      如：
     public synchronized void show()：
     public static synchronized void show()：  
     
synchronized修饰静态方法和普通方法有所不同，用synchronized修饰的静态方法，同时只能够被一个执行线程访问，但是其他线程可以访问这个对象的非静态方法（该非静态方法被synchronized修饰也可以访问）。两个线程是可以同时访问一个对象的两个synchronized方法的，一个静态方法一个非静态方法。

synchronized 修饰普通方法时，锁对象是this对象。因此如果两个不同对象访问这个同步方法，不是同一把锁。
  
synchronized 修饰静态方法时，锁对象是字节码文件对象。大家使用都是同一把锁，不同对象访问不会线程安全问题。

```

## Synchronized与Lock的区别

* 1.死锁的理解：不同的线程分别占用对方需要的同步资源不放弃，并且环路等待。四个条件
 *       都在等待对方放弃自己需要的同步资源，就形成了线程的死锁
 * 2.说明:
 *      》出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续，
 *      面试：那怎么检测死锁呢？

```java
总结Synchronized与Lock的区别
存在层次上
synchronized: Java的关键字，在jvm层面上
Lock: 是一个接口 ---ReentrantLock类实现了Lock接口

锁的释放（死锁产生）
synchronized: 在发生异常时候会自动释放占有的锁，因此不会出现死锁
Lock: 发生异常时候，不会主动释放占有的锁，必须手动unlock来释放锁，可能引起死锁的发生 要写try finally

锁的类型
synchronized: 可重入 不可中断 非公平  悲观锁
Lock: 可重入 可中断 可公平（两者皆可），可以设置超时时间 ，支持多个条件变量 悲观锁

性能
在没有竞争时，synchronized做了很多优化，如偏向锁，轻量级锁，性能好
在竞争激烈时，lock会提供更好的性能
    
条件变量
synchronized：只有一个waitSet 线程阻塞等待存放的地方·，而且唤醒是notify（随机唤醒），notifyAll，颗粒度太大
ReentrantLock ：支持多个条件变量，精确唤醒线程。
    
底层实现
synchronized: 底层使用指令码方式来控制锁的，映射成字节码指令就是：（monitorenter和monitorexit 指令）。
   当线程执行遇到monitorenter指令时会尝试获取内置锁，如果获取锁则锁计数器+1，如果没有获取锁则阻塞；
   当遇到monitorexit指令时锁计数器-1，如果计数器为0则释放锁。
   由于synchronized一开始为重量级锁，在jdk1.6做了优化，无锁-》偏向锁-》轻量级锁-》重量级锁 
    
Lock: 底层是CAS乐观锁，依赖AbstractQueuedSynchronizer(AQS)类，把所有的请求线程构成一个CLH队列。而对该队列的操作均通过Lock-Free（CAS）操作。
```

## 4种解决线程安全问题的方式

```Java
1.synchronized  
2.使用Lock接口下的实现类ReentrantLock(常用)。
3.使用线程本地存储ThreadLocal。当多个线程操作同一个变量且互不干扰的场景下，可以使用ThreadLocal来解决。它会在每个线程中对该变量创建一个副本。通过set(T value)方法给线程的局部变量设置值；get()获取线程局部变量中的值。底层其实是通过ThreadLocalMap来实现的。
4.使用乐观锁机制。使用一个version字段记录查询的版本号。每次查询时，查出带有version的数据记录，更新数据时，判断数据库里对应id的记录的version是否和查出的version相同。若相同，则更新数据并把版本号+1；若不同，则说明，该数据发生了并发，被别的线程使用了。
```

## 线程间的通信方式

```Java
线程通信主要可以分为三种方式，分别为共享内存、消息传递。
1.共享内存：线程之间共享程序的公共状态，线程之间通过读-写内存中的公共状态来隐式通信 如：volatile
    
2.消息传递：线程之间没有公共的状态，线程之间必须通过明确的发送信息来显示的进行通信 如：wait/notify/join

volatile有一个关键的特性：保证内存可见性，即多个线程访问内存中的同一个被volatile关键字修饰的变量时，当某一个线程修改完该变量后，需要先将这个最新修改的值写回到主内存，从而保证下一个读取该变量的线程取得的就是主内存中该数据的最新值。定义如：private static volatile boolean flag=true;

wait():一旦执行此方法，当前线程就进入阻塞状态，并自动释放锁。
notify():一旦执行此方法，就会唤醒被wait的一个线程。如果有多个线程被wait，就唤醒优先级高的那个。
notifyAll():一旦执行此方法，就会唤醒所有被wait的线程。
    说明：
 *      1.wait()，notify()，notifyAll()三个方法必须使用在同步代码块或同步方法中。
 *      2.wait()，notify()，notifyAll()三个方法的调用者必须是同步代码块或同步方法中的同步监视器。
	如：  synchronized (obj) { 
				obj.wait(); }	
```

## sleep()和wait()的异同

**wait的原理**
![image-20220908201134697](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220908201134697.png)

![image-20220908201410679](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220908201410679.png)

```java
 /**
 * 面试题：sleep() 和 wait()的异同？
 * 1.相同点：一旦执行方法，都可以使得当前的线程进入阻塞状态。
 * 2.不同点：1）两个方法声明的位置不同：Thread类中使用sleep() , Object类中使用wait()
 *          2）调用的要求不同：sleep()可以在任何需要的场景下调用。 wait()需要和synchronized一起使用
 *          3）关于是否释放同步监视器：如果两个方法都使用在同步代码块或同步方法中，sleep()不会释放锁，wait()会释放锁。
 */
```

```Java
final static Object obj = new Object();

public static void main(String[] args) {
    new Thread(() -> {
        synchronized (obj) {
            log.debug("执行....");
            try {
                obj.wait(); // 让线程在obj上一直等待下去
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            log.debug("其它代码....");
        }
    }).start();
```

## 线程池设置

CPU/IO密集型场景如何设置线程池？





## volatile

```java
它可以用来修饰变量，他可以避免线程从自己的工作缓存中查找变量的值，必须到主存中获取它的值，线程操作 volatile 变量都是直接操作主存。从而达到一个线程对volatile 变量的修改，对另一个线程可见。
volatile ，实现了可见性，有序性。原理：读写屏障
如何保证可见性？
● 对 volatile 变量的写指令后会加入写屏障：写屏障保证在该屏障之前的，对共享变量的改动，都同步到主存当中
● 对 volatile 变量的读指令前会加入读屏障：读屏障保证在该屏障之后，对共享变量的读取，加载的是主存中最新数据
有序性 --禁用指令重排
● 写屏障会确保指令重排序时，不会将写屏障之前的代码排在写屏障之后 
● 读屏障会确保指令重排序时，不会将读屏障之后的代码排在读屏障之前
    
    
    修改变量的操作
    写屏障
    -------------
    读屏障
    读变量的操作
    
volatile不能保证线程安全。因为没有保证原子性
```

![image-20230227113523090](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230227113523090.png)

## 原子类

```Java
原子类是具有原子性的类，原子性的意思是对于一组操作，保证并发情况下的线程安全。相对于锁的优势：1粒度更细，竞争范围缩小到变量级别。2 效率更高，因为原子类底层利用了CAS，在低并发情况下。
    
原子整数：
AtomicBoolean  AtomicInteger   AtomicLong 

原子引用  AtomicXXXReference    
● AtomicReference  --AtomicReference<BigDecimal> tmp  //可以让一个对象保持原子性，而不局限一个变量。
● AtomicMarkableReference(仅维护是否修改过)   
● AtomicStampedReference (维护版本号)

原子数组
● AtomicIntegerArray 
● AtomicLongArray 
● AtomicReferenceArray 

字段原子更新器  AtomicXXXFieldUpdater
//利用字段更新器，可以针对对象的某个域（Field）进行原子操作，只能配合 volatile 修饰的字段使用
//字段更新器主要用于对已经声明的非原子变量，为它增加原子性，让该变量拥有CAS操作的能力。
● AtomicReferenceFieldUpdater // 域字段 
● AtomicIntegerFieldUpdater 
● AtomicLongFieldUpdater

原子累加器
● DoubleAccumulator
● DoubleAdder
● LongAccumulator
● LongAdder：性能提升原理：在有竞争时，设置多个累加单元，Therad-0 累加 Cell[0]，而 Thread-1 累加Cell[1]... 最后将结果汇总。这样累加时操作的不同Cell变量，减少了CAS重试失败，从而提高性能。

LongAdder原理：AtomicLong是多个线程对同一个value值进行操作，导致多个线程自旋次数太多，性能降低。而LongAdder在无竞争的情况，跟AtomicLong一样，对同一个base进行操作，当出现竞争关系时则是采用分段累加的做法，从空间换时间，用一个数组cells，将一个value拆分进这个数组cells。多个线程需要同时对value进行操作时候，可以对线程id进行hash得到hash值，再根据hash值映射到这个数组cells的某个下标，再对该下标所对应的值进行自增操作。当所有线程操作完毕，将数组cells的所有值和无竞争值base都加起来作为最终结果。

// 累加单元数组, 懒惰初始化
transient volatile Cell[] cells;
// 基础值, 如果没有竞争, 则用 cas 累加这个域
transient volatile long base;
// 在 cells 创建或扩容时, 置为 1, 表示加锁
transient volatile int cellsBusy
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAyMC8zLzE1LzE3MGRlMDQwZjUwZTA2NGI?x-oss-process=image/format,png)



什么原子类
原子类是具有原子性的类，原子性的意思是对于一组操作，要么全部执行成功，要么全部执行失败，不能只有其中某几个执行成功。

原子类作用
作用和锁有类似之处，是为了保证并发情况下的线程安全。

相对于锁的优势
粒度更细
原子变量可以把竞争范围缩小到变量级别,通常情况下锁的粒度也大于原子变量的粒度。
效率更高
除了在高并发之外，使用原子类的效率往往比使用同步互斥锁的效率更高，因为原子类底层利用了CAS，不会阻塞线程。

**Array 数组类型原子类**
AtomicArray 数组类型原子类，数组里的元素，都可以保证其原子性，比如 AtomicIntegerArray 相当于把 AtomicInteger 聚合起来，组合成一个数组。我们如果想用一个每一个元素都具备原子性的数组的话， 就可以使用 AtomicArray

**Atomic*Reference 引用类型原子类**

AtomicReference引用类型原子类，作用和AtomicInteger没有本质区别，AtomicReference是让一个对象保持原子性，而不局限一个变量。
![image-20221016200306496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221016200306496.png)



# 常用类

## String

String为什么是不可变的？

1.保存字符串的数组被final 修饰且为私有的，并且没有暴露修改这个字符串的方法。
2.String类被final 修饰 导致其不能被继承，进而避免了子类破坏String。

### String s1 与 new String()区别

```
Java把内存划分成两种：一种是栈内存，一种是堆内存。栈内存存放基本类型变量和对象的引用变量，堆内存用来存放由new创建的对象和数组。   
1.用new()来新建对象的，它会在存放于堆中。每调用一次就会创建一个新的对象
2.第一种先在栈中创建一个对String类的对象引用变量str，然后查找字符串常量池中有没有存放"abc"，如果没有，则将"abc"存放进，并令str指向”abc”，如果已经有”abc” 则直接令str指向“abc”。
String str1 = new String("abc") ;
String str2 = new String("abc") ;
上面一共创建了几个对象？

答案：答案:3个 ,编译期字符串常量池中创建1个,运行期在堆中创建2个.（用new创建的每new一次就在堆上创建一个对象，用引号创建的如果在常量池中已有就直接指向，不用创建）
```

<img src="https://img-blog.csdnimg.cn/20200509201753900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2MTUzOTQ5,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom: 50%;" />



**String s1 = new String("abc");这句话创建了几个字符串对象？**

会创建 1 或 2 个字符串对象。

1、如果字符串常量池中不存在字符串对象“abc”的引用，那么它将首先在字符串常量池中创建，然后在堆空间中创建，因此将创建总共 2 个字符串对象。

2.如果字符串常量池已存在 字符串对象“abc”，那么只会在堆中创建一个字符串对象“abc”。





### String与基本数据类型包装类的转换

```Java
String --> 基本数据类型、包装类：调用包装类的静态方法：parseXxx(str)   如：Integer.parseInt(str1);
基本数据类型、包装类 --> String:调用String重载的valueOf(xxx)  如String.valueOf(num)
    
String --> char[]:调用String的toCharArray()
char[] --> String:调用String的构造器   如： new String(arr)
```

### StringBuffer和StringBuilder的区别

<img src="https://img-blog.csdnimg.cn/2020022622124239.png" alt="在这里插入图片描述" style="zoom: 80%;" />

String 属于常量类型，被声明为 final class，所有的属性也都是 final 类型，因此 String 对象一旦创建，便不可更改； StringBuilder / StringBuffer  两个类属于变量类型，是可以更改的，它们都是为了解决字符串由于拼接产生太多中间对象的问题而提供的类。





## 如何自定义注解

```java
public @interface MyAnnotation {
    String value();
}
/**
 * 注解的使用
 *  3.如何自定义注解：参照@SuppressWarnings定义
 *      ① 注解声明为：@interface
 *      ② 内部定义成员，通常使用value表示
 *      ③ 可以指定成员的默认值，使用default定义
 *      ④ 如果自定义注解没有成员，表明是一个标识作用。
 *
 *      如果注解有成员，在使用注解时，需要指明成员的值。
 *      自定义注解必须配上注解的信息处理流程(使用反射)才有意义。
 *      自定义注解通过都会指明两个元注解：Retention、Target
 *     @Retention: 只能用于修饰一个Annotation定义, 用于指定该Annotation 的生命周期,
 	   @Target: 用于修饰Annotation 定义, 用于指定被修饰的Annotation 能用于修饰哪些程序元素
 */
@MyAnnotation(value = "hello")
```

![img](https://img-blog.csdnimg.cn/img_convert/55f870273e1957635488dbcab66a299d.png)

# 集合

Java集合有哪些？

Java集合分三种，List、Set、Map，这三种集合适用于不同的场景

- List：适用于有序，可重复的集合
- Set：适用于不可重复集合
- Map：适用于键值对的存储

![Java 集合框架概览](https://oss.javaguide.cn/github/javaguide/java/collection/java-collection-hierarchy.png)

<img src="https://img-blog.csdnimg.cn/img_convert/a1fdb6d75598e87a224697d12c2c2bcf.png" alt="img" style="zoom:60%;" />



Map、list、set、queue这四个有什么区别？
list:存储的元素是有序的、可重复的，ArrayList、LinkedList、Vector
Set:无序、不可重复 HashSet,LinkedHashSet
queue：队列，按照先进先出的顺序排列，存储的元素有序，可重复。
`Map`是存储键值对这样的双列数据的集合, HashMap, HashTable, LinkedHashMap, Map接口是不继承于Colletion接口的

## ArrayList

### List接口常用实现类的对比

```java
/**
 *    |----Collection接口：单列集合，用来存储一个一个的对象
 *       |----List接口：存储有序的、可重复的数据。  -->“动态”数组,替换原有的数组
 *          |----ArrayList：作为List接口的主要实现类；线程不安全的，效率高；底层使用动态数组存储
 *          |----LinkedList：对于频繁的插入、删除操作，使用此类效率比ArrayList高；底层使用双向链表存储
 *          |----Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] elementData存储
 *
 * 面试题：比较ArrayList、LinkedList、Vector三者的异同？
 *        同：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据
 *        不同：见上
```

### ArrayList扩容机制分析
[浅谈 ArrayList 及其扩容机制 - 城北有个混子 - 博客园 (cnblogs.com)](https://www.cnblogs.com/ruoli-0/p/13714389.html)

```
第一种情况，初始化情况不同：
当ArrayList使用无参构造创建，创建的Array List的容量为0，添加第一个元素后，容量变成10，此后扩容为正常扩容。
当ArrayList传容量扩容，那么就创建相同容量，后续进行正常扩容。如果传的容量是0，创建的容量为0，添加第一个元素后，容量为1，此时是满的，下次添加元素为正常扩容(1.5倍)。
```

![image-20230404215137723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230404215137723.png)

Arraylist 为什么会出现并发问题？

```
1.扩容的时候，如果在并发操作ArrayList的时候，可能会有数组索引越界的异常产生。
原因：线程A和线程B获取的size都是9，线程A先插入元素e，这个时候elementData数组的大小为10，是正常情况下下次应该是要扩容的，但是线程B获取的size=9而不是10，在线程B中没有进行扩容，而是报出数组index越界异常。
```



## Set

### Set接口实现类的对比

```
/**
 * 1.Set接口的框架：
 * |----Collection接口：单列集合，用来存储一个一个的对象
 *          |----Set接口：存储无序的、不可重复的数据  
 *             |----HashSet：作为Set接口的主要实现类；线程不安全的；可以存储null值,存储没有顺序
 *                 |----LinkedHashSet：作为HashSet的子类；遍历其内部数据时，可以按照添加的顺序遍历
 *                                    对于频繁的遍历操作，LinkedHashSet效率高于HashSet.
 *             |----TreeSet：可以按照添加对象的指定属性，进行排序。
```

### HashSet中元素的添加过程

```
/**
     * 一、Set:存储无序的、不可重复的数据
     *     1.无序性：不等于随机性。存储的数据在底层数组中并非按照数组索引的顺序添加，而是根据数据的哈希值决定的。
     *     2.不可重复性：保证添加的元素按照equals()判断时，不能返回true.即：相同的元素只能添加一个。
     *
     * 二、添加元素的过程：以HashSet为例：
     *      我们向HashSet中添加元素a,首先调用元素a所在类的hashCode()方法，计算元素a的哈希值，
     *      此哈希值接着通过某种算法计算出在HashSet底层数组中的存放位置（即为：索引位置），判断
     *      数组此位置上是否已经有元素：
     *          如果此位置上没有其他元素，则元素a添加成功。 --->情况1
     *          如果此位置上有其他元素b(或以链表形式存在的多个元素），则比较元素a与元素b的hash值：
     *              如果hash值不相同，则元素a添加成功。--->情况2
     *              如果hash值相同，进而需要调用元素a所在类的equals()方法：
     *                    equals()返回true,元素a添加失败
     *                    equals()返回false,则元素a添加成功。--->情况3
     *
     *      对于添加成功的情况2和情况3而言：元素a 与已经存在指定索引位置上数据以链表的方式存储。
     *      jdk 7 :元素a放到数组中，指向原来的元素。
     *      jdk 8 :原来的元素在数组中，指向元素a
     *      总结：七上八下
     *
     * HashSet底层：数组+链表的结构。
     */
```

<img src="https://img-blog.csdnimg.cn/img_convert/9cadfe3cd3721e9c7ba5f414f57434ec.png" alt="img" style="zoom:80%;" />

### LinkedHashSet

```
LinkedHashSet是HashSet的子类
LinkedHashSet根据元素的hashCode值来决定元素的存储位置，但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入顺序保存的。
LinkedHashSet插入性能略低于HashSet，但在迭代访问Set 里的全部元素时有很好的性能。
LinkedHashSet不允许集合元素重复。
```





## Map接口及其多个实现类的对比

```Java
/**
 * 一、Map的实现类的结构：
 *  |----Map:双列数据，存储key-value对的数据   ---类似于高中的函数：y = f(x)
 *         |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value
 *              |----LinkedHashMap:保证在遍历map元素时，可以按照添加的顺序实现遍历。
 *                      原因：在原有的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
 *                      对于频繁的遍历操作，此类执行效率高于HashMap。
 *         |----TreeMap:保证按照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序
 *                      底层使用红黑树
 *         |----Hashtable:线程安全，效率低-原因每个方法加上synchronized；不能存储null的key和value
 *              |----Properties:常用来处理配置文件。key和value都是String类型
 * 
 *      HashMap的底层：数组+链表  （jdk7及之前）
 *                    数组+链表+红黑树 （jdk 8）
 *  面试题：
 *  1. HashMap的底层实现原理？
 *  2. HashMap 和 Hashtable的异同？
 */
3. CurrentHashMap 与 Hashtable 的异同？
 （1）CurrentHashMap 锁的颗粒度比 Hashtable 小得多。
 （2）HashMap同时支持null键值对，而 CurrentHashMap 中却都不支持。
 原因：减少CurrentHashMap在并发场景下出现歧义。例如，通过map.get(key)获取值，但是返回的结果为null，在并发场景下，无法判断是键是不存在，还是对应的值为null。在非并发场景下，可以通过containsKey(key)来判断。但是在并发场景下，两次调用之间数据是可能发生变化的。
```

### 1.HashMap的底层实现原理

![img](https://img-blog.csdnimg.cn/img_convert/58c89db281ee84d93098a4f043dd3856.png)

![img](https://img-blog.csdnimg.cn/img_convert/b2aebd46ed8d41111825cd15ac2a3be6.png)

```
/*
 * 三、HashMap的底层实现原理？以jdk7为例说明：
 *    HashMap map = new HashMap():
 *    在实例化以后，底层创建了长度是16的一维数组Entry[] table。
 *    map.put(key1,value1):
 *    首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过indexFor算法(length-1取模)以后，得到在Entry数组中的存放位置。
 *    如果此位置上的数据为空，此时的key1-value1添加成功。 ----情况1
 *    如果此位置上的数据不为空，(意味着此位置上存在一个或多个数据(以链表形式存在)),比较key1和已经存在的一个或多个数据的哈希值：
 *           如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功。----情况2
 *           如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals(key2)方法，比较：（注意此时比较的不是hash值，而是key）
 *                如果equals()返回false:此时key1-value1添加成功。----情况3
 *                如果equals()返回true:使用value1替换value2。
 *
 *   补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储(拉链法)解决hash冲突。
 *
 *   在不断的添加过程中，会涉及到扩容问题，当超出临界值(且要存放的位置非空)时，扩容。默认的扩容方式：扩容为原来容量的2倍，并将原有的数据复制过来（需要重新计算新位置）。扩容的临界值，=容量*填充因子：16 * 0.75 => 12
 
  HashMap--jdk8 相较于jdk7在底层实现方面的不同：
 *      1.new HashMap():初始化时底层没有创建一个长度为16的数组，采用懒加载方式
 *      2.jdk 8底层的数组是：Node[]数组,而非Entry[]
 *      3.首次调用put()方法时，底层创建长度为16的数组
 *      4.jdk7底层结构只有：数组+链表。jdk8中底层结构：数组+链表+红黑树。
 *         4.1形成链表时，七上八下（jdk7:新的元素指向旧的元素-头插法。jdk8：旧的元素指向新的元素-尾插法）
 *         4.2当数组的某一个索引位置上的元素以冲突链表形式存在的数据个数 > 8 且当前数组的长度 > 64时，此时此索引位置上的所数据改为使用红黑树存储。
 		  4.3树退化成链表情况1，树的元素<=6 则会退化成链表
 		     树退化成链表情况2，remove树节点时，如果root root.left, root.right, root.left.left有null
```

面试题：
**1.为什么要转换成红黑树呢？**
红黑树也就是自带平衡的二叉搜索树，查找算法为二分查找，复杂度O(log n)。而链表为O(n)
hashMap一开始没有引入红黑树，是由于时间和空间的折中考虑。红黑树put操作可能需要旋转算法，并且维护指针。

为啥是扩容两倍？





### **2.红黑树原理**

红黑树的特点：

1.节点分为红黑色，根节点为黑，
2.如果一个节点是红色，则它的子节点必须为黑色。（等同于 一条路径上不会出现连续的两个红色节点）
3.对于每个节点，从该节点到其后代叶节点的简单路径上，均包含相同数目的黑节点。这个特性使得红黑树是自带平衡二叉树。

![image-20230407154235866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230407154235866.png)

**红黑树和二叉树的区别**

**区别：** 

1、红黑树放弃了追求完全平衡，追求大致平衡，在与[平衡二叉树](https://so.csdn.net/so/search?q=平衡二叉树&spm=1001.2101.3001.7020)的时间复杂度相差不大的情况下，保证每次插入最多只需要三次旋转就能达到平衡，实现起来也更为简单。

2、平衡二叉树追求绝对平衡，条件比较苛刻，实现起来比较麻烦，每次插入新节点之后需要旋转的次数不能预知。



3.HashMap为什么不是线程安全的？

- **JDK1.7 中，由于多个线程对HashMap进行扩容，调用了HashMap#transfer()，具体原因：某个线程执行扩容过程中，由于时间片被耗尽，导致线程被挂起，然后切换其他线程完成数据迁移，等CPU资源释放后被挂起的线程重新执行之前的扩容逻辑，数据已经被改变，造成死循环、数据丢失。**
- **JDK1.8 中，由于多线程对HashMap进行put操作，调用了HashMap#putVal()，具体原因：假设两个线程A、B都在进行put操作，并且hash函数计算出的插入下标是相同的，当线程A执行完第六行代码后由于时间片耗尽导致被挂起，而线程B得到时间片后在该下标处插入了元素，完成了正常的插入，然后线程A获得时间片，由于之前已经进行了hash碰撞的判断，所有此时不会再进行判断，而是直接进行插入，这就导致了线程B插入的数据被线程A覆盖了，从而线程不安全。**

- JDK1.7 HashMap线程不安全体现在：扩容引发的线程不安全，导致死循环、数据丢失
- JDK1.8 HashMap线程不安全体现在：多线程进行put操作时导致数据覆盖

JDK1.7 中，HashMap的扩容操作，重新定位每个桶的下标，并采用头插法将元素迁移到新数组中。头插法会将链表的顺序翻转，这也是形成死循环的关键点。

如何使HashMap在多线程情况下进行线程安全操作？

使用 Collections.synchronizedMap(map)，包装成同步Map，原理就是在HashMap的所有方法上synchronized。



### ConcurrentHashMap分析

ConcurrentHashMap 1.7 的存储结构

<img src="https://mmbiz.qpic.cn/mmbiz_png/4lfok2icUkibSCkvgIicM9SrfNSjOdW2ib4dVxyH81KAwY3Hrl0mOgyZ1tjjQNmWgMTA0n9F2c8tcE6m7o1ibGmruTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:80%;" />

```Java
ConcurrentHashMap 1.7 ：
底层使用数组+链表实现，默认将数组分成16段Segment,Segment 继承了 ReentrantLock，所以 Segment 是一种可重入锁，扮演锁的角色。默认支持最多 16 个线程并发。每个segment 下包含hashentry数组和冲突链表。
put流程：
0.计算 put 的数据要放入的 segment 位置
1.tryLock() 获取锁，获取不到使用 scanAndLockForPut 方法继续获取。
2.计算 put 的数据要放入的在当前segment下的hashentry数组的位置，然后获取这个位置上的 HashEntry 链表的头节点，并且遍历链表。
3.如果发现key已经存在，则覆盖value并退出，如果不存在，则初始化Node节点，准备采用头插法插入链表。
4.插入链表之前，需要判断HashEntry数组是否需要扩容（超过长度3/4才扩容,扩容是翻倍），并将node放入hashEntry数组中的指定位置，释放锁。
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220821233701548.png" alt="image-20220821233701548"  />

ConcurrentHashMap 1.8底层分析
![图片](https://mmbiz.qpic.cn/mmbiz_png/4lfok2icUkibSCkvgIicM9SrfNSjOdW2ib4d3aFUSU08gLTTaFAUMI0ZibmJj1ib9ejuGZMdOkCFfVs5oSqZv0LTvIbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

```
底层实现：1.数组+链表+红黑树，冲突链表长度>8 && 数组长度>64，冲突链表变成红黑树，优点：遍历速度快（二叉平衡树）
		2.使用Synchronized 锁 + CAS 的机制保证线程安全
		3.初始化7是饿汉式，8是懒汉式，数组容量达到3/4就会扩容，翻倍扩容
		将锁的级别控制在了更细粒度的hashentry数组元素级别，相比于16个segment。
		
put流程：
1.根据key计算出hash值；   
2.判断数组是否为空，如果为空使用CAS初始化
3.根据存储的元素（key）计算位置,判断该位置是否为空，为空则利用CAS设置该节点，失败则自旋等待。不为空说明可能存在竞争，则使用synchronized加锁，遍历冲突链表中的数据，替换或者新增节点到冲突链表中。这里可以看出，只有在操作同一个数组位置的时候才会加锁，提升了并发性。
4.判断是否需要扩容和转换红黑树。
5.扩容的过程：翻倍扩容，并从后向前遍历，迁移完的位置上会打上标签（ForwardingNode），并重新计算节点新索引值。

在扩容过程中，出现并发的get  put 操作？
get操作：如果要查询的数据还在旧数组上，就直接查旧数组。如果计算的索引下标的数组位置被打上ForwardingNode，说明数据迁移到了新数组。
冲突链表迁移的过程中，是重新创建里面的元素，这是因为可能会出现元素的next发生变化

put操作：如果要put的数据的位置是扩容的未完成区间，不加锁。
		如果是刚好处在迁移冲突链表的位置，会阻塞等待。
		如果数据的位置是已经完成了扩容的区间，阻塞等待，要等待数组全部扩容完才能到新数组put
```

![image-20230405222219166](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405222219166.png)

## Synchronized的原理



## CAS、偏向锁，轻锁、重量级锁

CAS:
**CompareAndSwap**的缩写，中文意思是：比较并替换。CAS需要有3个操作数：内存地址V，旧的预期值A，即将要更新的目标值B。基于乐观锁的思想
CAS指令执行时，当且仅当内存地址V的值与预期值A相等时，将内存地址V的值修改为B，否则就什么都不做。整个比较并替换的操作是一个原子操作。CAS必须需要搭配volatile(读取变量的最新值)
缺点：
1.循环时间长开销很大：CAS长时间不成功，cpu在空转。
2.只能保证一个共享变量的原子操作：当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁来保证原子性。
3.ABA问题：CAS只检查值有没有变化，没法检查值是否是从A-B-A得变化，可以引入版本号解决。

无锁：
1.资源不会出现在多线程环境，或者说即使出现在多线程环境也不存在资源竞争的情况。
2.资源被竞争，但是不使用操作系统同步原语对共享资源锁定，采用其他机制来控制同步如CAS

偏向锁（比轻量级锁更轻）
轻量级锁在没有竞争时（就自己这个线程），每次重入仍然需要执行 CAS 操作。 
Java 6 中引入了偏向锁来做进一步优化：只有第一次使用 CAS 将线程 ID 设置到对象的 Mark Word 头，之后发现这个线程 ID 是自己的就表示没有竞争，不用重新 CAS。以后只要不发生竞争，这个对象就归该线程所有。

轻量级锁
如果一个对象虽然有多线程要加锁，但加锁的时间是错开的（也就是没有竞争），那么可以使用轻量级锁来优化。语法仍然是 `synchronized`，优先加轻量级锁。优点：竞争的线程不会阻塞，使用自旋。

当多个线程正在竞争锁，偏向锁->轻量级锁。获得锁的线程就可以执行任务，没有获得锁线程自旋(轮询)。如果自旋个数超过CPU核数的一半  或者自旋次数超过10次才会升级成重量级锁。

重量级锁
需要通过Monitor来对线程进行控制，此时将会使用同步原语来锁定资源，对线程控制最为严格。线程竞争不自旋而是阻塞

![image.png](https://cdn.nlark.com/yuque/0/2022/png/393192/1649161233466-6885edf3-0411-4505-8f06-c40d3dbf513b.png)

轻量级加锁流程

```Java
1、创建锁记录对象，每个线程的栈帧都包含一个锁记录的结构，内部可以存储锁对象的mark word信息
2、让锁记录对象中Object Reference指向锁对象，并且用CAS替换锁对象的markword，将markword的值存入锁记录
3、如果cas成功，对象头存储了锁记录地址和状态，表示由该线程给对象加锁
4、如果不成功，有两种情况
	1、如果其他线程已经持有该对象的轻量级锁，表明有竞争，自选并进入锁膨胀。
	2、如果是自己执行了 synchronized 锁重入，那么再添加一条 Lock Record 作为重入的计数。
5、当退出synchronized 代码块（解锁时）如果有取值为null的锁记录，表示有重入，重入计数减一。
6、● 当退出 synchronized 代码块（解锁时）锁记录的值不为 null，这时使用 cas 将 Mark Word 的值恢复给对象头 
  ○ 成功，则解锁成功 
  ○ 失败，说明轻量级锁进行了锁膨胀或已经升级为重量级锁，进入重量级锁解锁流程
  详细图解过程：https://www.yuque.com/mo_ming/gl7b70/rr1o32#G26mw
  
重量级锁解锁流程：
1、当 Thread-1 想要进行轻量级加锁时，Thread-0 已经对该对象加了轻量级锁
2、 这时 Thread-1 加轻量级锁失败，进入锁膨胀流程 
  ○ 即为 Object 对象申请 Monitor 锁，让 Object 指向重量级锁地址 
  ○ 然后自己进入 Monitor 的 EntryList BLOCKED
3、当 Thread-0 退出同步块解锁时，使用 cas 将 Mark Word 的值恢复给对象头，失败。这时会进入重量级解锁流程，即按照 Monitor 地址找到 Monitor 对象，设置 Owner 为 null，唤醒 EntryList 中 BLOCKED 线程
```

![img](https://pic4.zhimg.com/80/v2-395840866ccb36a0f139903a7d5ada07_720w.webp)

## AQS实现原理

全称是 AbstractQueuedSynchronizer，是抽象队列同步器。我们只需要继承该抽象类，并重写制定方法即可实现一套线程同步机制，如ReentrantLock 。

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及唤醒的机制，这个机制 AQS 是用 **双向 队列**实现的，即将暂时获取不到锁的线程加入到队列中。

详细来说有一个volatile state 和 一个线程等待队列。当state =1,则代表当前对象锁被占用，其他线程来加锁则失败，进入等待队列，并且会park操作挂起，等待释放锁时的线程唤醒。在`JUC`中，诸如`ReentrantLock`、`CountDownLatch`等都基于`AQS`实现。

核心原理图

![image-20230920195458934](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230920195458934.png)



![image-20221017154304807](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221017154304807.png)

![image-20230427215026323](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230427215026323.png)

## ReentrantLock 原理-加解锁

```
ReentrantLock构造器：NonfairSync 继承自 AQS 
加锁没有竞争时：
1、使用cas尝试将state由0 改为1，并把exclusiveOwnerThread 设置为当前线程

加锁有竞争时：
1、使用cas尝试将state由0 改为1，失败
2、进入 tryAcquire 逻辑，这时 state 已经是1，结果仍然失败
3、构造Node等待队列，把当前线程加入到该队列， 其中第一个 Node 称为 Dummy（哑元）或哨兵，用来占位，并不关联线程。
4、之后会再次调用tryAcquire尝试获取锁，如果都失败。就会将前驱 node，即 head 的 waitStatus 改为 -1，并且park该线程

解锁时：
Thread-0 释放锁，进入 tryRelease 流程，如果成功 
● 设置 exclusiveOwnerThread（占用资源的线程标志） 为 null 
● state = 0 （资源处于非占用）
唤醒线程流程：
head 的 waitStatus = -1，进入 unparkSuccessor 流程 
找到队列中离 head 最近的一个 Node（没取消的），unpark 恢复其运行。并将它原来所在的节点被置为头节点。

可重入原理：加锁的时候判断，如果线程还是当前线程，则发生锁重入，state++。解锁的时候state--，只有state减为0，才释放成功。

不可打断模式:在此模式下，即使它被打断，仍会驻留在 AQS 队列中，一直要等到获得锁后方能得知自己被打断了.
可打断模式：如果有打断标记，会抛出异常，退出循环。


非公平锁：不去检查AQS等待队列，直接尝试用CAS获取
公平锁： 先检查 AQS 队列中是否有前驱节点, 没有才去竞争。这两个的区别主要在 tryAcquire 方法的实现

条件变量实现原理：
每个条件变量其实就对应着一个等待队列，其实现类是 ConditionObject 
await流程：
1、开始 Thread-0 持有锁，调用 await，创建新的 Node 状态为 -2，关联 Thread-0，加入等待队列尾部。
2、调用fullyRelease 流程，释放同步器上的锁
3、unpark（恢复） AQS 队列中的下一个节点即线程，竞争锁，park 阻塞 Thread-0。

signal流程：（唤醒）
1、取得对应的等待队列中第一个 Node，即 Thread-0 所在 Node。
2、该 Node 加入 AQS 队列尾部，将 Thread-0 的 waitStatus 改为 0，Thread-3 的waitStatus 改为 -1
3、Thread-1 释放锁，进入 unlock 流程
```

## **ReentrantReadWriteLock**

```
读写锁，当读操作远远给高于写操作时，这时候使用读写锁让读-读可以并发，提高性能。
读-写 、 写-写 互斥。

● 读锁不支持条件变量,写锁支持
● 重入时不支持升级：即持有读锁的情况下去获取写锁，会导致获取写锁永久等待
● 重入时支持降级：即持有写锁的情况下去获取读锁

读写锁的加锁流程和ReentrantLock基本是一样，不同之处在于读锁的时候，Node的模式是shared模式而非 Node.EXCLUSIVE 模式。
解锁流程多加了判断是否下一个也是读锁，因为读读不影响。
```

## Semaphore 有什么用？

`synchronized` 和 `ReentrantLock` 都是一次只允许一个线程访问某个资源，而`Semaphore`(信号量)可以用来控制同时访问特定资源的线程数量，也就是可以同时存在多个线程来访问。

redissonClient.getSemaphore，如：可以使得三个客户端获得锁（共享锁），可以奖信号量理解为 许可证的数量，只要拿到了许可证的线程就能购买，而且获取信号量的方式是原子性的，解决超卖。

# 泛型

**那么为什么要有泛型呢，直接Object不是也可以存储数据吗？**

1、解决元素存储的安全性问题，好比商品、药品标签，不会弄错。2.解决获取数据元素时，需要类型强制转换的问题，好比不用每回拿商品、药品都要辨别。
2、Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生ClassCastException异常。同时，代码更加简洁、健壮。

![img](https://img-blog.csdnimg.cn/img_convert/e8b9c3de54e6610e42e08ea1a2d9f98a.png)

![img](https://img-blog.csdnimg.cn/img_convert/7bff707c9e87b7fafd21c1b57804fa7d.png)

知乎笔记：[Java泛型就这么简单 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/78811004)

## TCP与UDP区别

TCP协议：
使用TCP协议前，须先建立TCP连接，形成传输数据通道
传输前，采用“三次握手”方式，点对点通信，是可靠的
TCP协议进行通信的两个应用进程：客户端、服务端。
在连接中可进行大数据量的传输，传输完毕，需释放已建立的连接，效率低
UDP协议：
将数据、源、目的封装成数据包，不需要建立连接
每个数据报的大小限制在64K内
发送不管对方是否准备好，接收方收到也不确认，故是不可靠的
可以广播发送
发送数据结束时无需释放资源，开销小，速度快

## 序列化和反序列化

1. 序列化： 对象->字节序列  
2. 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
3. 将对象存储到文件中的时候需要进行序列化，将对象从文件中读取出来需要进行反序列化。
4. 将对象存储到缓存数据库（如 Redis）时需要用到序列化，将对象从缓存数据库中读取出来需要反序列化。
5. 序列化协议属于 TCP/IP 协议应用层的一部分。

# 反射与动态代理

## 反射

```
加载完类之后，在内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，也就是反射。
反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。
```

![img](https://img-blog.csdnimg.cn/img_convert/55a0434a0a4bf6209c9daf4ba81a3808.png)

![image-20220822204635375](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822204635375.png)

反射常用的api

1、getMethods():获取本类以及父类中所有public修饰符修饰的方法

2、getDeclaredMethods()：取得一个类的所有方法（不包括父类和实现的接口）

3、getConstructors:获取一个类的所有构造函数

4、Object instance = clazz1.newInstance(); 获取Class对象的方法

**Class类的理解**

```
关于java.lang.Class类的理解
1.类的加载过程：
程序经过javac.exe命令后，会生成一个或多个字节码文件（.class结尾）
接着使用Java.exe命令对某个字节码文件进行解释运行，相当于加载到内存中，此过程为类的加载。加载到内存的类为运行时类，也就是Class的一个实列。
Class的实例就对应着一个运行时类。
```



## 动态代理

```
代理：使用代理对象来代替对真实对象的访问，这样就可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。
静态代理：代理类和目标对象的类都是在编译期间确定下来，每一个代理类只能为一个接口服务。
动态代理：通过代理类调用对象，是在程序运行时根据需要动态创建目标类的代理对象。
```

**JDK proxy动态代理**

![image-20220822213259884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220822213259884.png)

**CGLIB 动态代理**
1.JDK 动态代理只能代理实现了接口的类或者直接代理接口，而 CGLIB 可以代理未实现任何接口的类--**原因在于代理类继承目标类，然后重写目标类的方法。**
2.效率：jdk proxy > cglib
3.CGLIB动态代理是通过生成一个被代理类的子类(继承)来拦截被代理类的方法调用，因此不能代理声明为 final 类型的类和方法
3.JDK调用代理方法，是通过反射机制调用。CGLib是通过FastClass机制，需要重写拦截方法。

为什么要用FastClass机制
当我们去调用方法一的时候，在代理类中会先判断是否实现了方法拦截的接口，没实现的话直接调用目标类的方法一；如果实现了那就会被方法拦截器拦截，在方法拦截器中会对目标类中所有的方法建立索引，其实大概就是将每个方法的引用保存在数组中，我们就可以根据数组的下标直接调用方法，而不是用反射；索引建立完成之后，方法拦截器内部就会调用invoke方法（这个方法在生成的FastClass中实现），在invoke方法内就是调用CGLIB$方法一$这种方法，也就是调用对应的目标类的方法一；

![img](https://img2018.cnblogs.com/blog/1368608/201906/1368608-20190602215701685-1483535597.png)



# 创建对象的5种方式

![image-20220905162450542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220905162450542.png)

# 浅拷贝和深拷贝

```
浅拷贝：创建一个新对象，然后将当前对象的非静态字段复制到该新对象，如果字段是值类型的，那么对该字段执行复制；如果该字段是引用类型的话，则复制引用但不复制引用的对象。因此，原始对象及其副本引用同一个对象。也就是指向同一块堆内存空间
Object 类提供的 clone 是只能实现浅拷贝的。原型模式

深拷贝：创建一个新对象，然后将当前对象的非静态字段复制到该新对象，无论该字段是值类型的还是引用类型，都复制独立的一份。当你修改其中一个对象的任何内容时，都不会影响另一个对象的内容。

如何实现深拷贝：
1.序列化--可以先使对象实现 Serializable 接口，然后把对象（实际上只是对象的一个拷贝）写到一个流里，再从流里读出来，便可以重建对象。
2.重写clone方法来实现深拷贝，对引用类型的属性进行单独处理
```

![image-20230405202321994](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405202321994.png)

浅拷贝例子：

![img](https://images2018.cnblogs.com/blog/1120165/201803/1120165-20180312214947767-1020527476.png)



# Java 8 新特性

![img](https://img-blog.csdnimg.cn/img_convert/873337fa3d817235541f830b76bfc87e.png)

1. lambda表达式

   1.举例： (o1,o2) -> Integer.compare(o1,o2);

    * 2.格式：
    *      -> :lambda操作符 或 箭头操作符
    *      ->左边：*lambda形参列表的参数类型可以省略(类型推断)；如果lambda形参列表只有一个参数，其一对()也可以省略*
    * ->右边：*lambda体应该使用一对{}包裹；如果lambda体只有一条执行语句（可能是return语句），省略这一对{}和return关键字*

   ```java
   //正常写法
   Comparator<Integer> c1 = new Comparator<Integer>() {
               @Override
               public int compare(Integer o1, Integer o2) {
                   System.out.println(o1);
                   System.out.println(o2);
                   return o1.compareTo(o2);
               }
           };
   
   Comparator<Integer> com2 = (o1,o2) -> {
               System.out.println(o1);
               System.out.println(o2);
               return o1.compareTo(o2);
           };
   
   //省略return 和{}
   Comparator<Integer> c1 = (o1,o2) -> {
               return o1.compareTo(o2);
           };
   Comparator<Integer> c2 = (o1,o2) -> o1.compareTo(o2);
   
   ```

   

2. Stream流

   常用的map方法，distinct去重，filter过滤

[这么简单，还不会使用java8 stream流的map()方法吗？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/588438562)

# JVM部分笔记

<img src="https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150547.png" alt="img" style="zoom: 80%;" />

**常量池**，**运行时常量池和字符串常量池的区别**

```
常量池
就是一张表，主要存放编译期生成的各种字面量和符号引用。
字面量:例如文本字符串、fina修饰的常量。 符号引用:例如类和接口的全限定名、字段的名称和描述符、方法的名称和描述符。

运行时常量池
当类加载到内存中后，JVM就会将class常量池中的内容存放到运行时常量池中。并把里面的符号地址变为真实地址。

字符串常量池
字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

JDK 1.7 为什么要将字符串常量池移动到堆中？
主要是因为永久代（方法区实现）的 GC 回收效率太低，只有在整堆收集 (Full GC)的时候才会被执行 GC。Java 程序中通常会有大量的被创建的字符串等待回收，将字符串常量池放到堆中，能够更高效及时地回收字符串内存。
```

## Jvm内存结构、内存模型

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230227164248337.png" alt="image-20230227164248337" style="zoom:67%;" />

```
Java内存区域包括：
1.方法区（jdk1.8之后没有方法区，移到了元空间实现）
方法区是各个线程共享的内存区域。
它用于存储已被虚拟机加载的类型信息、常量、静态变量，即时编译器编译后的代码缓存等数据。 类被加载到这里
方法区不受Java虚拟机垃圾回收器的控制。

2.堆
堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。
此内存区域的主要目的就是存放对象实例，Java世界里“几乎”所有的对象实例都在这里分配内存。
Java堆是垃圾收集器管理的内存区域

3.程序计数器
每个线程私有的内存空间，为了线程切换后能恢复到正确的执行位置

4.虚拟机栈
线程私有
a.每个方法被执行的时候，Java虚拟机都会同步创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态连接、方法出口等信息。
b.每一个方法被调用直至执行完毕的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。

5.本地方法栈
作用与虚拟机栈相似。区别是：
虚拟机栈为虚拟机执行Java方法（也就是字节码）服务。
本地方法栈则是为虚拟机使用到的本地（Native）方法服务


静态变量的存储位置
jdk1.7,静态成员存储在方法区中。
JDK1.8以后，方法区被移除，此时方法区的实现更改为元空间，但由于元空间主要用于存储字节码文件，因此静态成员的存储位置从方法区更改到了堆内存中。

常量池存储的位置
在JDK1.7中，方法区被整合到堆内存中，常量池存储在堆内存中。
JDK1.8后，方法区从堆内存中独立出来，常量池存储在方法区（元空间）中。
```

![img](https://ask.qcloudimg.com/http-save/yehe-1420980/biyx8kz2je.png?imageView2/2/w/2560/h/7000)



## Java内存模型JMM

Java虚拟机规范中定义了Java内存模型（Java Memory Model，JMM），用于屏蔽掉各种硬件和操作系统的内存访问差异，以实现让Java程序在各种平台下都能达到一致的并发效果，

Java内存模型规定了所有的变量都存储在主内存中，每条线程还有自己的工作内存，线程的工作内存中保存了该线程中是用到的变量的主内存副本拷贝，线程对变量的所有操作都必须在工作内存中进行，而不能直接读写主内存。不同的线程之间也无法直接访问对方工作内存中的变量，线程间变量的传递均需要自己的工作内存和主存之间进行数据同步进行。

而JMM就作用于工作内存和主存之间数据同步过程。他规定了如何做数据同步以及什么时候做数据同步。

![img](https://upload-images.jianshu.io/upload_images/9033085-34f1ccae5976239a.png?imageMogr2/auto-orient/strip|imageView2/2/w/397/format/webp)

JMM 体现在以下几个方面 

- 原子性 - 保证指令不会受到线程上下文切换的影响 ，操作是原子性的。synchronized
- 可见性 - 在变量修改后将新值同步回主内存，在变量读取前从主内存读取。volatile
- 有序性 - 保证指令不会受 cpu 指令并行优化的影响





## 常量池和运行时常量池

**常量池**

常量池其实就是一张记录着该类的一些常量、方法描述、类描述、变量描述信息的表。常量池中主要存放两类数据，`一是字面量、二是符号引用`。

字面量：比如String类型的字符串值或者定义为final类型的常量的值。

符号引用：类或接口的全限定名、变量或方法的名称、变量或方法的描述信息

**常量池的作用**：在解释器解释执行每条JVM指令码的时候，根据这些指令码的`符号地址`去常量池中找到对应的描述。然后解释器就知道该执行哪个类的那个方法、方法的参数是什么等。

**运行时常量池：**常量池其实就是一张对照表，当类的字节码被加载到内存中后，它的常量池信息就会集中放到一块内存，这快内存就被称为运行时常量池，并把里面的符号地址变为真实地址。



**字符串常量池的作用了解吗**

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

```java
// 在堆中创建字符串对象”ab“
// 将字符串对象”ab“的引用保存在字符串常量池中
String aa = "ab";
// 直接返回字符串常量池中字符串对象”ab“的引用
String bb = "ab";
System.out.println(aa==bb);// true
```



## 引用类型总结

```
1．强引用
只有不被gc root对象引用的时候，才会回收强引用对象
2.软引用
当GC Root指向软引用对象时，在内存不足时，会回收软引用所引用的对象
3.弱引用
只有弱引用引用该对象时，在垃圾回收时，无论内存是否充足，都会回收弱引用所引用的对象
4.虚引用
"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用主要用来跟踪对象被垃圾回收的活动。
虚引用和终结器引用必须配合引用队列。
当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。
5.终结器引用
所有的类都继承自Object类，Object类有一个finalize方法。当某个对象不再被其他的对象所引用时，会先将终结器引用对象放入引用队列中，然后根据终结器引用对象找到它所引用的对象，然后调用该对象的finalize方法。调用以后，该对象就可以被垃圾回收了
```



## 垃圾回收算法分类

两栈两方法区
1.虚拟机栈中引用的对象
2.本地方法栈引用的对象
3.方法区中类静态属性引用的对象
4.方法区中常量引用的对象

![image-20230302222209069](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302222209069.png)

```
标记-清除
它分为“标记”和“清除”两个阶段：首先标记出所需回收的对象，在标记完成后统一回收掉所有被标记的对象，它的标记过程其实就是前面的可达性分析算法中判定垃圾对象的标记过程。缺点：产生碎片太多

标记-整理
在完成标记之后，它不是直接清理可回收对象，而是将存活对象都整理到一端，然后清理掉端边界以外的内存。

复制
将内存分为等大小的两个区域，FROM和TO（TO中为空）。先将被GC Root引用的对象(存活的)从FROM放入TO中，再回收不被GC Root引用的对象。然后交换FROM和TO。这样也可以避免内存碎片的问题，但是会占用双倍的内存空间。

标记--整理垃圾回收算法合适老年代（存活率高） ，复制回收算法合适新生代（存活率低）
```

**分代回收算法**

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150931.png)

```
回收流程：
1.新创建的对象都被放在伊甸园区
2.当伊甸园区内存不足时，进行一次垃圾回收，minorGC
3.Minor GC 会将伊甸园和幸存区FROM存活的对象先复制到 幸存区 TO中， 并让其寿命加1。垃圾回收完再交换两个幸存区。
4.如果幸存区中的对象的寿命超过某个阈值（最大为15，4bit），就会被放入老年代区
5.如果新生代、老年代的内存都满了，就会先触发MinorGC，再触发Full GC，扫描新生代和老年代中所有不再使用的对象并回收

大对象处理策略
当遇到较大对象时，就算新生代的伊甸园为空，也无法容纳该对象时，会将该对象直接晋升为老年代。
线程内存溢出
某个线程的内存溢出了而抛异常，不会让其他的线程结束运行。这是因为当一个线程OOM异常后，它所占据的内存会全部被释放掉，从而不会影响其他线程的运行，进程依然正常。
```

## 垃圾回收器分类

新生代垃圾收集器(复制)：Serial 收集器(单)、ParNew 收集器(多)、Parallel Scavenge 收集器(多)
老年代垃圾收集器(标记-整理)：Serial Old 收集器(单)、Parallel Old 收集器(多)、CMS 收集器(并发、标记-清除)
新生代和老年代垃圾收集器:G1收集器

**CMS收集器-并发收集器**

Concurrent Mark Sweep，一种以获取**最短回收停顿时间**为目标的**老年代**收集器
**特点**：基于**标记-清除算法**实现。并发收集、低停顿，但是会产生内存碎片

<img src="https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608151052.png" alt="img" style="zoom:67%;" />

**CMS收集器的运行过程分为下列4步：**

**初始标记**：标记GC Roots能直接到的对象。速度很快但是**仍存在Stop The World问题**
**并发标记**：进行GC Roots Tracing 的过程，找出存活对象且用户线程可并发执行
**重新标记**：为了**修正并发标记期间**因用户程序继续运行而导致标记产生变动的那一部分对象的标记记录。仍然存在Stop The World问题。
**并发清除**：对标记的对象进行清除回收，并发执行。



### G1收集器

JDK 9以后默认使用，而且替代了CMS 收集器

![img](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200909201212.png)

```
特点：
1、G1从整体上来看基于“标记-整理”算法实现的收集器，从局部上看是基于复制算法实现的，因此G1运行期间不会产生空间碎片。
2、可预测的停顿。G1能建立可预测的时间停顿模型，能让使用者明确指定一个长度为M毫秒的时间片段内，消耗在垃圾收集上的时间不得超过N毫秒。

过程：
1、初始标记。标记出GC Roots直接关联的对象，这个阶段速度较快，需要停止用户线程，单线程执行。
2、并发标记。从 GC Root开始对堆中的对象进行可达新分析，找出存活对象，这个阶段耗时较长，但可以和用户线程并发执行。
3、最终标记。修正在并发标记阶段引用户程序执行而产生变动的标记记录。
4、筛选回收。选回收阶段会对各个 Region 的回收价值和成本进行排序，根据用户所期望的 GC 停顿时间来指定回收计划（用最少的时间来回收包含垃圾最多的区域，这就是 Garbage First ，第一时间清理垃圾最多的区块），这里为了提高回收效率，并没有采用和用户线程并发执行的方式，而是停顿用户线程。（这一步和cms收集器有区别）。
```

## 跨代引用问题

```java
跨代引用是指新生代中存在对老年代对象的引用，或者老年代中存在对新生代的引用。

假如要现在进行一次只局限于新生代区域内的收集（Minor GC），但新生代中的对象是完全有可能被老年代所引用的，为了找出该区域中的存活对象，不得不在固定的 GC Roots 之外，再额外遍历整个老年代中所有对象来确保可达性分析结果的正确性，反过来也是一样。无疑会为内存回收带来很大的性能负担。

解决办法：记忆集==卡表
每个Region又被分成了若干个大小为512字节的Card，这些Card都会记录在全局卡表中。Card中的每个元素对应着其标识的内存区域中一块特定大小的内存块，这个内存块被称为卡页。一个卡页的内存中通常不止一个对象，只有卡页中有一个及以上对象的字段存在着跨Region引用，这个对应的元素的值就标识为1。脏卡
这样在Minor GC时，只需要将变脏的Region中的那个卡页加入GC Roots一并扫描即可。比起扫描老年代的所有对象，大大减少了扫描的数据量，提升了效率。
```

## 类加载阶段

系统加载 Class 类型的文件主要三步：**加载->连接->初始化**。连接过程又可分为三步：**验证->准备->解析**。

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230302142733033.png" alt="image-20230302142733033" style="zoom: 80%;" />

```
1.加载
类的加载主要的职责为将.class文件的二进制字节流读入内存，并在方法区中创建Class对象，作为.class文件进入内存后的数据的访问入口。如果此类的父类没有加载，先加载父类。 并且加载是懒惰执行

2.连接- 验证、准备、解析
验证： 该阶段主要是为了验证类是否符合Class的规范。其中有对元数据的验证，例如检查类是否继承了被final修饰的类；还有对符号引用的验证，例如校验符号引用是否可以通过全限定名找到，或者是检查符号引用的权限(private、public)是否符合语法规定等。

准备： 准备阶段的主要任务是为类的静态变量分配空间并赋默认值。
1、静态变量是基本类型（int、long、short、char、byte、boolean、float、double）的默认值为0
2、静态变量是引用类型的，默认值为null
3、静态常量会直接赋值。如：static final int value = 123 ，赋给value的值为123

解析： 该阶段的主要职责为将该类在常量池中的符号引用转变为直接引用。

3.初始化
该阶段主要是为类的静态变量赋初始化值，

类的初始化的懒惰的，只有以下情况会初始化
1.main 方法所在的类，总会被首先初始化
2.首次访问这个类的静态变量或静态方法时
3.子类初始化，如果父类还没初始化，会引发
4.子类访问父类的静态变量，只会触发父类的初始化
5.对类进行反射调用的时候，会进行初始化，如Class.forName
6.new 会导致初始化
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230225231105137.png" alt="image-20230225231105137" style="zoom:150%;" />



## 类加载器

![image-20230210213730826](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230210213730826.png)

启动类加载器：最顶层的加载类，由 C++实现，负责加载 `%JAVA_HOME%/lib`目录下的 jar 包和类。剩下的加载器都继承java.lang.ClassLoader。
扩展类加载器：主要负责加载 `%JRE_HOME%/lib/ext` 目录下的 jar 包和类

### 双亲委派模型

即在类加载的时候，系统会首先判断当前类是否被加载过。已经被加载的类会直接返回，否则才会尝试加载。加载的时候，首先会把该请求委派给父类加载器的 `loadClass()` 处理，因此所有的请求最终都应该传送到顶层的启动类加载器 `BootstrapClassLoader` 中。当父类加载器无法处理时，才由自己来处理。

好处：避免类的重复加载（JVM 区分不同类的方式不仅仅根据类名，相同的类文件被不同的类加载器加载产生的是两个不同的类）

![ClassLoader](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/classloader_WPS%E5%9B%BE%E7%89%87.png)

**自定义类的加载器**

```Java
1.继承ClassLoader父类。
2.要遵从双亲委派机制，重写findClass()方法，
2.1.获取该class文件字节码数组
2.2.调用父类的defineClass方法加载类
3.使用者调用该自定义类加载器的 loadClass 方法，让它委派父类去加载.
```

##### 即时编译器（JIT）与解释器的区别

- 解释器
  - 将字节码**解释**为机器码，下次即使遇到相同的字节码，仍会执行重复的解释。合适不常用的代码
  - 是将字节码解释为针对所有平台都通用的机器码
  
- 即时编译器
  - 将一些字节码**编译**为机器码，**并存入 Code Cache**，下次遇到相同的代码，直接执行，无需再编译。合适热点代码。
  
  - 根据平台类型，生成平台特定的机器码
  
    

## **内存溢出和内存泄漏**

内存溢出 out of memory，是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory

内存泄露 memory leak，是指程序在申请内存后，无法释放已申请的内存空间，最终导致OOM



## JVM内存逃逸分析

Java 中对象的创建一般会由堆内存去分配内存空间来进行存储。**“逃逸分析”** 它的目的就是判断哪些对象是可以存储在栈内存中而不用存储在堆内存中的，从而让其随着线程的消逝而消逝，进而减少了 GC 发生的频率，这也是常见的 JVM 优化技巧之一。Java Hotspot 虚拟机可以分析新创建对象的使用范围，并决定是否在 Java 堆上分配内存的一项技术。在开发中尽可能控制变量的作用范围，让虚拟机对没有逃逸的对象进行优化。

一个对象有三种逃逸状态：

1. **全局逃逸**（GlobalEscape）：即一个对象的作用范围逃出了当前方法或者当前线程，

> 一般有以下几种场景：
> ① 对象是一个静态变量
> ② 对象是一个已经发生逃逸的对象
> ③ 对象作为当前方法的返回值

2.**参数逃逸**（ArgEscape）：即一个对象被作为方法参数传递或者被参数引用，但在调用过程中不会发生全局逃逸，这个状态是通过被调方法的字节码确定的。

3.**没有逃逸**：即方法中的对象没有发生逃逸。

**逃逸分析的优势**：逃逸分析的作用，就是筛选出没有发生逃逸的对象，从而对它们进行以下三方面的优化：

1.同步消除（锁消除）
2.标量替换：基础类型和对象的引用可以理解为标量，对象就是聚合量。如果一个对象没有发生逃逸，那压根就不用创建它，只会在栈或者寄存器上创建它用到的成员标量，节省了内存空间。
3.栈内存分配：将原本分配在堆内存上的对象转而分配在栈内存上，减少堆内存的占用，从而减少 GC 的频次。

# Cookie和Session的区别

```
1.Cookie的工作原理
（1）浏览器端第一次发送请求到服务器端
（2）服务器端创建Cookie，该Cookie中包含用户的信息，然后将该Cookie发送到浏览器端
（3）浏览器端再次访问服务器端时会携带服务器端创建的Cookie
（4）服务器端通过Cookie中携带的数据区分不同的用户

Cookie的根本作用就是在客户端存储用户访问网站的一些信息。
1.记住密码，下次自动登录。
2.购物车功能。
3.记录用户浏览数据，进行商品（广告）推荐。
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190917204655188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW4xMzMzMzMzNjY3Nw==,size_16,color_FFFFFF,t_70)

```
2.Session的工作原理
（1）浏览器端第一次发送请求到服务器端，服务器端创建一个Session，同时会创建一个特殊的Cookie（name为JSESSIONID的固定值，value为session对象的ID），然后将该Cookie发送至浏览器端
（2）浏览器端发送第N（N>1）次请求到服务器端,浏览器端访问服务器端时就会携带该name为JSESSIONID的Cookie对象
（3）服务器端根据name为JSESSIONID的Cookie的value(sessionId),去查询Session对象，从而区分不同用户

Session的根本作用是在服务端存储用户和服务器会话的一些信息。
1.判断用户是否登录
2.购物车功能

区别对比：
(1)cookie数据存放在客户的浏览器上，session数据放在服务器上
(2)cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,如果主要考虑到安全应当使用session
(3)session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，如果主要考虑到减轻服务器性能方面，应当使用COOKIE
(4)单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。
(5)所以：将登陆信息等重要信息存放为SESSION;其他信息如果需要保留，可以放在COOKIE中
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091720521815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW4xMzMzMzMzNjY3Nw==,size_16,color_FFFFFF,t_70)



# 多线程交替打印

1.多线程交替打印奇偶数

```Java
public class OddEvenPrinter {
    private Object monitor = new Object();
    private final int limit;
    private volatile int count;
    
    OddEvenPrinter(int count, int limit){
        this.count = count;
        this.limit = limit;
    }

    private void print(){
        synchronized(monitor){
            while (count < limit){
                try{
                    System.out.println("线程:"+ Thread.currentThread().getName() +"打印：" + count);
                    count++;
                    monitor.notifyAll();
                    monitor.wait();
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
            //防止有子线程被阻塞未被唤醒，导致主线程不退出
            monitor.notifyAll();
        }
    }

    public static void main(String[] args){
        OddEvenPrinter printer = new OddEvenPrinter(0, 10);
        new Thread(printer::print, "ou").start();
        new Thread(printer::print, "ji").start();
    }
}
```

2.三个线程分别打印 A，B，C，要求这三个线程一起运行，打印 n 次，输出形如“ABCABCABC....”的字符串

```java
public class PrintABCUsingLock {
    private int times;
    private int state;
    private Lock lock = new ReentrantLock();

    public PrintABCUsingLock(int times) {
        this.times = times;
    }

    private void printLetter(String name, int targetNum){
        for (int i=0; i<times;){
            lock.lock(); //这里存在线程不精准问题，有可能前10次都是同一个线程抢到
            if (state%3 == targetNum){ //第一个进入的线程是A线程
                state++;
                i++;
                System.out.print(name);
            }
            lock.unlock();
        }
    }

    public static void main(String[] args){
        PrintABCUsingLock loopThread = new PrintABCUsingLock(3);
        new Thread(()->{
            loopThread.printLetter("A", 0);
        }, "A").start();

        new Thread(()->{
            loopThread.printLetter("B", 1);
        }, "B").start();

        new Thread(()->{
            loopThread.printLetter("C", 2);
        }, "C").start();
    }
}
```

3.用lock的条件变量 实现三个线程分别打印 A，B，C，要求这三个线程一起运行，打印 n 次，输出形如“ABCABCABC....”的字符串

```java
public class PrintABCUsingLockCondition {
    private int times;
    private int state = 0;
    private static Lock lock = new ReentrantLock();
    private static Condition c1 = lock.newCondition();
    private static Condition c2 = lock.newCondition();
    private static Condition c3 = lock.newCondition();

    public PrintABCUsingLockCondition(int times) {
        this.times = times;
    }

    public void print(String name, int targetState, Condition current, Condition next){
        for (int i=0; i<times; ){
            lock.lock();
            try {
                while (state%3 != targetState){
                    current.await();
                }
                //运行到这里就是正确的线程
                System.out.print(name);
                state++;
                i++;  //这个i不是volatile，是每个线程独享的
                //精确唤醒线程
                next.signal();
            }catch(Exception e){
                e.printStackTrace();
            }finally {
                lock.unlock();
            }
        }
    }

    public static void main(String[] args){
        PrintABCUsingLockCondition printer = new PrintABCUsingLockCondition(5);

        new Thread(()->{
            printer.print("A", 0, c1, c2);
        }).start();

        new Thread(()->{
            printer.print("B", 1, c2, c3);
        }).start();

        new Thread(()->{
            printer.print("C", 2, c3, c1);
        }).start();
    }
}
```



## 线程池

线程池状态： 运行状态---- 关闭 --- 停止--- 整理 ---- 终结状态

![image-20230221222540275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230221222540275.png)

![image-20230221222637034](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230221222637034.png)

```Java
阻塞队列的种类：直接提交队列、有界任务队列、无界任务队列、优先任务队列
1.直接提交队列
使用SynchronousQueue队列，提交的任务不会被保存，总是会马上提交执行
2.有界任务队列
使用ArrayBlockingQueue有界任务队列，和下面的线程池工作方式是一样的
3.无界任务队列
使用LinkedBlockingQueue来创建无界任务队列，线程池的任务队列可以无限制的添加新的任务，而线程池创建的最大线程数量就是corePoolSize设置的数量，也就是说在这种情况下maximumPoolSize这个参数是无效的
4.优先任务队列
优先任务队列通过PriorityBlockingQueue实现，按照优先级来处理任务。其他的队列是按照按照先后执行。

线程池工作方式：
1.线程池刚开始没有线程，当一个任务提交给线程池后，线程池会创建一个新线程来执行任务。
2.当线程数达到（核心线程数目），并且没有线程空闲，这时再加入任务，新的任务会被加入到阻塞队列。
3.如果任务超过了阻塞队列大小，会创建额外的 （最大线程数-核心线程数目）线程来救急
4.如果队列已满并线程已达到最大线程数目，依然有新任务被提交，此时会执行拒绝策略。拒绝策略如下：
	a.AbortPolicy      让调用者抛出异常，默认策略
	b.CallerRunsPolicy 让调用者自个运行任务
	c.DiscardPolicy    放弃本次任务
	d.DiscardOldestPolicy 放弃队列中最早的任务，新的任务取而代之。

5.当高峰过去之后，超过核心线程数目 的救急线程如果处于空闲，需要结束节省资源
    
    
```

## 阻塞队列

阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。这两个附加的操作是：在队列为空时，获取元素的线程会等待队列变为非空。当队列满时，存储元素的线程会等待队列可用。

阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。

![image-20231022230458433](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231022230458433.png)

[Java 阻塞队列--BlockingQueue - 北极猩球 - 博客园 (cnblogs.com)](https://www.cnblogs.com/bjxq-cs88/p/9759571.html)



## Executors类中提供的工厂方法

newFixedThreadPool----定长的线程池

特点 

- 核心线程数 == 最大线程数（没有救急线程被创建），因此也无需超时时间 ，一个定长的线程池
- 阻塞队列是无界的，可以放任意数量的任务

**评价** 适用于任务量已知，相对耗时的任务

newCachedThreadPool

特点 

- 核心线程数是 0，最大线程数是 Integer.MAX_VALUE，救急线程的空闲生存时间是 60s，意味着 

- - 全部都是救急线程（60s 后可以回收）
  - 救急线程可以无限创建 

newSingleThreadExecutor---单线程化的线程池

使用场景： 

希望多个任务排队执行。线程数固定为 1，任务数多于 1 时，会放入无界队列排队。

任务执行完毕，这唯一的线程也不会被释放。和自己创建一个线程来工作有什么区别？如果是自己创建一个单线程执行任务，如果任务执行失败会终止，没有补救措施。而线程池还会新建一个线程，保证池的正常工作。

**newScheduledThreadPool**--定时任务的线程池

创建一个定长线程池。支持定时及周期性去执行任务



## 线程池优雅关闭

Java自带的线程池默认没有优雅关闭，但是spring封装的ThreadPoolTaskExecutor 默认优雅关闭。

# Spring

## 1.spring ioc容器的理解

![image-20230223113533863](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223113533863.png)

![image-20230223141747882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223141747882.png)

![image-20230223143437061](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223143437061.png)

```Java
自己总结的spring ioc整体流程：
1.第一步肯定是创建一个BeanFactory， Bean工厂对象，这个对象来负责bean的注册，bean的创建等一系列工作。
    
2.BeanDefinition阶段：  （Bean的定义阶段）
首先通过注解或者xml的方式，加载解析bean对象，创建 定义对象BeanDefinition，把定义对象存储到BDMap中
执行Bean工厂的后置处理器，此处是扩展点，允许我们在bean实例化之前修改bean的定义信息（增强）

3.Bean实例化阶段：  （从这步开始为Bean的生命周期）
通过反射的方式把BeanDefinition对象实例化成具体的bean对象，此时bean还是个半成品。

4.Bean初始化阶段：
对Bean实例的属性进行填充
执行Aware接口方法回调
调用BeanPostProcessor接口的前置对象处理方法、Bean的初始化init方法、调用BeanPostProcessor接口的后置对象处理方法
（注意：BeanPostProcesssor是实现AOP的关键！！！）
    
5.生成完整的Bean对象，并存储单例池中，通过getBean方法直接获取。
    
 两种bean的注册方式
 方法一：通过@Bean+@Configuration（在类上） 的方式直接定义要创建的对象 与对象的关系
 方法二：通过@Component 定义类，这种方式必须使用@ComponentScan定位Bean扫描路径
```

**spring bean的生命周期**

![image-20230223154556882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223154556882.png)

**spring 循环依赖问题**

![image-20230223154930800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223154930800.png)

```
Spring 循环依赖可能出现的三种方式
第一种：构造器参数循环依赖
第二种：setter方式单例，默认方式
第三种：setter方式原型，prototype

1、 Spring bean初始化的循环依赖只能解决单例模式的set方式（依靠==第三级缓存==提前暴露==无参构造函数==new出的对象）
2、 scope="prototype"(原型模式)时，三级缓存不保存非单例模式的bean对象，所以无法解决。
```



<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223160248618.png" alt="image-20230223160248618" style="zoom: 80%;" />

![image-20230223160455306](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223160455306.png)

```
解决循环依赖的原理：
Bean对象实例化后，把它给缓存起来。提前曝光出来给大家使用。
流程：（场景）A的某个field或者setter依赖了B的实例对象，同时B的某个field或者setter依赖了A的实例对象
1.A首先完成了Bean对象实例化之后，并且将自己提前曝光到singletonFactories中
2.发现自己依赖对象B，此时就尝试去get(B)，发现B还没有被创建，所以走创建beanB流程
3.B在初始化第一步的时候发现自己依赖了对象A，于是尝试get(A)，尝试一级缓存singletonObjects(肯定没有，因为A还没初始化完全)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，由于A通过ObjectFactory 将自己提前曝光了，所以B能够通过ObjectFactory.getObject拿到A对象(虽然A还没有初始化完全，但是总比没有好呀),因此B对象正常完成初始化。
4.此时返回A中，A此时能拿到B的对象顺利完成自己的初始化。
```



## 2.Bean 的作用域有哪些?

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 `getBean()` 两次，得到的是不同的 Bean 实例。
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。
- **application/global-session** （仅 Web 应用可用）： 每个 Web 应用在启动时创建一个 Bean（应用 Bean），该 bean 仅在当前应用启动时间内有效。
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

------

## **spring的单例模式与线程安全**

在什么情况下，单例的Bean对象存在线程安全问题
1.对于原型Bean,每次创建一个新对象，也就是线程之间并不存在Bean共享，自然是不会有线程安全的问题。
2.如果单例Bean,是一个无状态Bean，也就是线程中的操作不会对Bean的成员执行查询以外的操作，那么这个单例Bean是线程安全的。比如Spring mvc 的 Controller、Service、Dao等，这些Bean大多是无状态的，只关注于方法本身。
3.对于单例Bean，当Bean对象对应的类存在可变的成员变量并且其中存在改变这个变量的线程时，多线程操作该Bean对象时会出现线程安全。

###### 原因

当多线程中存在线程改变了bean对象的可变成员变量时，其他线程无法访问该bean对象的初始状态，从而造成数据错乱。

###### 解决办法

1. 在Bean对象中尽量避免定义可变的成员变量；
2. 在bean对象中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在ThreadLocal中





## 2.**BeanFactory 和 ApplicationContext 区别和联系**

在 Spring 中，BeanFactory 和 ApplicationContext 都是用于**管理 Spring Bean 的容器**，它们的区别和联系如下。

区别：

1. BeanFactory 是 Spring 框架的**基础设施**，**用于管理 Bean 的生命周期和依赖关系**，提供了 **IoC** 和 **DI** 功能。**ApplicationContext 是 BeanFactory 的子接口**，提供了更多的功能，例如**国际化支持**、**AOP 支持**等。
2. BeanFactory 是**延迟加载**的，即只有在**获取 Bean 时才会进行实例化**，可以减少系统资源的占用。而 ApplicationContext 在**启动时会立即加载所有的 Bean**，导致启动时间较长。
3. BeanFactory 是**单例模式**，即在整个应用中只有一个 BeanFactory 实例。而 ApplicationContext 可以有**多个实例**，并且可以通过**父子容器**的方式组织起来，方便模块化开发。

联系：

1. BeanFactory 和 ApplicationContext 都是用于**管理 Spring Bean 的容器**，可以**管理 Bean 的生命周期和依赖关系**，提供了 **IoC** 和 **DI** 功能。
2. ApplicationContext 是 BeanFactory 的**扩展**，提供了更多的功能，例如**国际化支持、AOP 支持**等，同时也**支持 BeanFactory 的所有功能**。
3. BeanFactory 和 ApplicationContext 都可以**管理单例 Bean 和原型 Bean**，可以**控制 Bean 的作用域和生命周期**





## 3.Spring aop的理解

**aop是在spring ioc 的bean初始化阶段的BeanPostProcesssor处理方法实现的！！**

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP 就是基于动态代理的，如下图所示：

![image-20230223152620027](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223152620027.png)

![image-20230223152714961](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223152714961.png)

![image-20230223152923416](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223152923416.png)

AspectJ 定义的通知类型有哪些？

- **Before**（前置通知）：目标对象的方法调用之前触发
- **After** （后置通知）：目标对象的方法调用之后触发
- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发
- **AfterThrowing**（异常通知） ：目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。
- **Around** （环绕通知）：编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方法

------







## 4.Spring事务



![image-20230223162055038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223162055038.png)

注解式事务管理：通过在方法上添加 @Transactional 注解，开发人员可以非常方便地声明事务的属性，例如隔离级别、传播行为等（把问题引导事务的隔离级别和传播行为）。

事务隔离级别：

```
1.DEFAULT：Spring 中默认的事务隔离级别，以连接的数据库的事务隔离级别为准；
2.读未提交
3.读已提交
4.可重复读
5.串行化
```

事务的传播行为：

spring事务默认的传播行为是REQUIRED。 mandatory：强制的  nested：嵌套

![image-20230223161555480](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223161555480.png)

![这里写图片描述](https://img-blog.csdn.net/20170420213432872?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29vbmZseQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**spring事务的失效场景**

```Java
1.注解@Transactional配置的方法非public权限修饰；
@Transactional 只能用于 public 的方法上，否则事务不会失效，如果要用在非 public 方法上，可以开启 AspectJ 代理模式。

2.注解@Transactional所在类非Spring容器管理的bean；
类上少了类似：@Component，@Controller,@Service, @Mapper或@Repository等注解。

3.同一个类中方法互相调用（自调用）
最容易踩坑的：在类A里面有方法a 和方法b， 然后方法b上面用 @Transactional加了方法级别的事务，在方法a里面 调用了方法b， 方法b里面的事务不会生效。
原因：Spring在扫描Bean的时候会自动为标注了@Transactional注解的类生成一个代理类（proxy）,当有注解的方法被调用的时候，实际上是代理类调用的，代理类在调用之前会开启事务，执行事务的操作，但是同类中的方法互相调用，相当于this.B()，此时的B方法并非是代理类调用，而是直接通过原有的Bean直接调用，所以注解会失效。

4.传播行为配置成了不支持事务：Propagation.NOT_SUPPORTED表示不以事务运行，当前若存在事务则挂起。

5.业务代码抛出的异常类型非RuntimeException，导致事务失效，默认下，事务只对Error和RuntimeException及子类异常回滚！
@Transactional注解修饰的方法，加上rollbackfor属性值，指定回滚异常类型：@Transactional(propagation = Propagation.REQUIRED,rollbackFor = Exception.class)

6.业务代码中存在异常时，使用try…catch…语句块捕获异常，而catch语句块没有抛出异常;

7.数据库引擎不支持事务，MyISAM存储引擎不支持事务，Spring事务是基于数据库事务的。
```

**spring事务的原理**

- Spring 对事务的管理底层实现方式是**基于 AOP 实现的，采用 AOP 的方式进行了封装**，核心接口是 PlatformTransactionManager：

​	当在某个类或者方法上使用 @Transactional 注解后，**spring 会基于该类生成一个代理对象**，并将这个代理对象作为 bean。当调用这个代理对象的方法时，如果有事务处理，则会先关闭事务的自动功能，然后执行方法的具体业务逻辑，如果业务逻辑没有异常，那么代理逻辑就会直接提交，如果出现任何异常，那么直接进行回滚操作。当然我们也可以控制对哪些异常进行回滚操作。

![image-20230316232124739](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230316232124739.png)



## 5.@Autowired和@Resource的区别

```
@Autowired 属于 Spring 的；@Resource 为jdk提供的注解。
@Autowired的类型优先，通过bean的类型去寻找，如果找到多个相同类型的bean，再通过bean的name去找。
@Resource的name优先，先根据bean的name去查找bean。
```



@Component 和 @Bean 的区别是什么？

- `@Component` 注解作用于类，而`@Bean`注解作用于方法。
- `@Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。



## 6. SpringMVC

**spring mvc 的请求处理过程（工作原理）**

![image-20230223204618369](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230223204618369.png)

SpringMVC 的执行流程如下。

1. 用户点击某个请求路径，发起一个 HTTP request 请求，该请求会被提交到 DispatcherServlet（前端控制器）；
2. 由 DispatcherServlet 请求一个或多个 HandlerMapping（处理器映射器），并返回一个执行链（HandlerExecutionChain）。
3. DispatcherServlet 将返回的 Handler 信息发送给 HandlerAdapter（处理器适配器）；
4. HandlerAdapter 根据 Handler 信息找到并执行相应的 Handler（常称为 Controller）；
5. Handler 执行完毕后会返回给 一个 ModelAndView 对象（Spring MVC的底层对象，包括 Model 数据模型和 View 视图信息）；
6. HandlerAdapter 接收到 ModelAndView 对象后，将其返回给 DispatcherServlet ；
7. DispatcherServlet 接收到 ModelAndView 对象后，会请求 ViewResolver（视图解析器）对视图进行解析；
8. ViewResolver 根据 View 信息匹配到相应的视图结果，并返回给 DispatcherServlet；
9. DispatcherServlet 接收到具体的 View 视图后，进行视图渲染，将 Model 中的模型数据填充到 View 视图中的 request 域，生成最终的 View（视图）；
10. 视图负责将结果显示到浏览器（客户端）。

简单描述：前端控制器，通过处理器映射器、处理器适配器找到Handler处理器（Controller），处理完后返回ModelAndView，由前端控制器转发到视图解析器进行解析，获取View视图，最终视图渲染返回给浏览器

**Springmvc常用注解**

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。

**SpringMVC怎么样设定重定向和转发的？**

1）转发：在返回值前面加"forward:"，例如"forward:user.do?name=method4"

（2）重定向：在返回值前面加"redirect:"，例如"redirect:http://www.baidu.com"



## 7.拦截器

```Java
preHandle()方法
这个方法在业务处理器处理请求之前被调用，以可以在这个方法中进行一些前置初始化操作或者是对当前请求的一个预处理，也可以在这个方法中进行一些判断来决定请求是否要继续进行下去。该方法的返回值是布尔值Boolean 类型的，当它返回为false 时，表示请求结束，后续的Interceptor 和Controller都不会再执行；当返回值为true 时就会继续调用下一个Interceptor 的preHandle 方法，如果已经是最后一个Interceptor 的时候就会是调用当前请求的Controller 方法。

postHandle()方法
 这个方法在当前请求进行处理之后，也就是Controller方法调用之后执行，但是它会在DispatcherServlet 进行视图返回渲染之前被调用，所以我们可以在这个方法中对Controller处理之后的ModelAndView 对象进行操作。postHandle 方法被调用的方向跟preHandle 是相反的，也就是说先声明的Interceptor 的postHandle 方法反而会后执行。
 
 afterCompletion()方法
  该方法也是需要当前对应的Interceptor 的preHandle 方法的返回值为true 时才会执行。顾名思义，该方法将在整个请求结束之后，也就是在DispatcherServlet渲染了对应的视图之后执行。这个方法的主要作用是用于进行资源清理工作的。afterCompletion方法被调用的方向和perHandle也是相反的，先声明的Interceptor的afterCompletion方法后执行。
```

## 8.过滤器

1、过滤器 (Filter)
过滤器的配置比较简单，直接实现Filter 接口即可，也可以通过@WebFilter注解实现对特定URL拦截，看到Filter 接口中定义了三个方法。

init() ：该方法在容器启动初始化过滤器时被调用，它在 Filter 的整个生命周期只会被调用一次。注意：这个方法必须执行成功，否则过滤器会不起作用。

doFilter() ：容器中的每一次请求都会调用该方法， FilterChain 用来调用下一个过滤器 Filter。

destroy()： 当容器销毁 过滤器实例时调用该方法，一般在方法中销毁或关闭资源，在过滤器 Filter 的整个生命周期也只会被调用一次

```Java
@Component
public class MyFilter implements Filter {
    
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("Filter 前置");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("Filter 处理中");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        System.out.println("Filter 后置");
    }
}

```



# Spring Boot

**springboot 自动配置原理**

Spring Boot 通过`@EnableAutoConfiguration`开启自动装配，通过 SpringFactoriesLoader 最终加载`META-INF/spring.factories`中的自动配置类实现自动装配，自动配置类其实就是通过`@Conditional`按需加载的配置类，想要其生效必须引入`spring-boot-starter-xxx`包实现起步依赖.



1. 在springboot的启动类中使用了@SpringBootApplication注解，里面的**@EnableAutoConfiguration**注解是自动配置的核心，该注解内部使用@Import(AutoConfigurationImportSelector.class)（class文件用来哪些加载配置类）注解来加载配置类，并不是所有的bean都会被加载，只有导入了对应的start，就有对应的启动器，才满足@ConditionalOnClass条件，才加载。
2. @EnableAutoConfiguration 给容器导入META-INF/spring.factories 里定义的自动配置类，并且根据导入的对应的starter，来筛选有效的自动配置类。每一个自动配置类结合对应的 xxxProperties.java 的类来读取配置文件(.yaml 或者 property文件)进行自动配置功能。
3. 所以我们在项目中全局配置文件application.yml中可以修改server.port :8081等等。

@SpringBootApplication = @ComponentScan + @EnableAutoConfiguration + @Configuration

![在这里插入图片描述](https://img-blog.csdnimg.cn/23d87633cf954e8bb23a2e284fa6a30f.jpeg#pic_center)

**Spring Boot 的核心配置文件有哪几个？它们的区别是什么？**

Spring Boot 的核心配置文件是 application 和 bootstrap 配置文件。

application 配置文件这个容易理解，主要用于 Spring Boot 项目的自动化配置。

bootstrap 配置文件有以下几个应用场景。

- 使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息；



# 微服务知识

## nacos的服务注册与发现



![nacos注册中心面试总结_大数据_02](https://s2.51cto.com/images/blog/202207/09002008_62c85938a8fce58974.jpg?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)

```
1.服务注册：Nacos 客户端会通过发送REST请求的方式向Nacos Server注册自己的服务，提供自身的元数据，比如ip地址、端口等信息。Nacos Server接收到注册请求后，就会把这些元数据信息存储在一个双层的内存Map中。

2.服务心跳：在服务注册后，Nacos 客户端会维护一个定时心跳来持续通知Nacos Server，说明服务一直处于可用状态，防止被剔除。默认5s发送一次心跳。

3.服务同步：Nacos Server集群之间会互相同步服务实例，用来保证服务信息的一致性。 leader raft

4.服务发现：服务消费者（Nacos Client）在调用服务提供者的服务时，会发送一个REST请求给Nacos Server，获取上面注册的服务清单，并且缓存在Nacos Client本地，同时会在Nacos Client本地开启一个定时任务定时拉取服务端最新的注册表信息更新到本地缓存。也就是如果注册中心挂了，还是能访问服务的，因为本地缓存了服务的地址。

5.服务健康检查：Nacos Server会开启一个定时任务用来检查注册服务实例的健康情况，对于超过15s没有收到客户端心跳的实例会将它的healthy属性置为false(客户端服务发现时不会发现)，如果某个实例超过30秒没有收到心跳，直接剔除该实例(被剔除的实例如果恢复发送心跳则会重新注册)
```



## OpenFeign调用服务原理

`OpenFeign`是内部实现了rest服务调用

![在这里插入图片描述](https://img-blog.csdnimg.cn/f5f38a10fb624e29a1d8389752f92057.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5puz5Yeh,size_20,color_FFFFFF,t_70,g_se,x_16)

Feign 和OpenFeign的区别

1）Feign是Spring Cloud组件中一个轻量级RESTful的HTTP服务客户端，Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务。Feign的使用方式是：使用Feign的注解定义接口，调用接口，就可以调用服务注册中心的服务。也就是Fegin本身不支持Springmvc的注解。

2）OpenFeign是Spring Cloud在Feign的基础上支持了SpringMVC的注解，如@RequestMapping等等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

**Feign远程调用流程图**

<img src="https://upload-images.jianshu.io/upload_images/6271376-7635e2dc9b32e3ec.png?imageMogr2/auto-orient/strip|imageView2/2/w/871/format/webp" alt="img" style="zoom:80%;" />



(1) 基于面向接口的动态代理方式生成实现类

在使用feign 时，会定义对应的接口类，在接口类上使用Http相关的注解，标识HTTP请求参数信息。
同时，在Feign底层，会通过基于面向接口的动态代理方式生成实现类，将请求调用委托到动态代理实现类。如下：

<img src="https://upload-images.jianshu.io/upload_images/14126519-4949493085b0f547.png?imageMogr2/auto-orient/strip|imageView2/2/w/779/format/webp" alt="img" style="zoom:80%;" />

(2) 根据Contract协议规则，解析接口类的注解信息，解析成内部表现：

<img src="https://upload-images.jianshu.io/upload_images/6271376-cebfed0fa4f18190.png?imageMogr2/auto-orient/strip|imageView2/2/w/915/format/webp" alt="img" style="zoom: 67%;" />

(3) 基于 RequestBean，动态生成Request

根据传入的Bean对象和注解信息，从中提取出相应的值，来构造Http Request 对象

(4) 使用Encoder 将Bean转换成 Http报文正文（消息解析和转码逻辑）

Feign 最终会将请求转换成Http 消息发送出去，传入的请求对象最终会解析成消息体，如下所示：

<img src="https://upload-images.jianshu.io/upload_images/14126519-b5c571b44f453707.png?imageMogr2/auto-orient/strip|imageView2/2/w/719/format/webp" alt="img" style="zoom:67%;" />

(5) 发送Http请求

Feign 真正发送HTTP请求是委托给 feign.Client 来做的。

Feign 默认底层通过JDK 的 java.net.HttpURLConnection 实现了feign.Client接口类,**在每次发送请求的时候，都会创建新的HttpURLConnection 链接**，这也就是为什么默认情况下Feign的性能很差的原因。可以通过拓展该接口，使用Apache HttpClient 或者OkHttp3等基于连接池的高性能Http客户端。

**Feign 整体框架非常小巧，在处理请求转换和消息解析的过程中，基本上没什么时间消耗。真正影响性能的，是处理Http请求的环节。**



## HTTP 和 RPC 有什么区别

1.服务发现

在 **HTTP** 中，你知道服务的域名，就可以通过 **DNS 服务**去解析得到它背后的 IP 地址，默认 80 端口。

而 **RPC** 的话，就有些区别，一般会有专门的**中间服务**去保存服务名和IP信息，比如 nacos。想要访问某个服务，就去这些中间服务去获得 IP 和端口信息。

2.底层连接形式

以主流的 **HTTP/1.1** 协议为例，其默认在建立底层 TCP 连接之后会一直保持这个连接（**Keep Alive**），之后的请求和响应都会复用这条连接。

 **RPC** 协议，也跟 HTTP 类似，也是通过建立 TCP 长链接进行数据交互，但不同的地方在于，RPC 协议一般还会再建个**连接池**，在请求量大的时候，建立多条连接放在池内，要发数据的时候就从池里取一条连接出来，**用完放回去，下次再复用**。

3.传输的内容

对于主流的 HTTP/1.1，在 Body 这块，它使用 **Json** 来**序列化**结构体数据。

而 RPC，因为它定制化程度更高，可以采用体积更小的 Protobuf 或其他序列化协议去保存结构体数据，同时也不需要像 HTTP 那样考虑各种浏览器行为，比如 302 重定向跳转啥的。**因此性能也会更好一些**。

# 分布式共识算法

## **cap理论**

C:Consistency一致性，在分布式系统中的所有节点中，在同一时间的账本是否为一致的。（强一致性）

A:可用性，指的是客户端的请求，集群都能得到响应数据。但系统可用并不代表存储系统的所有节点提供的数据一致。如客户端想要读取文章评论,, 但评论缺少最新的一条。在这种情况下, 我们仍然可以说系统可用。

P:分区容错性，指的是分布式系统可能由于网络不可靠原因，会存在部分系统分区崩溃的情况，但是系统仍会继续提供服务，不会挂掉。

cap理论指的是这三种是无法同时满足的，对于分布式系统，保证分区容错性是基本的。因此分布式系统只能满足AP CP.

那为什么说在P满足的情况下，为什么说CA不能同时满足呢？我们来通过假设看一看，如果CA同时满足会怎么样：

（1）假设现在要求满足C（一致性），那么就是说所有的节点在某一刻提供的数据都必须一致，我们知道在P的情况，是不可能保证的，要保证的话，就只能把其他节点全部干掉，比如禁止读写，那这其实就是和A是相悖的（某些节点虽然延迟，但是节点本身可用）

（2）假设现在要求满足A（可用性），那么就是说只要节点本身没什么问题，就可以对外提供服务，哪怕有点数据延迟，很明显这肯定是和C相悖的。



## **Paxos算法**

Paxos算法运行在允许宕机故障的异步系统中，不要求可靠的消息传递，可容忍消息丢失、延迟、乱序以及重复。它利用大多数 (Majority) 机制保证了2F+1的容错能力。这个是basic paxos， 只能对单个值进行决议

Paxos将系统中的角色分为`提议者 (Proposer)`，`决策者 (Acceptor)`，和`最终决策学习者 (Learner)`:

- `Proposer`: 提出提案 (Proposal)。Proposal信息包括提案编号 (Proposal ID) 和提议的值 (Value)。
- `Acceptor`: 参与决策，回应Proposers的提案。收到Proposal后可以接受提案，若Proposal获得多数Acceptors的接受，则称该Proposal被批准。
- `Learner`: 不参与决策，从Proposers/Acceptors学习最新达成一致的提案(Value)。

------

![img](https://www.nenggz.com/images/alg/alg-dst-paxos-2.jpg)

![image-20230405224640415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405224640415.png)

缺点：效率很低，因为：

- 同时存在多个proposer，存在提案内容冲突等情况
- 通过一个提案需要两个来回的rpc调用(prepare阶段和accept阶段)

解决方案：

1. 选举leader：无论任何时候，只有一个服务器是proposer
2. 压缩prepare过程的rpc：

- 一次让所有的日志执行prepare阶段(并不是一条日志执行一次)

- 大部分的日志条目在一个来回的rpc调用中就可以确定

  

### Multi-Paxos算法

原始的Paxos算法（Basic Paxos）只能对一个值形成决议，决议的形成至少需要两次网络来回，在高并发情况下可能需要更多的网络来回，极端情况下甚至可能形成活锁。如果想连续确定多个值，Basic Paxos搞不定了。

实际应用中几乎都需要连续确定多个值，而且希望能有更高的效率。Multi-Paxos正是为解决此问题而提出。Multi-Paxos基于Basic Paxos做了两点改进：

1. 针对每一个要确定的值，运行一次Paxos算法实例（Instance），形成决议。每一个Paxos实例使用唯一的Instance ID标识。
2. 在所有Proposers中选举一个Leader，由Leader唯一地提交Proposal给Acceptors进行表决。这样没有Proposer竞争，解决了活锁问题。在系统中仅有一个Leader进行Value提交的情况下，Prepare阶段就可以跳过，从而将两阶段变为一阶段，提高效率。

![img](https://pic1.zhimg.com/80/v2-e5cd197abc9c922ca4ca91c3df74fa70_720w.webp)



## **Raft算法**

Leader选举：

1.系统在刚开始的时候，所有节点都是Follower节点，这时都有机会参与选举，将自己变成Candidate，看谁的定时器最快结束，谁变成候选者。

2.变成候选者的节点，会先投自己一票，并且告诉其他节点，请求投票。每个节点在当前任期中，只会对一个候选者投票。当收到超过半数以上的票数时，候选者变成领导者。在特殊的情况下，可能会出现多个候选者，并且票数也是一样，这个时候候选者都会随机推迟一段时间再次请求投票，直到出现唯一的领导者。

3.在Leader节点选举出来以后，Leader节点会不断的发送心跳给其它Follower节点证明自己是活着的，其他Follower节点在收到心跳后会清空自己的定时器，并回复给Leader，因为此时没必要触发选举了。

4.如果Leader节点在某一刻挂了，那么Follower节点就不会收到心跳，因此在定时器到来时就会触发新一轮的选举。

数据日志的复制：

正常情况下的日志复制

1.当领导节点收到客户端的请求变更时，会把变更记录到日志,然后领导节点会将这个变更随着下一次心跳通知给跟随者，收到消息的跟随者同样把变更写入到日志中，然后回复领导节点。

2.当领导节点收到大多数的回复后，就把变更写入自己的存储空间，同时回复客户端，并告诉跟随者应用这个日志，集群变更就达成共识。

如果存在网络分区的日志复制：

实际上，在 网络分区的情况下，部分节点之间没办法互相通信，Raft 也能保证这种情况下数据的一致性。原因在于：等到网络状态恢复时，节点再次处于同一个网络状态下，任期数较小的领导节点会自动降级为跟随者，同时删除掉没有共识成功的日志。这样所有的节点日志又恢复了一致。

![img](https://pic3.zhimg.com/80/v2-f6c1319c3f50c2ab13e1bbb5dfc839d6_720w.webp)

Raft与Multi-Paxos中相似的概念:

![img](https://pic1.zhimg.com/80/v2-a932cb62a02604d5ec57dc0a046a1414_720w.webp)

Raft与Multi-Paxos的不同：

1.multi-paxos和raft，在选定了leader状态下的行为模式一模一样

2.raft仅允许日志最多的节点当选为leader，而multi-paxos则相反，任意节点都可以当选leader

3.raft不允许日志的空洞，这也是为了比较方便和拉平两个节点的日志方便，而multi-paxos则允许日志出现空洞

![img](https://pic3.zhimg.com/80/v2-7679d235c0ac8056552ba88b677e73a2_720w.webp)



## **PBFT算法**

Raft和Paxos，都是非常高效的算法，他们只支持CFT（Crash fault tolerance），只允许系统内节点宕机（crash），并不考虑系统内有作恶节点。但是对于开放区块链系统（公有链）来说，任何节点都可以加入这个网络中，那么就必须要考虑作恶节点的问题。

**特点**：n > 3f, 消息复杂度为O(n^2)

**缺点：**

- 仅仅适用于(联盟链/私有链)。
- 通信复杂度过高，可拓展性比较低，一般的系统在达到100左右的节点个数时，性能下降非常快。
- PBFT在网络不稳定的情况下延迟很高。

![img](https://pic1.zhimg.com/80/v2-46aa2be22ec65a3f3de58753bf3178c4_720w.webp)

**pre-prepare:** 主节点收到客户端发送来的消息后，构造`pre-prepare`消息结构体，广播到集群中所有节点。

**prepare**：`副本(backup)`收到主节点请求后，会对消息进行检查，检查通过会存储在本节点。节点收到`2f+1`（包括自己）个相同的消息后，会进入`PREPARE`状态，并广播prepare 消息给所有节点。

**commit：**`副本`收到`2f+1`（包括自己）个一致的`PREPARE`消息后，会进入`COMMIT`阶段，并广播commit消息给所有节点

**Reply**:`副本`收到`2f+1`（包括自己）个一致的`COMMIT`个消息后执行`m`中包含的操作，返回操作成功给客户端



# Mybatis

**#{ }  和 ${ } 的区别**

```sql
1.#{} 是占位符，  ${}是拼接符
MyBatis 会将 sql 中的 #{} 替换为?号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的?号占位符设置参数值。

2.#{} 对应的变量会自动加上单引号， ${}不会自动加单引号

3.#{} 能防止sql 注入, 而${}会有sql注入的危险
例如：传参 name = "富贵 or name = 狗蛋"，
select * from `role` where name = ${name}
因为${}是拼接符，会直接替换，所以实际是：
select * from `role` where name = '富贵' or name = '狗蛋'

而#{} 为啥就可以防止SQL注入呢？
select * from `role` where name = #{name}
因为#{}是占位符，所以实际是：
select * from `role` where name = '    富贵 or name = 狗蛋   '

```

## Mybatis缓存机制

一级缓存（默认开启）

- 一级缓存是SqlSession级别的（即同一个SqlSession对象,不同的对象没有效果），通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问  
- 使一级缓存失效的四种情况：  

```
1. 不同的SqlSession对应不同的一级缓存  
2. 同一个SqlSession但是查询条件不同
3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
4. 同一个SqlSession两次查询期间手动清空了缓存
```

二级缓存（手动开启）

- 二级缓存是mapper级别，是针对一个表的查结果的查询，可以共享给所有针对这张表的查询用户，也就是多个SqlSession共享的，其作用域是mapper的同一个namespace；此后若再次执行相同的查询语句，结果就会从缓存中获取  
- 使二级缓存失效的情况：两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效
- 二级缓存开启的条件

```
1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
2. 在映射文件中设置标签<cache />
3. 二级缓存必须在SqlSession关闭或提交之后有效 sqlSession.close()
4. 查询的数据所转换的实体类类型必须实现序列化的接口(Serializable接口)
```

![img](https://img-blog.csdn.net/20150726164148424?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)



## MyBatis 映射

如何将 sql 执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用 `<resultMap>` 标签，逐一定义列名和对象属性名之间的映射关系。
第二种是使用别名机制，就是数据库字段名和实体类的属性名不一致，只存在驼峰规则，会根据resultType 中的实体类参数 自动转换。

```xml
方式一
<select id="selectUser" resultType="constxiong.po.User" parameterType="constxiong.po.User">
    select * from user where id = #{id}
</select>
 
 
方式二
<select id="selectUserByResultMap" resultMap="userMap" parameterType="constxiong.po.User">
    select * from user where id = #{id}
</select>
<resultMap id="userMap" type="constxiong.po.User">
    <id property="id" column="id" />
    <result property="mc" column="name"/>
</resultMap>
 
```

**mybatis将sql结果 转换成 对应Java的原理是？**

根据解析得到 ResultMap 结合 sql 执行结果，通过 反射 创建对象，根据映射关系反射填充返回对象的属性

源码体现在 DefaultResultSetHandler 的 handleResultSets 方法

# 跨域问题

```Java
1.同源的定义
同源是指协议相同（http和https也属于不同协议）、域名相同、端口相同；如果中间有任何一个不相同，两个服务就属于不同源，这样一方去访问另外一方的资源时候就会产生跨域问题。

2.什么是跨域？
两个不同源的服务去访问对方的资源，就是所谓的跨域，而浏览器处于安全方面的考虑是不允许跨域请求的。
当发生跨域请求时候，请求是可以正常发送到对方服务器的，只是浏览器会根据 ”Access-Control-Allow-Origin" 来判断当前域名是否有访问权限，从而决定是否解析返回的数据信息。 其中简单的说就跨域是浏览器不会解析跨域请求返回的数据。

2.怎么解决跨域问题
简单请求（get post）
当浏览器发现跨域之后，就会向请求头信息里面自动添加一个Origin字段
GET /cors HTTP/1.1
Origin: http://api.bob.com    //Origin字段说明了本次请求来自哪个域（协议+域名+端口）

如果发现 Origin为指定的源 （白名单中），服务器会响应成功，并在响应头中多几个信息字段
// 该字段是必须的 该字段要么 * 要么 是请求时 Origin 中的值
Access-Control-Allow-Origin: http://api.bob.com  

// 该字段可选 是否允许发送cookie
Access-Control-Allow-Credentials: true

复杂请求（put）
复杂请求的时候会先发预检请求，它包含了一些额外的头部信息，如 Origin 和 Access-Control-Request-Method 等，用于告知服务器实际请求的方法和来源。服务器收到预检请求后，可以根据这些头部信息，进行验证和授权判断。如果服务器认可该跨域请求，将返回一个包含 Access-Control-Allow-Origin 等头部信息的响应，浏览器才会继续发送实际的跨域请求。

Cors实现跨域资源共享
CORS 实现跨域的原理是 修改请求header中的 Access-Control-Allow-Origin"。
可以使用WebMvcConfigure 配置的addCorsMappings方法配置CorsInterceptor拦截器，设置允许跨域的路径

@Configuration
public class CorsConfig implements WebMvcConfigurer {
​
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowCredentials(true)
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .maxAge(3600);
    }
​
}
```

![img](https://pic4.zhimg.com/80/v2-313794f8d240091b19f6321f6452e2bb_720w.webp)



# 设计模式

## 单例模式

[单例模式详细解析 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/160835278)

1.由于在系统内存中只存在一个对象，**因此可以节约系统资源，**对于一些需要频繁创建和销毁的对象单例模式无疑可以提高系统的性能

实现单例模式主要有以下几个关键点：

1. **构造函数设置为 private** ，这避免外部通过 new 创建实例；
2. 通过一个**静态方法或者枚举返回单例类对象**；
3. 考虑对象创建时的线程安全问题，确保单例类的对象**有且仅有一个**，尤其是在多线程环境下；
4. 确保单例类对象在反序列化时不会重新构建对象。
5. 考虑是否支持延迟加载；

![image-20230404224829930](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230404224829930.png)

```Java
//饿汉式单例测试，线程安全,因为在类加载期间就初始化好了。缺点不能延迟加载
public class Singleton{
    // 1、私有化构造⽅法
    private Singleton(){}
    // 2、定义⼀个静态变量指向⾃⼰类型
    private static final Singleton instance = new Singleton();
    // 3、对外提供⼀个公共的⽅法获取实例
    public static Singleton getInstance(){
        return instance;
    }
}


//懒汉式单例，线程不安全, 加锁之后性能有问题
public class Singleton{
    // 1、私有化构造⽅法
    private Singleton(){}
    // 2、定义⼀个静态变量指向⾃⼰类型
    private static Singleton instance;
    // 3、对外提供⼀个公共的⽅法获取实例
    public static synchronized Singleton getInstance(){
        // 判断为 null 的时候再创建对象
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}


//使用双重检查锁 DCL,实现安全的懒汉式单例
//饿汉式不能延时加载，懒汉式有性能问题，而双重检测方式既支持延迟加载、又支持高并发的单例实现方式。
public class Singleton{
    private Singleton(){}
    private volatile static Singleton instance;  //不加volatile会出现指令重排导致new Singleton 非原子操作
    public static Singleton getInstance(){
        // 第⼀重检查是否为 null
        if (instance == null){
            synchronized(Singleton.class){
                // 第⼆重检查是否为 null
                if (instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}


//使用内部类  实现安全的懒汉式单例，延迟加载
public class Singleton{
    private Singleton(){}
    public static Singleton getInstance(){
        return InnnerClass.INSTANCE;
    }
    // 定义静态内部类
    private static class InnerClass{
        private static final Singleton INSTANCE = new Singleton();
    }
}

//使用枚举实现单例  INSTANCE在枚举类底层的实现是被static final修饰，然后创建对象的实例是在静态代码块中创建
public enum Singleton{
    INSTANCE;
}
```

JDK中单例设计模式的体现

spring 的bean 默认是单例模式。

![image-20230405144648757](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405144648757.png)

spring 单例模式Bean 和原生写的单例模式的区别？

单例模式是指在一个JVM进程中仅有一个实例，而Spring单例是指一个Spring Bean容器(ApplicationContext)中仅有一个实例。所以在一个JVM进程中，如果有多个Spring容器，即使是单例bean，也一定会创建多个实例，代码示例如下：

```Java
//  第一个Spring Bean容器
ApplicationContext context_1 = new FileSystemXmlApplicationContext("classpath:/ApplicationContext.xml");
Person yiifaa_1 = context_1.getBean("yiifaa", Person.class);
//  第二个Spring Bean容器
ApplicationContext context_2 = new FileSystemXmlApplicationContext("classpath:/ApplicationContext.xml");
Person yiifaa_2 = context_2.getBean("yiifaa", Person.class);
//  这里绝对不会相等，因为创建了多个实例
System.out.println(yiifaa_1 == yiifaa_2);
```



## 工厂模式

### 1.简单工厂模式

指由一个工厂对象来创建实例，客户端不需要关注创建逻辑，只需要提供传入工厂的参数。

举例：pizza工厂一共生产三种类型的pizza：cheese，pepper，greak。通过工厂类（SimplePizzaFactory）来统一实例化这三种对象。

![img](https://img-blog.csdnimg.cn/20190609001610870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ExMzQyNzcy,size_16,color_FFFFFF,t_70)

```java 
工厂类的代码
public class SimplePizzaFactory {
       public Pizza CreatePizza(String ordertype) {
              Pizza pizza = null;
              if (ordertype.equals("cheese")) {
                     pizza = new CheesePizza();
              } else if (ordertype.equals("greek")) {
                     pizza = new GreekPizza();
              } else if (ordertype.equals("pepper")) {
                     pizza = new PepperPizza();
              }
              return pizza;
       }
}
```

**简单工厂存在的问题与解决方法：** 简单工厂模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了开闭原则。如何解决？可以定义一个创建对象的抽象方法并创建多个不同的工厂类实现该抽象方法，这样一旦需要增加新的功能，直接增加新的工厂类就可以。这种就是工厂方法模式



### 2.工厂方法模式

将生成具体产品的任务下发到具体的产品工厂。定义一个抽象工厂，其定义了产品的生产接口，但不负责具体的产品，将生产任务交给不同的派生类工厂。

![image-20230405153004439](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405153004439.png)

举例：pizza工厂（一开始的产地有伦敦和纽约），然后添加一个新的产地，如果用简单工厂模式的话，需要修改工厂代码的，增加一堆if else语句。

![img](https://img-blog.csdnimg.cn/20190609001610872.png)

```Java
OrderPizza中有个抽象的方法：
abstract Pizza createPizza();

两个工厂类继承OrderPizza并实现抽象方法：
public class LDOrderPizza extends OrderPizza {
       Pizza createPizza(String ordertype) {
              Pizza pizza = null;
              if (ordertype.equals("cheese")) {
                     pizza = new LDCheesePizza();
              } else if (ordertype.equals("pepper")) {
                     pizza = new LDPepperPizza();
              }
              return pizza;
       }
}
public class NYOrderPizza extends OrderPizza {
 
	Pizza createPizza(String ordertype) {
		Pizza pizza = null;
 
		if (ordertype.equals("cheese")) {
			pizza = new NYCheesePizza();
		} else if (ordertype.equals("pepper")) {
			pizza = new NYPepperPizza();
		}
		return pizza;
	}
}

通过不同的工厂会得到不同的实例化的对象，PizzaStroe的代码如下：
public class PizzaStroe {
       public static void main(String[] args) {
              OrderPizza mOrderPizza;
              mOrderPizza = new NYOrderPizza();
       }
}

解决了简单工厂模式的问题：增加一个新的pizza产地（北京），只要增加一个BJOrderPizza类：
public class BJOrderPizza extends OrderPizza {
       Pizza createPizza(String ordertype) {
              Pizza pizza = null;
              if (ordertype.equals("cheese")) {
                     pizza = new LDCheesePizza();
              } else if (ordertype.equals("pepper")) {
                     pizza = new LDPepperPizza();
              }
              return pizza;
       }
}
```

**工厂方法存在的问题与解决方法：**客户端需要创建类的具体的实例。简单来说就是用户要订纽约工厂的披萨，他必须去纽约工厂，想订伦敦工厂的披萨，必须去伦敦工厂。 当伦敦工厂和纽约工厂发生变化了，用户也要跟着变化，这无疑就增加了用户的操作复杂性。

为了解决这一问题，我们可以把工厂类抽象为接口，用户只需要去找默认的工厂提出自己的需求（传入参数），便能得到自己想要产品，而不用根据产品去寻找不同的工厂，方便用户操作。这也就是我们接下来要说的抽象工厂模式。



### **3.抽象工厂模式**

简单⼯⼚模式和⼯⼚⽅法模式不管⼯⼚怎么拆分抽象，都只是针对⼀类产品，如果要⽣成另⼀种产品，就⽐较难办了！抽象⼯⼚模式通过在 AbstarctFactory 中增加创建产品的接口，并在具体⼦⼯⼚中实现新加产品的创建。

**定义：**定义了一个接口用于创建相关或有依赖关系的对象族，而无需明确指定具体类。

**举例：**（我们依然举pizza工厂的例子，pizza工厂有两个：纽约工厂和伦敦工厂）。类图如下：

![img](https://img-blog.csdnimg.cn/20190609001610898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ExMzQyNzcy,size_16,color_FFFFFF,t_70)

```Java
工厂的接口：
public interface AbsFactory {
       Pizza CreatePizza(String ordertype) ;
}

工厂的实现：
public class LDFactory implements AbsFactory {
       @Override
       public Pizza CreatePizza(String ordertype) {
              Pizza pizza = null;
              if ("cheese".equals(ordertype)) {
                     pizza = new LDCheesePizza();
              } else if ("pepper".equals(ordertype)) {
                     pizza = new LDPepperPizza();
              }
              return pizza;
       }
}

PizzaStroe的代码如下：
public class PizzaStroe {
       public static void main(String[] args) {
              OrderPizza mOrderPizza;
              mOrderPizza = new OrderPizza("London");
       }
}
```

![image-20230405160305936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405160305936.png)

JDK中工厂模式的体现

![image-20230405192328692](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405192328692.png)

1. Calendar 抽象类的 getInstance ⽅法，调⽤ createCalendar ⽅法根据不同的地区参数创建不同的⽇历对象。
2. Spring中的BeanFactory 使用了简单工厂模式，根据传入一个唯一的标识来获得Bean对象。



## 策略模式

定义： 策略模式定义了一系列算法，并将每个算法封装起来，使他们可以相互替换，且算法的变化不会影响到使用算法的客户。

主要解决：在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

何时使用：一个系统有许多许多类，而区分它们的只是他们直接的行为。

如何解决：将这些算法封装成一个一个的类，任意地替换。

关键代码：实现同一个接口。

优点： 1、算法可以自由切换。 2、避免使用多重条件判断。 3、扩展性良好。

缺点： 1、策略类会增多。 2、所有策略类都需要对外暴露。


```Java
1、定义抽象策略角色
public interface Strategy {
	public int calc(int num1,int num2);
}

2、定义具体策略角色
public class AddStrategy implements Strategy {
	@Override
	public int calc(int num1, int num2) {
		// TODO Auto-generated method stub
		return num1 + num2;
	}
 
}
public class SubstractStrategy implements Strategy {
	@Override
	public int calc(int num1, int num2) {
		// TODO Auto-generated method stub
		return num1 - num2;
	}
}

3、环境角色
public class Environment {
	private Strategy strategy;
	public Environment(Strategy strategy) {
		this.strategy = strategy;
	}
	public int calculate(int a, int b) {
		return strategy.calc(a, b);
	}
}

4、测试
public class MainTest {
	public static void main(String[] args) {
		Environment environment=new Environment(new AddStrategy());
		int result=environment.calculate(20, 5);
		System.out.println(result);
		
		Environment environment1=new Environment(new SubstractStrategy());
		int result1=environment1.calculate(20, 5);
		System.out.println(result1);
	}
}
```

**工厂模式和策略模式的区别**

工厂模式关心创建实例，然后通过实例调用方法。
策略模式更关心实例的行为，在策略模式的类中对实例的方法进行封装，然后通过策略模式类调用方法





## 原型模式

![image-20230405194442289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405194442289.png)

![image-20230405195233457](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405195233457.png)

原型模式在Spring框架中的运用

![image-20230405195401987](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405195401987.png)



## 建造者模式

**当一个类的构造函数参数个数超过4个，而且这些参数有些是可选的参数，考虑使用构造者模式**

![image-20230405203324931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405203324931.png)

![image-20230405203300476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405203300476.png)

![image-20230405203727343](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405203727343.png)

![image-20230405210918036](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230405210918036.png)



## 适配器模式

在我们的应⽤程序中我们可能需要将两个不同接⼝的类来进⾏通信，在不修改这两个的前提下我们可能会需要某个中间件来完成这个衔接的过程。这个中间件就是适配器。所谓适配器模式就是将⼀个类的接⼝，转换成客户期望的另⼀个接⼝。它可以让原本两个不兼容的接⼝能够⽆缝完成对接。

**Target:** 定义 Client 真正需要使⽤的接⼝。

**Adaptee:** 其中定义了⼀个已经存在的接⼝，也是我们需要进⾏适配的接⼝。

**Adapter:** 对 Adaptee 和 Target 的接⼝进⾏适配，保证对 target 中接⼝的调⽤可以间接转换为对 Adaptee 中接⼝进⾏调⽤。

**类适配器**

![image-20230406214750957](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230406214750957.png)

**对象适配器**

![image-20230406215455281](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230406215455281.png)

适配器模式的运用

![image-20230406220143961](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230406220143961.png)

![image-20230406225527057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230406225527057.png)

![image-20230406225351615](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230406225351615.png)



## 代理模式

![image-20230407200103103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230407200103103.png)

动态代理类图示意： JDKProxy代理

![image-20230407203312038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230407203312038.png)

代理模式应用：Spring事务的方法调用，是用代理类调用。



# BigDecimal 解决浮点数精度

问题：不是所有的小数都可以使用二进制来表示。如0.1， `0.1 + 0.1 + 0.1 == 0.3`将会返回false，

BigDecimal的解决方案就是，不使用二进制，而是使用十进制（[BigInteger](https://so.csdn.net/so/search?q=BigInteger&spm=1001.2101.3001.7020)）+小数点位置(scale)来表示小数

```Java
    public static void main(String[] args) {
        BigDecimal bd = new BigDecimal("100.001");
        System.out.println(bd.scale());
        System.out.println(bd.unscaledValue());
    }
//输出：
 3
100001  也就是100.001 = 100001 * 0.1^3
```



**BigDecimal注意事项**：使用它的BigDecimal(String)构造器创建对象才有意义。其他的如BigDecimal b = new BigDecimal(1)这种，还是会发生精度丢失的问题。

另外，BigDecimal所创建的是对象，我们不能使用传统的+、-、*、/等算术运算符直接对其对象进行数学运算，而必须调用其相对应的方法。方法中的参数也必须是BigDecimal的对象。





# 负载均衡常见的算法

### 随机法

**随机法** 是最简单粗暴的负载均衡算法。

如果没有配置权重的话，所有的服务器被访问到的概率都是相同的。如果配置权重的话，权重越高的服务器被访问的概率就越大。

### 轮询法

轮询法是挨个轮询服务器处理，也可以设置权重。

### 一致性 Hash 法

相同参数的请求总是发到同一台服务器处理，比如同个 IP 的请求。



# TOP K算法



从arr[1, n]这n个数中，找出最大的k个数，这就是经典的TopK问题。

1.全局排序，时间复杂度：O(n*lg(n))

2.局部排序，像局部冒泡排序，时间复杂度：O(n*k)。 注意冒泡排序的平均时间复杂度为O(n2)

3.堆排序，时间复杂度：O(n*lg(k))。
先用构造小顶堆，然后从K+1个元素开始扫描，和堆顶进行比较，如果大于堆顶，则替换堆顶的元素，并调整堆。确保堆内的k个元素，总是当前最大的k个元素。

![img](https://pic3.zhimg.com/80/v2-f7aec1e403b784753102a0a11626658a_720w.webp)

# **git**

git本地有很多commit提交，如何合成一个？

git rebase

```
# 查看前10个commit
git log -10
# 将4个commit压缩成一个commit
git rebase -i HEAD~4	
# add已经跟踪的文件
git add -u
# 提交
git commit -m "修改信息"
# 强制push以替换远程仓的commitID
git push --force
```



# 常见场景题目

## 海量数据排序

使用有限的内存对海量数据进行排序

1.分治法

假设 100 亿个数据都是 int 类型的数字

1 个 int 类型占 4 个字节（byte，B）

1 B = 8 位（bit）

1024 B（1024 B = 1KB） = 8 * 1024 bit

1024 * 1024 KB（1024KB = 1MB）= 1024 * 8 * 1024 bit

100 亿 int 型数字就是 100 亿 x 4B = 400 亿 B = 38146.97265625 MB 约等于 37.25GB

100 亿个 int 型数字大概占 37 个 G，2G 内存显然一次性是装不下的。

最常见的思路，拆分成一个一个的小文件来处理呗，最终再合并成一个排好序的大文件。

典型的分治法

1）把这个 37 GB 的大文件，用哈希或者直接平均分成若个小文件（比如 1000 个，每个小文件平均 38 MB 左右）

2）拆分完了之后，得到 1000 个 30 多 MB 的小文件，那么就可以放进内存里排序了，可以用快速排序，归并排序，堆排序等等

3）1000 个小文件内部排好序之后，就要把这些内部有序的小文件，合并成一个大的文件，可以用堆排序来做 1000 路合并的操作（假设是从小到大排序，用小顶堆）：

- 首先遍历 1000 个文件，每个文件里面取第一个数字，组成 `(数字, 文件号)` 这样的组合加入到堆里，遍历完后堆里有 1000 个 `(数字，文件号)` 这样的元素

- 然后不断从堆顶 pop 元素追加到结果集，每 pop 一个元素，就根据它的文件号去对应的文件里，补虫一个元素进入堆中，直到那个文件中的元素被拿完

- 按照上面的操作，直到堆被取空了，此时最终结果文件里的全部数字就是有序的了

  

2.位图法（Bitmap）

位图法的基本思想就是利用一位（bit）代表一个数字，例如第 3 位上为 1，则说明 3 这个数字出现过，若为0，则说明 3 这个数字没有出现过。

也就是说，**通过位图法，只需要 1M 的空间，我们就可以处理 800 多万级别的数据**

Java 中没有 bit 这样的基本数据类型，最小数据类型是 `byte`，我们可以用 **byte 数组**来实现这个位图法

byte 数组上的每一个元素都是 byte 类型，一个 byte 等于 8 个 bit，我们可以把 10 进制的 byte 用二进制的 bit 来表示，如下图：

![img](https://pic2.zhimg.com/80/v2-76e4695794ec29f3eef185a6d6b5e969_720w.webp)

这样，byte 数组中的一个元素就能表示 8 个数字是否出现过，比如 byte[0] 可以表示 0 ~ 7 是否出现过，byte[1] 可以表示 8 ~ 15 是否出现过.....

全部处理完之后，我们**从前往后遍历一遍 byte 数组**就能获取到有序数据了，时间复杂度为 O(N)

java.util 封装了 `BitSet` 这样一个类，是位图法的典型实现。



## 智力题

[天平与假币的算法思考_称硬币和找毒药问题分别用到什么数学知识-CSDN博客](https://blog.csdn.net/u014028063/article/details/82388101)
