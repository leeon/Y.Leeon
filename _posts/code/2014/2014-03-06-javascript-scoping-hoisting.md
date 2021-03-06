---
layout: post
title: "JavaScript Scoping and Hoisting"
category: code
tags: JavaScript
---

>看到一篇好的文章,忍不住要翻译一下了,本文来自twitter工程师 ben cherry的[JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html),主要讲解了JavaScript中的变量和函数声明的解析顺序，阅读本文之前,读者最好掌握了JavaScript中变量对象VO、函数声明FD以及函数表达式FE的概念.考虑到作者没有详细解释的部分,我进行了代码演示以及文字补充。翻译不恰当的地方,欢迎交流指出。
<!-- break -->

##正文
你知道下面的JavaScript程序执行后,alert会得到什么结果吗？

    var foo = 1;
    function bar() {
        if (!foo) {
            var foo = 10;
        }
        alert(foo);
    }
    bar();

答案是 10,如果你觉得很意外，那么下面这个可能就会更让你震惊了.

    var a = 1;
    function b() {
        a = 10;
        return;
        function a() {}
    }
    b();
    alert(a);

这段代码在浏览器中执行会显示1.为什么呢？尽管这个例子看起来比较奇怪，令人疑惑，但这恰恰正是JavaScript语言的一个强大的特性.我不知道是不是有一个标准的名字来描述这种机制，但是我喜欢用“hoisting”这个词.这篇文章将会解开这种机制(hoisting)的面纱。开始之前，先让我们先去理解一下JavaScript的作用域吧.



##JavaScript中的作用域

对于JavaScript初学者来讲，最容易迷惑的就是作用域。事实上，不仅仅是初学者，我遇到过很多有经验的JavaScript程序员也不完全理解作用域。JavaScript的作用域如此难以理解的原因或许是它看起来像C语言家族的语言，看下面的代码：

    #include <stdio.h>
    int main() {
        int x = 1;
        printf("%d, ", x); // 1
        if (1) {
            int x = 2;
            printf("%d, ", x); // 2
        }
        printf("%d\n", x); // 1
    }

程序的输出是1,2,1.原因是C语言以及其家族语言，拥有块级作用域。当程序进入一个块区域，比如if-else语句的时候，一个新的作用域就被创建了，你可以在里面定义新的变量，而不会影响其他作用域。但这种机制并不适用于JavaScript，看下面的代码：

    var x = 1;
    console.log(x); // 1
    if (true) {
        var x = 2;
        console.log(x); // 2
    }
    console.log(x); // 2

上面的代码在firebug中会显示1，2，2.这是因为JavaScript拥有函数级的作用域，这与C家族的语言非常不同。在JavaScript中块，例如if语句，不会创建一个新的作用域，只有函数才会创建新的作用域。

对于许多习惯了C, C++, C# 或者 Java这样的语言的程序员，这个特性显得很不友好。幸运的是，得益于JavaScript函数的灵活性，还是有变通方法的，如果你需要在函数内部创建一个临时的作用域，可以使用（立即执行函数表达式IIFE）:

    function foo() {
        var x = 1;
        if (x) {
            (function () {
                var x = 2;
                // some other code
            }());
        }
        // x is still 1.
    }

这种方法其实非常灵活，你可以在任何地方创建临时的作用域，不仅仅是块语句中。然而，我强烈建议花点时间真正的理解和接受JavaScript作用域。它非常的强大，并且是我最喜欢的JavaScript特性之一。如果你理解了作用域，那么hoisting就更容易理解了。

###声明, 变量名, 和声明提升

在JavaScript中一个变量名可以通过下面四个方式进入一个作用域:(也就是被当前作用域识别)

1. 语言内置定义Language-defined：所有作用域都默认包含this 和 arguments变量。
2. 函数形式参数Formal parameters：函数可以拥有形式参数,并且在其函数体作用域内生效。
3. 函数声明Function declarations：形如function foo() {}的声明。
4. 变量声明Variable declarations: 形如var foo;的声明。

函数声明和变量声明总是被JavaScript解释器移动到作用域的顶部(hoisting).函数的参数和语言内置定义的变量很显然在这之前已经在那了。这意味着下面的代码：

    function foo() {
        bar();
        var x = 1;
    }

其实被解释成了这样:

    function foo() {
        var x;
        bar();
        x = 1;
    }

有一种情况是声明所在的语句根本不会执行，但这并不影响hoisting机制,下面两个函数就是等效的：

    function foo() {
        if (false) {
            var x = 1;
        }
        return;
        var y = 1;
    }
    function foo() {
        var x, y;
        if (false) {
            x = 1;
        }
        return;
        y = 1;
    }

