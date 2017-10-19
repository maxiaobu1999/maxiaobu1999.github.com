##设计模式总结
*   [工厂模式](http://blog.csdn.net/lovelion/article/details/17517213)
    * 抽象工厂模式的四个主要角色抽象工厂、具体工厂、抽象产品、具体产品。使用的时候只针对抽象的接口编程，而不需要
        关心创建的过程和具体的产品是什么。也因为这样添加新产品的不需要修改已有的代码，只需要创建新的具体工厂就
        可以，符合开闭原则；其他两种工厂模式就是对抽象工厂模式的省略，比方说我只有一个产品族，那么我就不需要抽象工厂了，只需要
        把工厂的创建方法用接口的形式抽出来交给客户端调用就可以了。工厂模式的局限性是不适合产品的结构的更改，比方法说设计的时候
        工厂创建两个按钮，现在我想再加一个，那么模式中所有的角色我都需要更改
    * 简单工厂模式
    * 工厂方法模式
    * 抽象工厂模式

*   [单例模式](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 只有一个实例，自行实例化并向系统提供实例，单例有7种的写发，关键的点是懒加载，线程安全以及加锁后带来的性能问题
        最好的方式应该枚举的写法，但我比较喜欢给instance加个volatile关键字，然后判null加锁的写法，"双重校验锁"
        
        
        public class Singleton {
            private volatile static Singleton instance; //声明成 volatile
            private Singleton (){}
        
            public static Singleton getSingleton() {
                if (instance == null) {                         
                    synchronized (Singleton.class) {
                        if (instance == null) {       
                            instance = new Singleton();
                        }
                    }
                }
                return instance;
            }
*   [建造者模式](http://blog.csdn.net/lovelion/article/details/17517213)
    * 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
    * 四种角色抽象建造者，具体建造者，指挥者，产品对象；由具体建造者去构建所需要的部件并提供返回产品对象的方法
        ，然后将抽象建造者传给指挥者，指挥者负责组装产品并返回组装好的产品对象。在建造者构建方法里可以返回个this，
        这样就可以链式调用，可读性能高一点
*   [适配器模式](http://blog.csdn.net/lovelion/article/details/17517213)
    * 三个角色Target（目标抽象类）、Adapter（适配器类）、Adaptee（被适配者类）；
        将一个接口转换成另外一个希望的另一个接口，使接口不兼容的那些类可以一起工作；
        android最典型的应用就是listView设置adapter，不过这个应用场景里没有需要转换的接口，
        而是要把数据转化成目标接口，这样数据就是被是被适配者，adapter也就是适配器，它把数据转成目标接口
        提供给listView使用。所以我个人的想法是当数据变化情况比较多的时候也可以使用设配器模式，
        这样可以数据与使用者分离开
*   [观察者模式](http://blog.csdn.net/lovelion/article/details/17517213)
    * 四个角色被观察者 、抽象被观察者、观察者、抽象被观察者
        目的定义一对多的依赖关系，被观察的事件发生改变，观察者都能收到通知
        抽象被观察者中用集合保存抽象的观察者，这个集合需要保证线程安全比如vector，当发生事件时一一调用观察者中的对应方法
        在util包里java提供了observer和Observable两实现类，分别充当抽象观察者和抽象被观察者
                

    
    
