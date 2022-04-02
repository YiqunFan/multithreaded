# A Study in Multithreaded Design Pattern

记录一下自己学习《Java多线程设计模式》的一些核心点。

具体的代码已经放在Github上：

https://github.com/YiqunFan/multithreaded



## 01 Single Threaded Execution模式

所谓Single Threaded Execution模式，意思就是以一个线程执行。就像独木桥同一时间内只允许一个人通行一样，该模式用于设置限制，以确保同一时间内只能让一个线程执行处理。

当我们修改多个线程共享的实例时，实例就会失去安全性。所以，我们应该仔细找出实例状态不稳定的范围，将这个范围设为临界区，对临界区进行保护，使其只允许一个线程同时执行。

Java中synchronized来定义临界区，保护多个线程共享的字段。



## 02 Immutable模式

Immutable的意思就是不变的，这个模式中存在着确保实例状态不发生改变的类。在访问这些实例时并不需要执行耗时的互斥处理，因此若能巧妙利用该模式，定能提高程序性能。

在设计类和理解已有的类时，要注意类的不可变性（immutability）。String就是一个Immutable类，StringBuffer就是一个Mutable类。StringBuffer表示的字符串可以随便改写，为了确保安全，改写时需要妥善使用synchronized，而String类则不能改写，String的所有方法都不是synchronized方法，所以应用速度也就更快。



## 03 Guarded Suspension模式

Guarded是守护、被保护的意思，Suspension则是暂停的意思。如果执行现在的处理会造成问题，就让执行处理的线程先等待，这就是Guarded Suspension模式。

该模式中存在一个持有状态的对象。该对象只有在自身的状态合适时，才会允许线程执行目标处理。

我们首先需要将对象的合适状态表示为“守护条件”，然后处理之前一定要去检查条件是否成立，只有成立才处理，不成立就等待。

Java中使用while语句来检查条件，使用wait方法来执行等待。当条件发生变化时，使用notify或notifyAll方法发出通知。



## 04 Balking模式

所谓Balk，就是“停止并返回”的意思。棒球中的“投手犯规”也是Balk这个词。

我们首先将对象的合适状态表示为守护条件。然后，在执行处理之前，检查守护条件是否成立。只有当守护条件成立时，对象才会继续执行处理；如果守护条件不成立，则不执行处理，立即从方法中返回。

Java中，我们使用if来检查守护条件，balk处理的执行则是使用return从方法中退出，或者使用throw抛出异常。守护条件检查处理则是使用synchronized放在临界区中。













