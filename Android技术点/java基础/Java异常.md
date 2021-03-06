#### 说说Java异常体系主要用来干什么的 & 异常体系？

> ## **java异常体系**
>
> |——Throwable 实现类描述java的错误和异常 一般交由硬件处理
>
> 　　　　|——Error(错误)一般不通过代码去处理，一般由硬件保护
>
> 　　　　|——Exception(异常)
>
> 　　　　　　|——RuntimeException(运行时异常)
>
> 　　　　　　|——非运行时异常
>
> java异常体系分为2类
> 1.错误（Error），一般与虚拟机、系统有关。这一类的错误无法由程序捕获，程序中断，比如内存溢出。
> 2.异常（Execption），一般有程序编码或者设计导致的错误，可以由程序捕获，程序不中断。异常分为检查型异常和非检查型异常：
> 检查型异常:编译器必须强制处理的异常，比如SQLExecption,ClassNotFoundExecption等。
> 非检查型异常:分为RuntimeExecption，比如NullPointerExecption,NumberFormatExecption异常。
>
> ### **多个try-catch语句联用时的顺序**
>
> 　　1、顺序执行，从上到下，有一个catch子句匹配之后，后面的自动不在执行
>
> 　　2、如果多个cach内的异常有父子类的关系
>
> 　　　　一定要，子类异常在上，父类异常在下
>
> ### **自定义异常类型**
>
> 一般都是提供两个构造参数，一个无参一个有参数，有参数的一般是调用父类的有参构造函数，调用形式super(message)
>
> ### **运行时异常**
>
> RuntimeException
>
> 　　|——ClassCastException多态中可以使用instanceof 进行规避
>
> 　　|——ArithmeticException进行if判断，吐过除数为0进行return
>
> 　　|——NullPointerException进行if判断是否为null
>
> 　　|——ArrayIndexOutBondsExcetion使用数组length属性以避免数组越界。
>
> 　　在后面我们异常处理的时候，经常把捕获的一场装华为运行时异常抛出，尤其是写一些函数框架时。throw new RuntimeException(e);
>
> **非运行时异(受检异常) 这些异常必须做出try-catch不然编译器无**法通过 注意事项
>
> ​	1、子类覆盖父类的方法时，父类方法抛出异常，子类的覆盖方法可以不抛出异常或者抛出父类方法相同的异常，或者抛出父类方法异常的子类。
>
> 　　2、父类方法抛出了多个异常，子类覆盖方法时，只能抛出父类异常的子集
>
> 　　3、父类没有抛出异常，子类不能抛出异常。子类发生非运行时异常时，需要进行try-catch处理
>
> 　　4、子类不能比父类抛出更多的异常。
>
> 　　凡事应当向父类看齐，父类已有就应当向分类看齐。
>
> ### **finally块 一般用于释放资源 无论程序正常与否都执行finally块**
>
> 　　1.只有一种情况，jvm退出了System.exit(0)这时候不会执行finally的内容
>
> 　　2、return语句也无法阻止finally的执行

#### Error和Exception的区别？

> 1.error：error 是指在正常情况下，不大可能出现的情况，绝大部分的 Error 都会导致程序处于非正常的、不可恢复状态。（这个时候运行的服务极有可能会down掉）--**不可控，比较难捕获，偏底层环境**
>
> 2.exception：exception是在程序编写过程中由于语法错误或者其他逻辑错误产生的异常，分为运行时异常和编译时异常；编译时异常是开发工具在编译期显示捕获的异常，而运行时异常常常与逻辑错误挂钩，是程序员编写程序时产生的错误。（一般异常出现时我们要主动去捕获，并且去想办法修复）--相对可控 
>
> **两者的“同”**：error和exception都是继承于Throwable类
>
> 回望exception的处理机制：（throw和try…catch）throw会一层一层的往上抛异常，而与之对应的是try…catch，在可能出现异常的代码块，我们加上try…catch即可，否则可能产生额外的性能开销（Java 每次实例化 Exception时，会对对应的栈进行快照，当放在try…catch的代码块很大的时候，那么就是一个很重的操作，会开始对性能产生不可忽视的影响）

#### 说说运行时异常和非运行时异常的区别？

> 运行时异常： 都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。
>
> 非运行时异常 （编译异常）： 是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

#### 如何自定义一个异常？

> 在Java中要想创建自定义异常，需要继承Throwable或者他的子类Exception。
>
> **自定义异常（重点）**
>
> A) 概念：一般在开发中只要业务方法出现例外基本上都交给一个异常类来处理。
>
> B)如何开发：
>
> i.   建立一个类，让此类继承Exception（一般不用）或RuntimeException（常用）
>
> ii.   建立一个成员变量message,显示写出无参构造和有参构造

#### throw和throws 的区别？

> Throw：
>
> 作用在方法内，表示抛出具体异常，由方法体内的语句处理。
> 具体向外抛出的动作，所以它抛出的是一个异常实体类。若执行了Throw一定是抛出了某种异常。
> Throws：
>
> 作用在方法的声明上，表示如果抛出异常，则由该方法的调用者来进行异常处理。
> 主要的声明这个方法会抛出会抛出某种类型的异常，让它的使用者知道捕获异常的类型。
> 出现异常是一种可能性，但不一定会发生异常。

#### try{}catch{}finally{}可以没有finally吗？

> 可以没有finally,

#### finally中的代码总是会执行吗？

> no，如果一个方法内在执行try{}语句之前就已经return了，那么finally语句指定不会执行了。因为它根本没有进入try语句中
>
> 如果在一个try语句中调用System.exit(0);方法，那么就会退出当前java虚拟机，那么finally也就没有执行的机会了。

#### return在try{}catch{}finally{}中执行具有哪些规则

> 1、不管有没有出现异常，finally块中代码都会执行；2、当try和catch中有return时，finally仍然会执行；3、finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，不管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；4、finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。