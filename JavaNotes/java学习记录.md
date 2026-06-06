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
# 要进行语法运行，要有一个方法，这一点和c基础很像

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
[[#^1c2c31|引到后面类的具体学习]]
```java
//需要一个方法的创建
Scanner sc = new Scanner(System in);
int a = sc.nextInt();
```
![[Pasted image 20260530210149.png|371]]


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

^1c2c31
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

![[Pasted image 20260530233645.png|363]]
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