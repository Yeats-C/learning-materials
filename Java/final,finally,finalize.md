## Final是一个修饰符：
    当final修饰一个变量的时候，变量变成一个常量，它不能被二次赋值
    当final修饰的变量为静态变量（即由static修饰）时，必须在声明这个变量的时候给它赋值
    当final修饰方法时，该方法不能被重写
    当final修饰类时，该类不能被继承
    final不能修饰抽象类，因为抽象类中会有需要子类实现的抽象方法
    final不能修饰接口，因为该接口有需要实现类来实现的方法

## Finally
    finally只能与try/catch语句结合使用，finally语句块中的语句一定会执行，并且会在return，continue，Break关键字之前执行

## Finalize
    Fianlize是一个方法，属于Java.lang.Object类，Finalize方法时GC运行机制的一部分。Finalize方法是在GC清理它所从属的对象时被调用的
