# ThreadLocal 线程局部变量总结

解释：提供了线程内的局部变量，不同的线程之间不会相互干扰，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间的一些公共变量传递的复杂度，实现了线程之间的数据隔离

1 每个Thread线程内部都有一个map(ThreadLocal Map)

2 Map里面存储ThreadLocal对象的key和线程变量副本value

3 Thread内部的Map是由ThreadLocal维护的

4 对于不同的线程，每次获取到副本值时，别的线程并不能获取到当前线程的副本值，形成了副本的隔离，互不干扰

5 对于ThreadLocal对象的引用是一个弱引用,优点：每个map存储的Entry数量变少，Thread锁销毁时，ThreadLocalMap随之销毁，减少内存消耗