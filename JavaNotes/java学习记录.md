基本变量 采用和C语言差不多的方式，java为强类型语言
**基础注意的点**
# 数据类型 变量名 = 值；
```
整数型： byte short int long        * 浮点：float double  
字符：char  
布尔：boolean  
引用类型：字符串：String 类 接口 数组  
 注意：  
int num;会报错 必须初始化  
long l = 768678689868L; 最后要加L  
加号 math计算 字符拼接 强制转换  
字符相加 计算ascll码值  
char c1 = 'a';  
char c2 = 'b';  
System.out.println(c1+c2);
``` 

**java采用类进行书写，和python一样，可以定义类，但class使用为主要，创建新文件也以类开始**

```java
public class OPT_three_fun {  
  
//    3大方法：封装，继承，多态  
    private String name;  
    private int age;  
//在Java编程语言中，构造器（也称为构造方法或构造函数）是一种特殊类型的方法  
// ，它在创建对象时自动调用。构造器的主要作用是初始化新创建的对象 。  
// 构造器的名称必须与类名完全相同，并且它们没有返回类型，甚至连void都不返回。  
    public OPT_three_fun() {  
    } //这是一个无参构造器  
  
    public OPT_three_fun(String name,int age){  
        this.name = name;  
        this.age = age;  
    } //这是一个有参构造器  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
}
```
# 要进行语法运行，要有一个方法

## 打印输出
```java
public class class {  
  public static void main(){
	  //输出要调用方法 快捷键 sout ,println型输出会暂用这一行余下的所有空格  print则正常连接
	  System.out.println("Hi");
	  System.out.print("不占一行");
	  
  }
 
}
``` 

