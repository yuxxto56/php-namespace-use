# PHP命名空间概念与使用
本文主要讲解PHP的命名空间与使用
### 概念
* 什么是命名空间？从广义上来说，命名空间是一种封装事物的方法。在很多地方都可以见到这种抽象概念。例如，在操作系统中目录用来将相关文件分组，对于目录中的文件来说，它就扮演了命名空间的角色。具体举个例子，文件 foo.txt 可以同时在目录/home/greg 和 /home/other 中存在，但在同一个目录中不能存在两个 foo.txt 文件。另外，在目录 /home/greg 外访问 foo.txt 文件时，我们必须将目录名以及目录分隔符放在文件名之前得到 /home/greg/foo.txt。这个原理应用到程序设计领域就是命名空间的概念。
### 使用
* 本次demo项目的主要结构体系是:
```
|__index.php
|__Model_|test.php
         |demo.php
```
* test.php文件编码：
```
<?php 
  /**
   * test file
   */
  namespace app\Model; #定义了命名空间 app\Model,可以理解当前类文件在app\Model目录下

  class test{ #类名称为test

     public function sayHello(){ #定义一个方法名为sayHello
        echo "say Hello";
     }
  }
?>
```
* index.php文件编码：
```
<?php
  namespace app;  
  use app\Model\test; 
  function myAutoload ($class) {
        if(strripos($class,'\\') !== false){
            $class = substr($class,strripos($class,'\\')+1);
        }
        include(__DIR__ .DIRECTORY_SEPARATOR."Model".DIRECTORY_SEPARATOR. $class . ".php");
  }
  spl_autoload_register("\app\myAutoload");
  $a  = new \app\Model\test();
  $a->sayHello();
  
  #备注
  namespace app;  #定义index.php是在app目录下工作的
  use app\Model\test; 
  1、命名空间导入\别名操作符,使用它可以简化实例化其他目录下的文件类、函数等，当前表示导入Model目录下test.php文件下test类
  2、使用use app\Model\test;,则实例化可直接$a=new test();即可，不使用实例化则必须 $a  = new \app\Model\test();
  3、前提:use操作符不是真正意义上导入，而是简化被实例化对象类对应的命名空间，使用上诉，则必须通过自动加载方法遍历需要加载的目录文件，php通常使用自动加载方法函数是：spl_autoload_register，index.php文件也有例子。
  
```
### 引用
PHP命名空间参考：[https://www.php.net/manual/zh/language.namespaces.php](https://www.php.net/manual/zh/language.namespaces.php)
