权限修饰符：public        protected     默认不写    private

特征修饰符：abstract         final           static           nativ等等

> private：本类。 
>
> 默认：本包
>
> protected：本包+子类(子类对象调）
>
> public：所有

类的成员：

* 属性：权限  特征  类型  名字  [=赋值]
* 方法：权限  特征  返回类型  名字(参数)  异常  [{方法体}]
* 构造函数：权限  名字(参数)  异常  {方法体}
* 块：方法体

类：`public` 或 `不写`

1. 普通类：public class Person{ ... }
2. 本包私有类： class Person{ ... }
3. 抽象类：public abstract class Person{ 属性; ...可以有0或多抽象方法...普通方法...静态方法...块... }  
   * 可以有构造函数，不能实例化。可以有块。
   * 没有抽象方法，都是具体方法，也可以定义为抽象类，抽象类不能实例化，根据需求设定即可
4. 接口：public interface Box{ 属性都是静态常量 - 默认public static final( - 没有实例字段)；只有抽象方法} 
   * 接口不是类。不能有构造函数，不能实例化。
   * 不能有块。

> 1）**接口**不能**有构造**方法，抽象类可以**有**。 2）**接口**不能**有**方法体，抽象类可以**有**。 3）**接口**只有抽象方法，1.8后可以**有**静态方法（只能调静态字段）。抽象类可以**有**静态方法和普通方法和抽象方法，数量无限制。 4）在**接口**中凡是变量必须是public static final，而在抽象类中没有要求。

属性：只能声明 或 声明时赋值。不能写赋值语句。

1. 普通属性：public/private/默认/protected Computer[] computers;    这个属性是一个数组。
2. 静态属性：private static int nextId;  属于类的 ，不属于任何一个对象，可改变，private只能类内部访问。
3. 静态常量：public static final String name = "Static Name";   属于类。凡**final属性**必声明时赋值。final属性不能被改。静态常量建议public。
4. 抽象类的属性：同上。
5. 接口的属性：String name; 默认都是public static final共有静态常量。

方法：

1. 静态方法：public static double random(){  ...  }  只能访问静态变量，不能访问对象变量。（在静态区加载，但是不默认执行，调用时才执行）静态方法可以直接通过类来调用。
2. final方法：public final boolean testTrue(){  ...  }  不能被子类重写。

3. 抽象方法：public abstract void getDescription();  必须public。没有方法体，必须被子类实现。或子类也是抽象类，保留抽象方法。
4. native方法：public native clone();   方法体内容由其他语言写成，没有java方法体。

构造函数：

1. 普通类的构造函数：4权限修饰符都可用
2. 抽象类的构造函数：4权限符都可（但是不能构造对象，只能给子类用）
3. 接口没有构造函数。

块：`static` 或 `无`

1. 普通块。默认在执行构造函数前执行。
2. static块。静态块，类加载时就执行。所以是先执行静态块（静态块可访问静态属性），再执行普通块。