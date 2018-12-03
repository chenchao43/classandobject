4.1概述
类之间的关系
依赖usea 聚合hasa 继承isa
对象的行为、状态 标识
42预定义类Date与LocalDate
date一般用来处理时间，是对1970年1月1日的毫秒数
localdate用来处理日期

* LocalDate 只包含了年月日的信息，不包含时间和时区。
* LocalDate 重写了 toString() 方法，一般的格式为"yyyy-MM-dd"。
* LocalDate 为不可变 class，任何对 LocalDate 的实例进行修改的方法将返回一个新的实例。


注意不要编写返回可变对象的访问器方法，如
private Date hireDay；
public Date getHireDay{
    return hireDay；
}
这样破坏封装性，因为date有settime可以在类外更改
应该return (Date) hireDay.clone();

静态域和静态方法
静态域：类的所有实例共享一个静态域
静态常量：一般为static final
静态方法：如Math.pow(x,a);
    静态方法是不能向对象实施操作的方法，没有this参数
    静态方法不能访问非静态域，因为它不能操作对象，但是可以访问自身类中的静态域，但是可以使用对象调用静态方法，建议使用类名
    两种情况：1方法不需要隐式参数，全由显式参数提供
    2方法只需要访问静态域
    
工厂方法
利用不同的多种构造器产生不同风格的输出

main方法
同理，main也是静态方法。 
每一个类都可以有一个main方法，这是出于单元测试的考虑

如果只想单独的运行某个类 
java xxx 
如果这个类是某个大型程序的一部分 
java Application

方法参数
JAVA总是按值调用，也就是说方法的参数都是拷贝

一个方法不能修改一个基本数据类型的参数（数值，布尔） 
一个方法可以改变一个对象参数的状态 
一个方法不能让对象参数引用一个新的对象

重载
什么是重载？比如构造器，对于有些构造函数，你可以写参数，你也可以不写参数。这是因为编辑器把你的构造重载到了不同的函数上，这个过程叫做重载解析

JAVA允许重载任何方法，不只是构造器方法

默认域初始化
如果构造器没有显式赋值给域，则自动赋值0，布尔是false，对象是null。但是一般来说都需要赋值。

无参数的构造器
很多时候类里会放一个无参数的构造器，这时候要赋适当的默认值 
事实上如果你写了一个类却没有写构造器，系统会隐式给你一个构造器把所有域初始化为默认值。 
但是如果你写了一个带参数的构造器，却在实例时没有用参数，此时实例不合法。 
也就是说，只有没有任何构造器时，系统才提供构造器。

显式域初始化
确保不管怎么使用构造器，实例域都能初始化到一个合适的值是一个好习惯（为了达到这一点，需要很多的构造器）

参数名
技巧：构造器参数名在将要赋值的域前面加个a，这样既能知道在为什么赋值，也能保证不重名，很清晰

还有一种方法

public Employee(String name,double salary){
    this.name=name;
    this.salary=salary;
}

因为参数会屏蔽实例域，但是使用this.可以强制访问实例域。达到目的。

调用另一个构造器
public Employee(double s){
    this("Employee #"+nextId,s);
    nextId++;
}

这样将会调用Employee(String,double) 
（前面拼接成一个string，后面是double，this代表本身，既构造器） 
这样的好处是公共构造器的代码只用写一次

初始化块
初始化数据域的方法：构造器中设置值，声明中赋值，初始化块

class Employee{
    private int id;
    private static int nextId;
    {
        id=nextId;
        nextId++;
    }
}

无论哪个构造器被调用，这个初始化块都会先被调用。先运行初始化块，再运行构造器本身

当然，这只是一种方法。直接放在构造器里初始化也可以

static{
}

也可以在初始化块前面加上static表示静态初始化块

public class Hello{
    static{
        System.out.println("Hello,World");
        System.exit(0);
    }
}

这个hello,world没有main，因为当这个类被加载时，静态初始化块就已经运行了。

包
JAVA允许用包将类组织起来，借助包可以方便的管理代码。并将自己的代码与别人的代码库分开管理 
包可以嵌套 
包也可以确保类唯一，同样类名在不同包内使可以的

类的导入
一个类可以访问当前包内所有类，以及其他包中的公共类(public class)

访问公共包的方法

//第一种
java.util.Date today=new java.util.Date()

//第二种
import java.util.*;

这段代码告诉我们，公共类中的代码是可以使用绝对定位调用的。当然还是import进来最方便

注意：*符号只能导入一个包，不能导入嵌套其他包的包

但是有的时候不同的包之间会有类名冲突，编译器不知道用哪个类

import java.util.*;
import java.sql.*;
//这样Date类就会冲突

//解决方法1：如果只需要用一个类
import java.util.*;
import java.sql.*;
import java.util.Date;

//解决方法2：两个类都要用
java.util.Date deadline=new java.util.Date();
java.sql.Date today=new java.sql.Date(...);

import与#include是有区别的，C++中使用#include是因为C++编译器无法查看任何外部文件的内容。但是JAVA编译器可以查看外部文件，只需要告诉编译器去哪里就行。比如直接给出包名而不导入。

静态导入
import static classname

//举例
import static java.lang.System.*;
out.println("Goodbey,World!");
exit(0);

静态导入之后就可以不加类名直接使用静态方法 
事实上还可以导入特定的类

import static java.lang.System.out;
1
这种方法看起来比较清晰

将类放入包中
package com.xxxx.corejava;
1
如果代码里没有指定在什么包里的话，那么源文件将会被放到默认包里去。默认包是一个没有名字的包。

包其实就是文件夹。所以也要根据包的层次结构放置文件

包作用域
如果一个东西没有被标记public或者private，那么可以在同一个包中的所有方法访问。

所以类变量一定不要忘了private，不然默认包可见

类路径
类文件也可以存储在JAR文件中（JAVA归档） 
JAR文件使用ZIP格式组织文件和子目录，可以使用压缩工具查看内部。

文档注释
JDK工具：javadoc可以生成API文档

类注释
类注释必须放在import语句之后，类定义之前

import ...
/**
....
.....
....
....
*/

public class xxx{
    ...
}

方法注释
@param 变量描述 
@return 返回描述 
@throws 类描述

域注释
一般只需要对静态常量建立文档

通用注释
@author 姓名 
@version 文本 
@since 文本 
@deprecated 文本 添加一个不再使用的标记 
@see 引用 这是一个超联接，引用直接写包路径就行

包概述和注释
方法1：提供一个package.html的文件，\\之间的内容会被提取

方法2:提供一个package-info.java文件，包含/**和*/界定的javadoc注释

还可以提供一个概述性注释:overview.html

注释的提取

类设计技巧
1.一定要保证数据私有
2.一定要对数据初始化
3.不要在类中使用过多的基本类型（使用组合类型代替多个组合类型，这样会易于理解）
4.不是所有域都需要独立访问器和修改器（有些东西根本不需要修改，比如日期记录）
5.将类进行分解（把复杂的大类分解为几个小类）
6.类名和方法要体现职责

    
    

