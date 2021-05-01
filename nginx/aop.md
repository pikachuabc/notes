# AOP

![](../.gitbook/assets/image%20%283%29.png)

Advice表示想要增加的内容，比如方法执行前/后/中要进行的操作就是下面pointcut中@Before修饰的方法中写的内容

Aspect的概念就是AOP这个概念中的类\(spring中确实也是一个普通的类加上@Aspect注解\)

PointCut就是新加内容方法上那个@Before/After...\(exceution\("com.au.\*..\(..\)"\)\)中那个exceution，筛选要插入的方法，或者类的所有方法，或者某个包下所有类的所有方法

JointPoint这个词感觉就单纯是个概念，jointpoint在spring中其实就是方法的执行，上面那个execution会筛选joint point（其实就是方法名，那个类似于正则的表达式的返回值）然后做插入新加的方法内容（其实是用proxy代理类实现的，插入这个词也不太准确），由@Before决定在方法执行之后执行新的方法内容

{% hint style="success" %}
_Join point_: a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point _always_ represents a method execution.
{% endhint %}

