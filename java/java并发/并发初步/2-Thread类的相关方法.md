

### Thread类的相关方法

```java
1：void    start( )
	1：启动线程；2：调用run( )方法

2:  run( )
	线程在被调度时执行的操作
		如果只调用 run() 方法，这将不会启动线程，只是普通的调用方法。

3:  getName( )
	获得当前线程的名字

4:  setName( )
	设置当前线程的名字

5:  currentThread( )
	静态方法，返回当前代码的线程
	static   Thread  currentThread( ) 

6:  yield( )
	释放当前执行的线程，不管当前线程的重要性以及执行的时间，会将执行机会让给优先级相同或更高的线程，如果在队列中没有同优先级的线程，忽略此方法
	static  void  yield( )

7:   join( )
	在线程  A  中调用   B   线程的join( ) 方法这会使得在join( )方法执行之后，将  B  线程执行完后再执行  A  线程
		低优先级的线程也可以获得执行
	join( )  本身会抛异常  ，利用try-catch-finally来解决

8:   startic   void   sleep( )(long  millis)   毫秒
	使线程在指定的时间放弃对CPU的使用，使得其他线程有机会被执行，时间到后会重新排队.
    --->抛出InterruptedException异常
        抛出的异常只能交给try-catch-final来解决，要用throws的话得在重写的run()方法出抛异常，但是在Thread这一类
        中的run()中并未有抛异常，要是在子类中抛了异常，就不再满足子类重写的方法不能比父类抛的异常大，此时父类中未抛异常，
        所以不能在重写的方法中抛异常

10:  bollean   isAlive( )
	判断线程是否还活着

11:  public   long   getId( )
	取得线程的唯一标识

改变线程当前的名称还可以使用建造构造器，从而在构造器的形参中将名称改正:    
    对于设置当前线程的名字，还可以时使用 在类中使用建造构造器来在创建 类的对象的时候对线程名进行初始化自定义。这里是调用了在父类中的构造器，要用到 super关键字
        
```