注意有时声明和赋值会一起写，但是赋值部分并不会**提升(hoist)**，只有变量名会被提升。对于函数声明，整个函数名和函数体都会得到提升。但是记住函数声明有两种方式，思考下面的JavaScript代码：

    function test() {
        foo(); // TypeError "foo is not a function"
        bar(); // "this will run!"
        var foo = function () { // function expression assigned to local variable 'foo'
            alert("this won't run!");
        }
        function bar() { // function declaration, given the name 'bar'
            alert("this will run!");
        }
    }
    test();

这种情形下，bar函数声明的函数体被提升到顶部了,foo这个变量被提升了，但是他对应的函数体仍在原来的位置，执行到那里的时候才会赋值。

>补充：原作者在这里并没有详细的介绍函数声明FD和函数表达式FE的概念，上面代码中bar对应的是一个函数声明，而foo对应的语句是一个函数表达式。bar所在的语句是一个完整的函数声明，所以会全部得到提升，而foo所在的语句严格来讲并不是一个函数声明，他可以分为两部分：一是声明了一个普通变量foo,然后把一个函数赋值给foo.因此声明foo变量的语句会提升，而后面的函数部分则不会。

前面包含了hoisting的基本概念，看起来并不是那么复杂令人疑惑.当然在一些特定的场景下，还是有点复杂的。

##变量识别顺序

我们要记住的重要的特例是变量的识别顺序。前面提到过变量名进入作用域有四种方式。刚刚我们列出的顺序就是他们被识别的顺序。通常，如果一个变量名已经被定义了，它就不会被另一个相同名字的变量覆盖(后者被忽略)。这意味着函数声明比函数变量声明有着更高的优先级。但是这并不影响对这个变量的赋值会继续进行，只是声明部分被忽略了。

>补充一段代码，表达原作者的意思：

    function foo(){}
    var foo = 3;
    console.log(foo);//3

这个例子会被解释器解释为：

    function foo(){}
    //var foo;  //这条语句就被忽略了。
    foo = 3;
    console.log(foo) 


接原文，当然也有一些例外:

+ 内置的变量arguments表现的比较怪异。它看起来优先级位于函数形式参数和函数声明之间，这意味着如果形式参数中存在一个叫做arguments的变量，将比语言内置的arguments拥有更高的声明优先级.这是一个不好的特性，不要使用arguments作为形式参数。
+ 在任何地方使用this作为标示符都会导致一个语法错误，这是一个好的特性。
+ 如果多个形式参数拥有相同的名字，那么最后一个将拥有最高的优先级，即使它是undefined。


##带有名字的的函数表达式

你也可以给函数表达式中的函数起一个名字，采用类似函数声明的语法。但这并不能使它变成一个函数声明，并且这个名字也不会添加到作用域，函数体也不会被提升。下面的代码演示了我所说的：

    foo(); // TypeError "foo is not a function"
    bar(); // valid
    baz(); // TypeError "baz is not a function"
    spam(); // ReferenceError "spam is not defined"

    var foo = function () {}; // anonymous function expression ('foo' gets hoisted)
    function bar() {}; // function declaration ('bar' and the function body get hoisted)
    var baz = function spam() {}; // named function expression (only 'baz' gets hoisted)

    foo(); // valid
    bar(); // valid
    baz(); // valid
    spam(); // ReferenceError "spam is not defined"


##用这些知识编程

现在你已经理解了作用域和声明提升，这对于JavaScript编程意味着什么呢？最重要的就是你应该总是使用**var**关键字来声明变量。我强烈的建议你在每一个作用域的顶部恰好写一个var语句(多变量的时候，用逗号连接)。如果你要求自己这么做，就不会遇到hoisting相关的困惑了。不过，这么做会让你寻找当前作用域中声明的变量变得更困难。我建议使用JSLint的onevar选项来加强这些，例如下面的代码：

    /*jslint onevar: true [...] */
    function foo(a, b, c) {
        var x = 1,
            bar,
            baz = "something";
    }

##标准怎么说

我发现通常直接参考[ECMAScript Standard (pdf)](http://www.mozilla.org/js/language/E262-3.pdf) 来理解这些机制非常的有效。这是标准中关于变量定义和作用域的描述：

>如果变量语句出现在一个函数声明内部，涉及的变量就会定义在函数内部作用域,参考章节10.1.3. 否则，他们就会被定义在全局作用域内(作为global对象的一个属性)，参考章节 10.1.3。 变量在进入作用域的时候被创建。一个block不能定义一个执行环境作用域。只有程序和函数声明可以创建作用域。变量在创建的时候会被初始化为undefined。一个带有初始化语句的变量在赋值语句执行的时候才会被赋上其赋值表达式对应的值，赋值并不是发生在变量创建的时候。



读完这篇文章建议去知乎上challenge一下这个[问题](http://www.zhihu.com/question/22949631)：） 


以上。

