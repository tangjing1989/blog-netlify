title: JAVA设计模式-创建-单例
author: TangJing
date: 2019-03-05 10:35:41
tags:
---
````
/**
 * @author tang
 * @version 1.0
 * @date 2019-03-04 11:07
 * singleton mode (4 ways to do it)
 */
public class Singleton {

    /** 懒汉模式 */
    LazySingletion lazySingletion = LazySingletion.getInstance();
    /** 饿汉模式 */
    HungrySingletion hungrySingletion = HungrySingletion.getInstance();
    /** 静态内部类 */
    StaticInnerSingletion staticInnerSingletion = StaticInnerSingletion.getInstance();
    /** 静态内部类 */
    EnumSingleton enumSingleton = EnumSingleton.getInstance();
}

/**
 * 懒汉模式
 */
class LazySingletion {

    private static final LazySingletion instance = new LazySingletion();

    static LazySingletion getInstance() {
        return instance;
    }
}

/**
 * 饿汉模式 双重检查锁
 *
 * 一定要加 volatile 因为有指令重排的坑
 *
 * 创建对象的过程 1.allocate 分配内存-> 2.初始化对象 -> 3.设置对象指向刚分配的内存 2 3 可能顺序反了 可能会出现 对象初始化2次
 */
class HungrySingletion {

    private static volatile HungrySingletion hungrySingletion;

    static HungrySingletion getInstance() {
        if (hungrySingletion == null) {
            synchronized (HungrySingletion.class) {
                hungrySingletion = new HungrySingletion();
            }
        }
        return hungrySingletion;
    }
}

/**
 * 静态内部类 虚拟机会保证一个类的<clinit>()方法在多线程环境中被正确地加锁、同步 同一个加载器下，一个类型只会初始化一次
 */
class StaticInnerSingletion {

    public static class InnerSingletion {

        static StaticInnerSingletion instance = new StaticInnerSingletion();
    }

    static StaticInnerSingletion getInstance() {
        return InnerSingletion.instance;
    }
}

/**
 * 枚举单例
 */
class EnumSingleton {

    public static EnumSingleton getInstance() {
        return Singleton.INSTANCE.getInstance();
    }

    private static enum Singleton {
        INSTANCE;
        private EnumSingleton singleton;

        //JVM会保证此方法绝对只调用一次
        private Singleton() {
            singleton = new EnumSingleton();
        }

        public EnumSingleton getInstance() {
            return singleton;
        }
    }
}
````