## 输入又叫扫描 
[[#类|引到后面类的具体学习]]
```java
//需要一个方法的创建
Scanner sc = new Scanner(System.in);
int a = sc.nextInt();
```
![[Pasted image 20260530210149.png|545]]
![[Pasted image 20260819141924.png]]

# 数组和字符串

```java
  
public class ArrayPra {  
  
    public static void main() {  
  
        //    数组初始化  
        int arr1[] = new int[]{1,2,3,4,5};  
        int arr2[] = new int[7];  
        String str1[]  = new String[]{"hhh","jfs","788",999};  
        int[] arr3 ={2,3,4,5,5};  
//    长度是固定的对元素进行修改  不能进行增删---创建新的数组  
//    索引 不能超出范围 越界异常  
//    遍历 数组名.fori enter  
//    导入Arrays包 用回车  
        System.out.println(str1[2]);  
        for (int i = 0; i < arr1.length; i++) {  
            System.out.println(arr1[i]);  
        };  
//        以数组的形式输出  
        System.out.println(Arrays.toString(arr1));  
    }  
  
  
}
```
## 数组
的初始化：
空白数组：数据类型 数组名 [ ] = new 数据类型[ ]{};
定义数值长度：int group [ ] = new int [10];
初始化数据：String i[] = new String []{"hihao","jjj","lll"};
初始化数据 只可以和定义数据类型一致
或者直接定义数据：int j[] ={1,3,4,2}


数组长度，数组名.length
区分：c--数组名.length  ; py:--len(数组名)

## 字符串

大写S String name = "小米";

# 循环和遍历 
和c 差不多
```java
int [] lll = {9,8,77,7};
for(int i = 0;i<lll.length;i++){
	System.out.println(arr1[i]);  
}
```



# 关系运算符

![[Pasted image 20260530204605.png]]
# 三元运算符
![[Pasted image 20260530205557.png]]

# 类


**类**是面向对象编程中的一个基本概念，它是对现实世界或思维世界中的实体在计算机中的反映。类将数据以及这些数据上的操作封装在一起，是一种抽象的数据类型。
类的定义通常包括属性和方法。例如，在Java中，可以这样定义一个类：


```java
public class LISTSHOW {  
    //构造方法，构造器  
	public LISTSHOW(){
		System.out.println("gzq");
	};
    //成员变量（对象）  
    public String name = "黎明";  
    public int age = 30;
    //成员方法  
    public void like(){  
        System.out.println("I like "+name+"His age is "+ age);  
    };
    
    public void like2(String name,int age){  
	    System.out.println("I like "+name+"His age is "+ age);  
	}
	//这两个方法有什么区别呢，来看原图
}
```

![[Pasted image 20260530233645.png|708]]
仔细观察发现这两个name的颜色并不一样，这就要讲到局部变量了，第二个是like2是有要求参数值的，这里的这个name其实是 这个方法 需要接收到的参数的值。
可以加上**this.**   ( 点要加) 表示在这个类中
![[Pasted image 20260531002920.png]]
```java
public class LISTSHOWTEST  extends LISTSHOW{  
    static void main() {  
        LISTSHOW show = new LISTSHOW();  
        show.like2("jey",33);  
    }  
}
```
![[Pasted image 20260531002939.png]]
可以看到这里我们设置了参数，但是它输出的依然是黎明


**方法(method)：就是完成特定功能的代码块 对象的行为**
这些代码都是用一对大括号括起来的，所以我们说，方法就是完成特定功能的代码块。
了解一下， 这个public是什么了，一种访问修饰符，用于控制这个对象或者方法，它的影响范围，它能涉及到的地方，他会涉及影响什么了--》 [[#类的3大特点]]

类的第一大使用就是
# 实例化
创建一个实例，可以在别的类里调用不同的类

回看之前的**构造器（也称为构造方法或构造函数）**
其实他的作用是在创建实例时才发动的
前在那个构造器里面写了System.out.println("gzq");
这时，当我们创建这个类的实例时，就会去触发这个构造性里面的内容，这次会打印gzq，
如果我们什么都不写的话，那么就什么都不会发生

```java
public class LISTSHOWTEST  {  
    static void main() {  
    // 类名 实例名(这个你自己随便取) = new 类名();
        LISTSHOW show = new LISTSHOW(); 
        //通过实例来调用方法 
        show.like();  
    }  
}
```

这里
# 类的3大特点 
封装 继承 多态
在PYTHON中类的继承可以在子类初始化父类继承过来了参数的值
但是在Java中就没有，而是需要手动初始化





## 封装
private + 构造器 + get/set 方法（Alt + insert）

这作为父类（超类） ，先记着
```java
public class LISTSHOW {  
  
//    3大方法：封装，继承，多态  
//可以看到这里public变成private，也就是私人的，
//从此，这一个变量它就无法被除了它所在的类而使用了
//为了直观了解，可以看继承
    private String name = "jjj";  
    private int age = 10;  
//在Java编程语言中，构造器（也称为构造方法或构造函数）是一种特殊类型的方法  
// ，它在创建对象时自动调用。构造器的主要作用是初始化新创建的对象 。  
// 构造器的名称必须与类名完全相同，并且它们没有返回类型，甚至连void都不返回。  
    public LISTSHOW() {  
    } //这是一个无参构造器  

    public OPT_three_fun(String name,int age){  
        this.name = name;  
        this.age = age;  
    } //这是一个有参构造器  
  
  
  //自己写的方法
	public void like(){  
	    System.out.println("I like "+name+"His age is "+ age);  
	}  
	public void like2(String name,int age){  
	    System.out.println("I like "+this.name+"His age is "+ this.age);  
	}
  
  
  //这些方法就是在别的类里可以调用private类的get/set方法
	public String getName() {  
	    return name;  
	}  
  
	public void setName(String name) {  
	    this.name = name;  
	}  
  
	public int getAge() {  
	    return age;  
	}  
  
	public void setAge(int age) {  
	    this.age = age;  
	}
}
```


## 继承

**extends：在 Java 中，**继承**是面向对象编程的一个重要特性，通过继承，子类可以继承父类的属性和方法，从而实现代码的复用和扩展。继承使用 _extends_ 关键字来实现。**
父类（超类）见上
```java
public class LISTSHOWTEST  extends LISTSHOW{  
    static void main() {  
		int age = 20;
        LISTSHOW show = new LISTSHOW();  
        show.like();  
        public void num(){
	        int age = 30
	        System.out.println(age);//30
	        System.out.println(this.age);//20
	        System.out.println(super.age);//10
        }
    }  
}
```
继承的特性在我们创建子类的实例的时候才能体现
在子类方法中访问一个变量 
1. 子类局部范围找 
2. 子类成员范围找 
3. 父类成员范围找 
4. 如果都没有就报错(不考虑父亲的父亲…)
总结：**就近原则**
和this.类似，还有super.
即调用其父类的对象
```java
public class text {  
    static void main() {  
        LISTSHOWTEST show = new LISTSHOWTEST();  
        show.num()
        // print: 30 20 10 
    }  
  
}
```
![[Pasted image 20260531011049.png]]

**_implements：在 Java 中，_implements_ 关键字用于让一个类实现接口，从而必须提供接口中声明的所有方法的具体实现。**

**接口/抽象方法/方法重写**

```java
public interface ListImg {  
    public  int height = 111;  
    public abstract void sum ();  
    public abstract int age();  
}
```
实际情况
![[Pasted image 20260531005024.png]]
可以发现有几个灰色的名词。当然不是指这个接口名和这个变量或者方法的名字，还是前面修饰的。
**abstract叫抽象方法：**
比如交通工具作为父类，而自行车和高铁作为子类的话，自行车和高铁都具有驾驶的这个方法，但是他们两个的方法并不是一样的，方法的具体内容也是不一样的，一个是脚蹬的，一个是在轨道上面跑的。它们都具有方法驾驶，但是无法用同一个父类里的一个方法来共同表示，这种都具有相同概念，但又具体区分的叫做抽象方法。
把抽象方法写在接口里，然后在我们的类中去调用它
这就要用到
 
### 方法重写

**重写** 重写是面向对象编程中的概念，通常发生在**子类与父类**之间。子类保留父类方法的**方法名和参数列表**，但**重新实现方法内容**，以覆盖父类的行为。

方法重写是为了什么，就是字面意识，新构建一个子类中的方法（和父类重名的方法),规定这个方法怎么做。以后通过实例调用方法就采用这个新的方法，当然方法重构只是一种思想，你也可以不这么做（就会采用父类中的原来的方法），我们这么做也只是为了实际编程需要来进行。

***是人使用语法，而不是语法使用人***

I.  对于继承的方法重写
Alt+insert
![[Pasted image 20260605133833.png|161]]
![[Pasted image 20260605134027.png|314]]
这里测试的时候把父类里的name age改了



II. 对于接口来所的方法重写

方法重写对于接口来说是必须的，因为接口中只可以都是无返回值的抽象方法，所以在imp接口的类中必须重写方法
![[Pasted image 20260531005759.png|579]]
在载入我们的接口后，放在最后面，使用ALT+enter快捷方式重写方法（当然，你也可以自己敲）
  ![[Pasted image 20260531010514.png|543]]
  
  这里重写好了，具体写什么看实际情况


## 多态

在代码的维护中，会有多次迭代，而多态就是为了方便维护
以父类或者接口作为实例可访问的类型 
Parent p = new son();
p.method();
对于右边的子类具体是什么，不确定的，但是都会去使用同一个方法method
父类或者接口只是一个空壳子，还是要在子类里重写方法

```java
//接口/(父类)
public interface People {  
    void eat();  
    void drink();  
}
```

```java
//子类
public class teacher implements People{  
  
    @Override  
    public void eat() {  
        System.out.println("eat meet");  
    }  
  
    @Override  
    public void drink() {  
        System.out.println("drink milk");  
    }  
}
```

```java
//方法类
public class live  {  
    public void live(People people){  
        people.eat();  
        people.drink();  
    }  
  
}
```

```java
//实现类
public class text {  
    static void main() {
	    //用方法类的写法  
        live live = new live();  
        People p1 = new teacher();  
        live.live(new teacher()); 
        People p1 = new teacher();
        
        
        //不用方法类的写法
        p1.eat();
        p1.drink(); 
  
    }  
}
```


## 匿名内部类与Lambda表达式

匿名内部类就是把实现接口实例和额外创建一个类文件和重写方法放在一块了
Lambda表达式就是匿名内部类只用重写一个方法时的简写形式

**匿名内部类 = 没有名字的临时一次性类**

- 它是一个**局部类**，只能用一次，用完就丢；
- 必须**继承一个父类** 或 **实现一个接口**；
- 作用：**简化代码**，不用单独写一个类文件 / 类定义。

例子：普通写法
一个接口
```java
public interface cloth {  
    void dress();  
}
```

要创建一个类
```java
class dress implements cloth { 
@Override 
public void dress() { 
	System.out.println("I like dress"); 
	} 
}

```

实现时
```java
public class text {  
    static void main() {  
        cloth d = new dress();
    }  
}

```

**匿名内部类
```java
public class text {  
    static void main() {  
        cloth d = new cloth(){ //这里
	        @Override 
			public void dress() { 
				System.out.println("I like dress"); 
			} 
        };
        d.dress();
    }  
}

```
Lambda表达式
Lambda 只认「单方法接口」，别的一概不认！
```java
public class text {  
    static void main() {  
        cloth d = ()-> System.out.println("I like dress"); //这里 
        d.dress();
    }  
}
```


# 异常
**什么是异常：**

**异常** 就是用来描述代码中出现的问题

异常：就是程序出现了不正常的情况。程序在执行过程中，出现的非正常的情况，最终会导致 JVM 的非正常停止。 **注意**：语法错误不算在异常体系中。 
![[Pasted image 20260826153933.png]]
![[Pasted image 20260826154043.png]]
##  异常的处理
```java
try{
	可能出现异常情况的代码,代码运行时就正常执行try的内容
}catch(异常类名 变量名){
	//如果出现了对应异常，就执行开启的内容，变量名一般为e
}final{
	//最终一定会执行的代码
}
```
`finally` **无论是否发生异常，都必定执行**（通常用于释放资源，如关闭文件流）
```java
try {
    System.out.print("请输入价格（0~100）：");
    price = Double.parseDouble(scanner.nextLine().trim());
    if (price < 0 || price > 100) {
        throw new PriceOutOfBoundsException
        ("价格必须在 0~100 之间");//<- e.getMessage()
    }
    break;
} catch (NumberFormatException e) {
    System.out.println("请输入有效的数字！");
} catch (PriceOutOfBoundsException e) {
    System.out.println("价格错误：" + e.getMessage());
} finally {
    // 可选
}
```
**throw**  : 抛出异常
抛出后会执行捕获（catch）这个异常的相关代码。
**throws**：[[#throws IOException|实例]]
**throws**关键字用于方法声明中，用来指明该方法可能会抛出的异常类型。当一个方法可能会产生某种异常，但并不希望在当前方法中处理这个异常，而是希望调用这个方法的上层代码来处理时，就会在方法声明时使用**throws**。这样，任何调用该方法的代码都必须考虑到异常处理的问题。**throws**后面可以跟多个异常类型，用逗号分隔。
**throws**表示这个方法可能会抛出异常，需要调用者来处理或者继续声明抛出，而**throw**则是在方法内部当检测到特定条件时，主动抛出一个异常


其实我们也不一定要在这个try里面进行抛出异常。因为Java它本身的API底层也有很多抛出，所以我们除了是自定义的异常，你直接在catch里面输出 `System.out.println("操作失败：" + e.getMessage());`类似的话就行了
## 异常自定义
一般为在同一个软件包下新建异常类
会有一个新的Java文件，然后里面默认是一个public的类
然后在main Class里面就可以去直接调用它了。


# 集合
集合与数组类似，也是一种**容器**，用于装数据。
假如我们在制作一个用户注册或者图书管理系统，哦，我们肯定要对每一个用户或者每一本书进行存储，那这时一般的思路是定义一个类user/book。
那对于这每一个类，我们其实也可以把它们存储起来，我们把提供这些存储的东西叫集合。

集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。
集合非常适合做元素个数不确定，且要进行增删操作的业务场景。 集合还停工了许多丰富好用的功能，而数组的功能比较单一。
![[Pasted image 20260826155804.png]]
**Collection单列集合，每个元素（数据）只包含一个值。**
**Map双列集合，每个元素包含两个值（键值对）。**

![[Pasted image 20260826155857.png]]
举个例子：
```java
//像之前讲的一样，创建一个类
public class Books {  
    private  String bookid = null;  
    private  String name = null;  
    private  int time = 0;  
    private  double price = 0;  
  
    public Books(String bookid,String name,int time,double price) {  
        this.bookid = bookid;  
        this.name= name;  
        this.price=price;  
        this.time =time;  
  
    }  
  
    public get,set(){} 
}

public class Main { 
	private static final List<Books> bookList = 
	new ArrayList<>(); 
    static void main() {
	    id,name,time,price = 什么什么。。。
	    Books book = new Books(id,name,time,price);  
		bookList.add((book));
    }

```
因为main方法是一个静态方法，所以我们创建一个全局的array list集合,用它来存储我们的每一本书。
集合里面有很多方法，包括一些删除，长度，判空，添加，移动，包含什么的
## prase,读取数据
**1.“parse”到底是什么意思？**
在编程里，**`parse`（解析）** 翻译成大白话就是：**“把字符串（String）翻译成其他类型”**。
---
一般读取字符串我们可以用scanner的nextLine()
但是对于整数或者浮点数之类的
`scanner.nextInt();`
`scanner.nextDouble();`
这些方法，如果我们在输入完之后进行回车，那这个回车占位符并不会被读掉，假如我们要连续读取，要读取int什么的，程序就会一直接到保留的那个回车，有可能会导致程序卡死。
但是用nextLine就不会有这种顾虑。
---
而且假如我们规定一个退出的标志，叫brake，用户输入这个就会退出。
那我们怎么样同时去接收可能的整数或者字符串呢。
我们就可以先用next line去接收字符串，把它存在第一个变量里，先对这个字符串判断是不是break。
然后再去用第二个变量就是，parse过的第一个变量，此时它变成了整数，去进行后续的数字判断

  ```java
  int time = Integer.parseInt( scanner.nextLine().trim() );
  ```
你可以把这个方法拆开读：**“Integer 类的 parse 方法，把括号里的东西解析成一个 int 整数”**。

## 泛型
可以发现刚刚在定义存储书籍的集合的时候，我们运用了\<Books\>
这其实是一种泛型，通过泛型，我们可以统一数据类型
泛型可以在很多地方进行定义: 
类后面———— 泛型类 
方法申明上——泛型方法 
接口后面 ———泛型接口
1.泛型中不能写基本数据类型int,double,可以String，Integer，Double
2.指定泛型的具体类型后，传递数据时，可以传入该类类型或者其子类类型
3.如果不写泛型，类型默认是Object

## 第一部分：泛型参数 `T`（类型占位符）

`T` 并不是什么神秘字母，它只是**一个形参**（就像方法里的 `int a`）。只不过它占的位置是**类型**，而不是值。

**定义时（写代码）：** 你不知道用户将来要传什么类型，先用 `T` 占坑。  
**使用时（实例化）：** 用户传入具体类型（如 `String`），`T` 就变成 `String`。

**语法约定（非强制，但行业规范）：**
`T` → Type（类型）
`E` → Element（集合元素，如 List）
`K` / `V` → Key / Value（映射）
`?` → 未知类型（通配符）
```java
// T 是类型形参
public class Box<T> {
    private T content;

    public void setContent(T content) {
        this.content = content;
    }

    public T getContent() {
        return content;
    }

    // 泛型方法：这里的 T 和类上的 T 是两码事，但写法一样
    public static <T> T getMiddle(T[] array) {
        return array[array.length / 2];
    }
}

// 使用时，T 被实例化为 String
Box<String> stringBox = new Box<>();
stringBox.setContent("Hello");
//也可以是integer
Box<Integer> box = new Box<>(); 
box.setContent(123); // 完美运行！
//又可以是字符串，又可以是integer，非常的灵活
String result = stringBox.getContent(); // 注意：这里不需要强制转型！

Box<Integer> intBox = new Box<>();
intBox.setContent(123); // 自动装箱
```
![[Pasted image 20260826163516.png]]
```java
// 1. 无界通配符：只读，不能写（除了 null）
public void printList(List<?> list) {
    for (Object obj : list) { // 只能当 Object 取
        System.out.println(obj);
    }
    // list.add("hello"); // 报错！因为你不知道 ? 是什么类型，无法安全添加
    list.add(null); // 唯一能加的
}

// 2. 上界通配符 <? extends Number>：获取安全，插入受限
public double sum(List<? extends Number> numbers) {
    double sum = 0.0;
    for (Number num : numbers) { // 可以安全地当做 Number 取出来
        sum += num.doubleValue();
    }
    // numbers.add(1); // 报错！因为可能是 List<Double>，你不能塞 Integer
    return sum;
}

// 3. 下界通配符 <? super Integer>：插入安全，获取受限
public void addIntegers(List<? super Integer> list) {
    list.add(1);   // 可以！因为 Integer 一定能转成 ? 代表的父类
    list.add(2);
    // Integer i = list.get(0); // 报错！因为可能是 List<Object>，取出的是 Object，不安全
    Object obj = list.get(0); // 只能当 Object 取
}
```
## 集合的遍历
![[Pasted image 20260829153103.png]]

# 装箱
刚刚学习完集合，但是其实集合有一个很大的问题:集合，比如Arraylist这类集合，它里面只能装对象，不能装基本类型。只能装对象（`Integer`、`Double`）。


**装箱（Boxing）** 的作用就是：把基本类型（`int`、`double`）变成**对象**（`Integer`、`Double`）。—— **“变废为宝”** 
**拆箱（Unboxing）**：把**对象**变回**基本类型**。—— **“现出原形”**
Java 是“万物皆对象”的语言，但 **`int`、`double`、`boolean` 不是对象**，它们是纯数值，放在栈内存里，干活快，但没有“功能”。

**包装类的两大核心价值：**
1. **为了泛型（你刚学的）**：`ArrayList` 这类集合，**只能装对象**，不能装基本类型。
错：`ArrayList<int> list = new ArrayList<>();`（编译报错！）
对：`ArrayList<Integer> list = new ArrayList<>();`（正因为有包装类，泛型才能用！）
2. **为了表达“空值（null）”**：数据库里某个年龄字段可能是“未知”，用 `int age` 没法表示“未知”（只能给个 -1 这种脏数据）。用 `Integer age = null;` 完美解决。

**代码实现**
 ```java
 List<Integer> scores = new ArrayList<>();
scores.add(100);         // 这里发生了自动装箱（int 100 -> Integer）
scores.add(200);

int scaore = scores.get(0); // 这里发生了自动拆箱（Integer -> int）
System.out.println(firstScore); // 输出 100  
 ```

# 迭代器

在C语言中，我们通常是采用for循环或者while来变异我们的数组或者链表
但是在面向对象的语言中，我们的存储方式通常是经过高度包装的、抽象过的。嗯，它的底层可能是各种不同的存储，比如链表数或者数组等等。
如果每次我便利的时候都要写一套各自对应的便利方式，就非常麻烦，所以我们进行一个便利的统一包装，把它叫做迭代器。
使用迭代器可以对底层不同数据结构的存储进行遍历。
---
回看我们之前的存储书籍的例子
```java
Iterator<Books> iterator = bookList.iterator();  
Books targetBook = null;  
while(iterator.hasNext()){  
    if(iterator.next().getBookid().equals("目标书籍")){  
        targetBook = iterator.next();  
        break;  
    }  
}
```
我们可以定义迭代器对我们的那个Arraylist的集合进行遍历迭代
迭代器初始指向我们数据结构的头节点的前面一个。
所以我们用hasnext进行判断，而此时的next正好也就是第一个
后续同理

---
Java 里的“流（Stream）”其实指两个完全不同的东西：

1. **IO 流（java.io）**：**数据的搬运工**。用来从硬盘、网络、键盘读取/写入数据（字节或字符）。
2. **Stream 流（java.util.stream）**：**数据的加工流水线**。JDK 1.8 引入，用来对集合（List/Set）进行链式数据处理（过滤、排序、映射）。

---

# IO 流（数据传输）—— 重点中的重点

 **它是干什么用的？**
**读写数据**。比如：上传文件、下载图片、读取配置文件、网络通信传输二进制数据。

##  核心分类（四大抽象基类，必须刻在脑子里）

| 分类维度 | 字节流（8位，万能，处理任何文件） | 字符流（16位，专门处理文本文件） |
| :--- | :--- | :--- |
| **读（输入）** | `InputStream` | `Reader` |
| **写（输出）** | `OutputStream` | `Writer` |

**记忆口诀**：操作图片/视频/音频用 `字节流`（后缀是 `Stream`）；操作纯文本（.txt/.java）用 `字符流`（后缀是 `Reader/Writer`）。

## 必须掌握的基础语法（读文件 + 写文件）

这是最常用、最规范的读写模板（**必须使用 `try-with-resources` 自动关闭，防止内存泄漏**）：

**场景一：用字节流复制图片（FileInputStream + FileOutputStream）**
```java
// 细节1：try() 里创建流，执行完自动调用 close()，不用再写 finally
try (FileInputStream fis = new FileInputStream("source.png");
     FileOutputStream fos = new FileOutputStream("target.png")) {
    
    // 细节2：千万别用 int b = fis.read() 单字节读（极慢）
    // 必须用字节数组缓冲区（1024的整数倍）
    byte[] buffer = new byte[1024];
    int len; // 记录每次实际读取的字节数
    while ((len = fis.read(buffer)) != -1) {
        fos.write(buffer, 0, len); // 细节3：写入时一定要写 0 到 len，不要写 buffer.length
    }
    // 细节4：字节流底层有缓冲区，flush() 可以强制刷出，但 close() 会自动 flush，不用刻意写
} catch (IOException e) {
    e.printStackTrace();
}
```

**场景二：用字符流按行读取文本（BufferedReader + FileReader）**
```java
// 细节5：BufferedReader 是“包装流”（装饰器），比 FileReader 性能高得多，必须用！
try (BufferedReader br = new BufferedReader(new FileReader("note.txt"))) {
    String line;
    while ((line = br.readLine()) != null) { // 细节6：readLine() 不包含换行符
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```
==如果是读取字符,不能定义byte[]了，要定义char[]来实现批量的读取==
## 代码
## throws IOException
```java
public class FileDemo {  
    public static void main(String[] args) throws IOException {  
  
        File files = new File("src//com//TextFILE");  
        File[] filess = files.listFiles();  
        if (filess != null) {  
            for (File one_file : filess) {  
                System.out.println("file.exists() = " +one_file.exists());  
            }  
        }  
        //字节流  
        FileOutputStream outputStream = new FileOutputStream("src//com//TextFILE//111.txt",true);  
        outputStream.write("hello".getBytes());  
  
        FileInputStream inputStream = new FileInputStream("src//com//TextFILE//222.txt");  
        byte[] bytes = new byte[1024];  
        int len ;  
        while(( len = inputStream.read(bytes)) != -1){  
            outputStream.write(bytes,0,len);  
        }  
        inputStream.close();  
        outputStream.close();  
  
  
        //字符流  
        File file1 = new File("src//com//text1.txt");  
        File file2 = new File("src//com//text2.txt");  
        try (FileWriter fileWriter = new FileWriter("src//com//text1.txt",true);  
             FileReader fileReader = new FileReader(file2)){  
  
            boolean exists = file1.exists();  
            System.out.println("fil1 exists = " + exists);  
  
            fileWriter.write("你好");  
  
            int length ;  
            char[] chars = new char[100];  
            while((length = fileReader.read(chars))!= -1){  
                fileWriter.write(chars,0,length);  
            }  
        }catch (IOException e ){  
//            throw new IOException("请再试一次");  
            //嗯，其实没必要自己写一个抛出，因为系统它Java里面本身也有个抛出  
            System.out.println("操作失败：" + e.getMessage());  
        }  
  
//把 `close()` 去掉，让 try-with-resources 会在最后统一关闭，更加规范
//        fileWriter.close();  
//        fileReader.close();  
```
## 进阶一点的
```java

		try (BufferedWriter writer = new BufferedWriter(
		         new OutputStreamWriter(new FileOutputStream(file1, true), StandardCharsets.UTF_8));
		     BufferedReader reader = new BufferedReader(
		         new InputStreamReader(new FileInputStream(file2), StandardCharsets.UTF_8))) {
		
		    writer.write("你好");
		    writer.newLine(); // 写入换行
		
		    String line;
		    while ((line = reader.readLine()) != null) {
		        writer.write(line);
		        writer.newLine(); // 保持原格式
		    }
		}
  //StandardCharsets.UTF_8规范格式标准
  //FileInputStream，FileOutputStream最内层使用字节流
  //new OutputStreamWriter/InputStreamReader(..., StandardCharsets.UTF_8)
  //中间层使字节和字符相互转换，最后存到文件里，统一用字节0 1
  //BufferedWriter/BufferedReader 最外层采用- **职责**：**缓冲流**。它自带一个 8KB 的大内存缓冲区（类似一个大水桶）。
    
- **作用**：你写一个字符，它不立刻存进硬盘（太慢），而是先扔进水桶，等水桶快满了再一次性倒进硬盘。极大提升读写速度。
  
  
    }  
}
```
`StandardCharsets.UTF_8`规范格式标准
`FileInputStream，FileOutputStream`最内层使用字节流
`new OutputStreamWriter/InputStreamReader(..., StandardCharsets.UTF_8)`
中间层使字节和字符相互转换，最后存到文件里，统一用字节0 1
`BufferedWriter/BufferedReader `最外层采用
**职责**：**缓冲流**。它自带一个 8KB 的大内存缓冲区（类似一个大水桶）。
**作用**：你写一个字符，它不立刻存进硬盘（太慢），而是先扔进水桶，等水桶快满了再一次性倒进硬盘。极大提升读写速度。

## 字符流乱码问题：
`FileReader` 默认使用系统编码（中文 Windows 是 GBK），读 UTF-8 的文件必乱码。**解决方案**：必须用 `InputStreamReader` 指定编码，`new InputStreamReader(new FileInputStream("a.txt"), StandardCharsets.UTF_8)`。
## flush() 的作用：
带缓冲区的流（BufferedOutputStream、PrintWriter），数据先存在内存里，不调用 `flush()` 或 `close()`，数据不会真正写到硬盘（丢失数据）。flush() 就是 **“强制把缓冲区的存量数据，立即写入底层文件（或网络）。”**

---

# Stream 流（数据处理）—— 集合的“流水线”

** 它是干什么用的？**
简化集合操作
**对集合进行声明式处理**。代替繁琐的 for 循环，让代码更简洁、可读性更强。

##  核心三板斧（链式编程）
**数据源（集合） -> 中间操作（过滤/转换） -> 终端操作（结束）**

**经典案例**：从员工列表中找出“工资大于 5000”且“年龄小于 30”的姓名，并转成集合。
`List<Employee> employees = ...;`
传统写法:
```java
List<Employee> employees = ...; // 假设里面有很多员工数据

// 传统写法（最朴实、最易懂）
List<String> names = new ArrayList<>(); // 建一个空集合，准备存名字

for (Employee e : employees) { // 遍历每一个员工
    // 判断条件：工资大于5000 并且 年龄小于30
    if (e.getSalary() > 5000 && e.getAge() < 30) {
        names.add(e.getName()); // 满足条件，把名字取出来放进新集合
    }
}

// 最终 names 里就是 ["张三", "李四"]
```
再看“匿名内部类”写法（Lambda的前身）:
```java
List<String> names = employees.stream()
    .filter(new Predicate<Employee>() {
        @Override
        public boolean test(Employee e) {
            return e.getSalary() > 5000; // 条件1
        }
    })
    .filter(new Predicate<Employee>() {
        @Override
        public boolean test(Employee e) {
            return e.getAge() < 30; // 条件2
        }
    })
    .map(new Function<Employee, String>() {
        @Override
        public String apply(Employee e) {
            return e.getName(); // 取出姓名
        }
    })
    .collect(Collectors.toList());
```
Lambda
```java
// 没有 Stream 时（老写法）：至少要写 3 层 for + if + 新集合。
// 有 Stream 时（新写法）：
List<String> names = employees.stream()          
		// 1. 创建流
        .filter(e -> e.getSalary() > 5000)        
        // 2. 中间操作：过滤（工资>5000）
        .filter(e -> e.getAge() < 30)             
        // 3. 中间操作：再过滤（年龄<30）
        .map(Employee::getName)                   
        // 4. 中间操作：映射（取出姓名）
        .collect(Collectors.toList());            
        // 5. 终端操作：收集为 List

// 注意：此时 names 里只有 ["张三", "李四"]
```

##  必须警惕的“惰性求值”细节（巨坑）
**中间操作（filter/map）在没有遇到终端操作之前，根本不会执行！**

```java
List<Integer> list = Arrays.asList(1, 2, 3);
list.stream()
    .filter(x -> {
        System.out.println("过滤了：" + x); // 如果后面没有 .collect()，这行永远不会打印！
        return x > 1;
    });
// 此时只构建了流水线，数据并未处理。必须加上 .count() 或 .collect() 才会触发。
```

---

# 总结一下（帮你分清主次）

| 对比维度 | **IO 流（java.io）** | **Stream 流（java.util.stream）** |
| :--- | :--- | :--- |
| **核心目的** | 数据传输（读写硬盘/网络） | 数据处理（操作内存中的集合） |
| **操作对象** | 字节（byte）或字符（char） | 对象（泛型 T） |
| **是否消耗资源** | **必须手动/自动关闭**（close） | 不涉及资源释放，用完即抛 |
| **必须掌握度** | **极高（工作必备）** | 极高（代码优雅必备） |

**给你的建议**：工作中 99% 的文件读写，直接拷贝上面的 **try-with-resources** 模板就行。面试时，面试官最爱问 **IO 流的装饰器模式**（为什么 BufferedInputStream 套 FileInputStream）和 **Stream 的中间操作/终端操作区别**。先把这两块磕死，其他的遇到了再查手册。😊

# properties

一种类似于字典的键值对的存储数据结构
  ```java
  public class properties {  
    static void main() {  
        //键值对存储  
        Properties properties = new Properties();  
        //设置值  
        properties.setProperty("user","one");  
        properties.setProperty("password","123456");  
        //删除  
        properties.remove("user");  
        properties.setProperty("user","one");  
        //获取通过键的值  
        String username = properties.getProperty("user");  
        //输出  
        System.out.println("user = " + username);  
    }  
}

  ```

```java
public class prorandio {  
    static void main(String[] args) throws IOException {  
        Properties properties = new Properties();  
        properties.load(new FileInputStream
        ("D:\\AITerm\\JAVA\\untitled\\src\\NewTEam\\properties\\config.properites"));  
        //stringPropertyNames对应获取每个key的string  
        Set<String> strings = properties.stringPropertyNames();  
        for(String s :strings){  
            String detail  = properties.getProperty(s);  
            System.out.println("key="+s+",detail = " + detail);  
        }  
    }  
}
```
config.properties
```properties
# 这是注释（用 # 或 ! 开头  
# 冒号也可以当作分隔符  
# 等号两边有空格也没关系，Java会自动去掉  
username=admin  
password=123456  
url = jdbc:mysql://localhost:3306/test  
version: 2.0
```
#  线程
## 并发和并行
并发：指两个或多个事件在同一时间间隔内发生。这些事件宏观上是同时发生的，但微观上是交替发生的。
并行：指两个或多个事件在同一时刻同时发生。
并发 VS 并行
eg：假设小渣和老渣每人有两个女朋友。任务1：和一号约会；任务2：和二号约会...
进程和线程
进程：就是操作系统中正在运行的一个应用程序。
线程：就是应用程序中做的事情。比如：360软件中的杀毒，扫描木马，清理垃圾。
## 生命周期

![[Pasted image 20260901163321.png|825]]


![[Pasted image 20260901163447.png|903]]
# 多线程的三种实现方法

//多线程实现方式 ：
//
//继承Thread类  extends Thread 重写run方法 给Thread执行  
//实现Runnable接口 重写run方法 给Thread执行  
//实现Callable接口 重写call方法 加 FutureTask中转  给Thread执行  

thread类可以理解为我们线程的执行者，当然我们可以单独去调用它本身，也可以结合Runnable和Callable来混合使用

Runnable 和 Callable他们就不能单独使用，他们更像是线程的任务单。
我们在里面写好了我这个线程要做哪些事情，然后再去Thread类（或者线程池）去执行
## Thread类--线程任务实现者
 ```java
 public class thread extends Thread{  
    @Override  
    public void run() {  
        for(int i=0;i<5;i++){  
            System.out.println(Thread.currentThread().getName()+i);  
        }  
    }  
}
 ```

## Runnable 和 Callable 接口-- 线程任务单
==Run==
```java
public class MyRUn implements  Runnable {  
  
    @Override  
    public void run() {  
        try {  
            Thread.sleep(1000);  
            for(int i=0;i<3;i++){  
                System.out.println("【Runnable】我是任务单"+
	            Thread.currentThread().getName()+i+"，正在被某个工人执行");  
            }  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
  
    }  
}
```
==Call==带返回值版的Runnable
```java
public class Mycall implements Callable<String> {  //可以定义泛型，就是返回值的类型
    @Override  
    public String call() throws Exception {  
        Thread.sleep(500);  
        for(int i=0;i<4;i++){  
            System.out.println(Thread.currentThread()+"  "+i);  
        }  
        return "这是返回值";  //String
    }  
}
```
==调用实现==
 ```java
 public class Text extends  thread{  
    static void main(String[] args) throws ExecutionException, InterruptedException {  
        //普通Thread，直接使用
        thread thread = new thread();  
        thread.setName("MyThread111");  //为线程重命名
  
		//Runnable 利用Thread调用
        MyRUn myRUn = new MyRUn();  
        Thread thread2 = new Thread(myRUn);  
		
		//Callable先需要一`FutureTask（未来任务单）作为“桥梁”转接一下。
		//FutureTask本身也实现了Runnable，所以可以交给 Thread。
        Mycall mycall = new Mycall();  
        FutureTask<String> futureTask = new FutureTask<>(mycall);  
        Thread thread1 = new Thread(futureTask, "操作工");  
        thread1.setName("callable");  
		
		//设置线程的优先级，数字越小越优先，越大越靠后，范围1~10
        thread.setPriority(10);  
        thread1.setPriority(1);  
  
		//开启线程
        thread.start();  
        thread1.start();  
        thread2.start();  
		
		
		//获取返回 输出
        if(futureTask.get() != null){  
            String rul = futureTask.get();  
            System.out.println(rul);  
        }  
    }  
}
 ```
1. **`Callable` 的 `get()` 会阻塞**。  
    如果工人还没干完活，主线程执行 `future.get()` 会**卡住等待**，直到拿到结果。
	    （比如我的call方法里面写了太多的代码，运行了很久，这个返回值的获取会一直等到他完了才返回）
    如果计算太久，容易导致程序假死，建议用 `future.get(2, TimeUnit.SECONDS)` 设置超时。

## 核心区别
![[Pasted image 20260901165630.png|922]]


# 线程锁---线程安全保护

有两种针对的角度，一种是同步代码块，针对我的具体代码。一种是针对方法添加。
## 锁的本质
这里以同步代码块的形式展示：
在Java里，**任何对象**都可以当作锁（`synchronized` 括号里放的就是锁）。
- `synchronized(this)` 锁的是 **“当前这个具体的对象”**（好比你家**房门的钥匙**）。
- `synchronized(类名.class)` 锁的是 **“这个类的模板（Class对象）”**（好比**整栋楼的大门门禁卡**）。
> **重点理解**：房门的钥匙（this）**管不了**大门门禁（class）；同样，大门门禁（class）**也管不了**具体的房门钥匙（this）。它们俩是两套独立的系统！

就看我们之前的Runnable。我创建两个不同的类，
我对Myrun1施加了synchronized(this)，对Myrun2施加了synchronized(类名.class)

```java
public static void main(String[] args) throws InterruptedException {
		//创建一个对象，然后后面两个线程使用这一个对象
		Myrun1 LockThis= new Myrun1();
		new Thread(LockThis, "线程A").start();
		new Thread(LockThis,"线程B").start();
		//此时这两个线程调用的是同一个对象，但是它又施加了this，所以无法同时运行,会排队执行
		        
        Thread.sleep(100); // 等上面先跑起来
        
        System.out.println("========= 关键分界线 =========");
	    // 关键：创建 2 个不同的 Myrun1 对象
        // 两个线程传的是不同对象 -> 有两把 this 锁 -> 不会互斥，会同时执行！
        new Thread(new Myrun1(), "线程C").start();
        new Thread(new Myrun1(), "线程D").start();
        
        Thread.sleep(100);
        
        System.out.println("\n========== 场景3：测试 Class 锁（全局唯一） ==========");
        // 类锁只有一个，无论传多少个对象，都会排队执行
        //这里的操作就是我调了Myrun2的两个实例，
        //但是他们都是由Myrun2这个类实现的，对于这同一个类来讲，无论它有多少个实例，它最后都只能去依次运行
        new Thread(new Myrun2(), "线程E").start();
        new Thread(new Myrun2(), "线程F").start();
    }
```

```java
import java.util.concurrent.TimeUnit;

// ==========================================
// 1. 使用 this 锁的类（锁的是“当前对象”）
// ==========================================
class LockThis implements Runnable {
    @Override
    public void run() {
        // 锁住当前对象（this）
        synchronized (this) {
            System.out.println(Thread.currentThread().getName() + " 进入了 this 锁代码块");
            try {
                TimeUnit.SECONDS.sleep(2); // 模拟干活
            } catch (InterruptedException e) {}
            System.out.println(Thread.currentThread().getName() + " 退出了 this 锁代码块");
        }
    }
}

// ==========================================
// 2. 使用 Class 锁的类（锁的是“类的模板”）
// ==========================================
class LockClass implements Runnable {
    @Override
    public void run() {
        // 锁住 LockClass.class（全局唯一）
        synchronized (LockClass.class) {
            System.out.println(Thread.currentThread().getName() + " 进入了 Class 锁代码块");
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {}
            System.out.println(Thread.currentThread().getName() + " 退出了 Class 锁代码块");
        }
    }
}

// ==========================================
// 3. 测试入口
// ==========================================
public class LockTest {
    public static void main(String[] args) throws InterruptedException {
        
        System.out.println("========== 场景1：测试 this 锁（同一个对象） ==========");
        // 关键：只创建 1 个 LockThis 对象
        LockThis sameObject = new LockThis();
        // 两个线程传的是同一个对象 -> 共用一把 this 锁 -> 会排队执行
        new Thread(sameObject, "线程A").start();
        new Thread(sameObject, "线程B").start();
        
        Thread.sleep(3000); // 等上面跑完
        
        System.out.println("\n========== 场景2：测试 this 锁（不同对象） ==========");
        // 关键：创建 2 个不同的 LockThis 对象
        // 两个线程传的是不同对象 -> 有两把 this 锁 -> 不会互斥，会同时执行！
        new Thread(new LockThis(), "线程C").start();
        new Thread(new LockThis(), "线程D").start();
        
        Thread.sleep(3000);
        
        System.out.println("\n========== 场景3：测试 Class 锁（全局唯一） ==========");
        // 类锁只有一个，无论传多少个对象，都会排队执行
        new Thread(new LockClass(), "线程E").start();
        new Thread(new LockClass(), "线程F").start();
    }
}
```
# 针对方法的来讲
如果要实现this，就是在方法名上面加synchronized
如果是要实现针对xxx.class，那就是加上static synchronized
# wait和notify（等待和唤醒）
针对被当做锁的对象操作的方法
```java
Object lock = new Object();
//假设线程A和线程B同时被调用

// 线程A 执行这段代码
synchronized (lock) {
    System.out.println("A 进来了");
    lock.wait();   // ① 线程A停在这里，并且释放了锁
    System.out.println("A 被唤醒了，继续执行"); // ④ 最后执行
}

// 线程B 执行这段代码（在A wait之后才能进来，因为A释放了锁）
synchronized (lock) {
    System.out.println("B 进来了"); // ② B 拿到了锁，进来了
    lock.notify(); // ③ B叫醒A，然后B继续执行完自己的代码块
    System.out.println("B 执行完毕，准备释放锁");
} // ⑤ B释放锁后，A才能重新抢到锁，继续执行
```
总的来说，就是对锁锁住了之后，当前的线程就会停在那里，然后后面正在排队的也被同一个锁锁住的线程就被释放了，
开始运行它的代码，假如第二个线程里面有释放的话，那么第二个线程就会去把线程给释放掉。
但是如果此时第二个线程它本身还有代码没有执行完毕的话，那他就会先把他剩余的代码执行完毕，才会把当前线程交出来。然后A才能直行
因为B还在执行，所以它正在占用这个线程。即使释放了线程A的锁，依然无法执行

锁等待会关闭锁线程里的任务暂停，释放出当前线程锁被释放线程不会立刻执行，需要等待有空余线程时才能继续执行

![[Pasted image 20260901174040.png|925]]


Wait会释放锁sleep不会释放锁。那么假如有两个线程，它们采用了同一个锁，第一个线程先去调用，然后它没有执行wait，而是执行了sleep。如果是wait，会把它当前的这个线程给释放出来，给第二个现在使用。但是sleep会不会呢

**如果线程1执行的是 `sleep()`，它绝对不会释放锁。线程2会被死死挡在门外，必须等线程1睡醒，并且执行完 `synchronized` 代码块（释放锁）之后，线程2才能进去。**
# 线程池
```java
ThreadPoolExecutor threadPool = new ThreadPoolExecutor(  
        2, //核心线程数
        5,//临时线程3，五减二等于三.当我们的任务提交数大于核心加最大队列数时  
        2,//等待时间单位数量  
        TimeUnit.MINUTES,  //时间单位
        new ArrayBlockingQueue<>(10)//对列长度  
        , Executors.defaultThreadFactory(),//线程创建的方式  
        new ThreadPoolExecutor.AbortPolicy());//拒绝： 当我们的任务提交数大于临时线程加最大队列数时
```
总的来说就是线程池里面有核心线程和临时线程，还有队列。我们任务传进来先交给核心线程，然后多余的任务放在队列里面排队等待，如果核心线程和队列都排完了，依旧有多余的任务，那么说明线程数不够，就开启更多线程，调用临时线程，如果连临时线程也不够的话，就是采用拒绝方法
```java
MyRUn myRUn = new MyRUn();  //创建一个线程
threadPool.submit(myRUn);   //交给线程池去
```

