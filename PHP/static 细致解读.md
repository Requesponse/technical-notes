### static 细致解读 

- 静态变量

> 知识点  



1. 静态变量必须在声明为静态变量时初始化，否则静态变量将失去意义。

2. 静态变量在声明并初始化第一次后再次执行到 static 关键字的声明并初始化的那行代码时将不再进行声明和初始化，也就是说静态变量的值只会声明和初始化一次，因为静态变量仅在php代码编译时被解析生成。



> 代码示例



```php

<?php



function counter()

{

    static $count = 0;//声明并初始化 $count 为函数内部的静态变量

    //static $count = 1+2;//这样声明会导致报错，因为静态变量不接受PHP表达式，因为静态变量的声明并初始化时编译时解析出来的，而表达式需要运行时才能获得值

    //$totalScore = 1;//static声明将失去意义，因为couter的值每次都会赋值为1

    echo $count."\n";

    // or do something else.

    $count++;

}



counter();//output:0

counter();//output:1

counter();//output:2



class Foo{

    public function stepCounter($addStep)

    {

        static $stepNumber = 0;//声明并初始化 $stepNumber 为类示例方法内部的静态变量

        $stepNumber = $stepNumber+$addStep;

        echo $stepNumber."\n";



    }



    public static function scoreCounter($addScore)

    {

        static $totalScore = 0;//声明并初始化 $totalScore 为类静态方法内部的静态变量

        $totalScore = $totalScore+$addScore;

        echo $totalScore."\n";



    }

}



$foo = new Foo;



$foo->stepCounter(5);//output:5

$foo->stepCounter(5);//output:10

$foo->stepCounter(5);//output:15



Foo::scoreCounter(10);//output:10

Foo::scoreCounter(10);//output:20

Foo::scoreCounter(10);//output:30

```

- 静态延迟绑定

> 知识点



1. 静态延迟绑定跟静态变量以及类静态方法等没有任何联系，PHP 语言小组原本打算定义一个新的语法关键字来使用静态延迟绑定，

但最后还是选择了已有的 static 关键字。

2. 静态延迟绑定中的"延迟绑定"是指 static:: 不再被解析为当前方法所在的类，而会被解析成运行时所计算出来的类。

> 代码示例

```php

<?php

class A {

    public static function who() {

        echo "A";

    }

    public static function test() {

        self::who();

        static::who();

    }

}



class B extends A {

    public static function who() {

        echo "B";

    }

}



class C extends B{

    public static function who() {

        echo "C";

    }

}



C::test();//output:AC

```

当我们删除 C 类中的 who 方法时输出为 AB，当我们删除 B 类中的 who 方法时输出为 AA，可以发现静态延迟绑定其实是为了让我们推导出静态方法最先被调用的那一个。



- 最佳实践

1. 不要用静态变量，因为它是一种非常规语法，并且会带来代码的副作用。

2. 静态延迟绑定（late static bindings）一定要好好理解一下，因为它会帮助你看懂 PHP 开源框架的源码实现。
