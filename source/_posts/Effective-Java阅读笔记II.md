title: Effective-Java阅读笔记II
date: 2019-12-31 11:30:12
categories: 2019年12月
tags: [Java]

---

第二章阅读：创建和销毁对象
03.使用私有构造方法或枚类实现 Singleton 属性
这种方式类似于公共属性方法，但更简洁，无偿地提供了序列化机制，并提供了防止多个实例化的坚固保证，即使是在复杂的序列化或反射攻击的情况下。单一元素枚举类通常是实现单例的最佳方式。注意，如果单例必须继承 Enum 以外的父类 (尽管可以声明一个 Enum 来实现接口)，那么就不能使用这种方法。

04.使用私有构造器执行非实例化
（这两部分理解有困难，需要再查阅相关资料）
<!-- more -->

# 使用私有构造方法或枚类实现 Singleton 属性

单例是一个仅实例化一次的类，详细描述参见笔记：《设计模式面试高频题》。单例对象通常表示无状态对象，如函数 (条目 24) 或一个本质上唯一的系统组件。让一个类成为单例会使测试它的客户变得困难，因为除非实现一个作为它类型的接口，否则不可能用一个模拟实现替代单例。

　有两种常见的方法来实现单例。两者都基于保持构造方法私有和导出公共静态成员以提供对唯一实例的访问。在第一种方法中，成员是 final 修饰的属性：

    // Singleton with public final field
    public class Elvis {
        public static final Elvis INSTANCE = new Elvis();
        private Elvis() { ... }
        public void leaveTheBuilding() { ... }
    }

私有构造方法只调用一次，来初始化公共静态 final Elvis.INSTANCE 属性。缺少一个公共的或受保护的构造方法，保证了全局的唯一性：一旦 Elvis 类被初始化，一个 Elvis 的实例就会存在——不多也不少。客户端所做的任何事情都不能改变这一点，但需要注意的是：特权客户端可以使用 AccessibleObject.setAccessible 方法，以反射方式调用私有构造方法（详见第 65 条）。如果需要防御此攻击，请修改构造函数，使其在请求创建第二个实例时抛出异常。
　在第二个实现单例的方法中，公共成员是一个静态的工厂方法：

    // Singleton with static factory
    public class Elvis {
        private static final Elvis INSTANCE = new Elvis();
        private Elvis() { ... }
        public static Elvis getInstance() { return INSTANCE; }

        public void leaveTheBuilding() { ... }
    }

所有对 Elvis.getInstance 的调用都返回相同的对象引用，并且不会创建其他的 Elvis 实例（与前面提到的警告相同）。

　　公共属性方法的主要优点是 API 明确表示该类是一个单例：公共静态属性是 final 的，所以它总是包含相同的对象引用。 第二个好处是它更简单。

　　静态工厂方法的优势之一在于，它提供了灵活性：在不改变其 API 的前提下，我们可以改变该类是否应该为单例的想法。工厂方法返回该类的唯一实例，但是，它很容易被修改，比如，改为每个调用该方法的线程返回一个唯一的实例。 第二个好处是，如果你的应用程序需要它，可以编写一个泛型单例工厂（generic singleton factory ）（详见第30 条）。 使用静态工厂的最后一个优点是，可以通过方法引用（method reference）作为提供者，例如 Elvis::instance 等同于 Supplier<Elvis>。 除非满足以上任意一种优势，否则还是优先考虑公有域（public-field）的方法。

　　为了将上述方法中实现的单例类变成是可序列化的 （第 12 章），仅仅将 implements Serializable 添加到声明中是不够的。为了保证单例模式不被破坏，必须声明所有的实例字段为 transient，并提供一个 readResolve 方法（详见第 89 条）。否则，每当序列化的实例被反序列化时，就会创建一个新的实例，在我们的例子中，导致出现新的 Elvis 实例。为了防止这种情况发生，将如下的 readResolve 方法添加到 Elvis 类：

    // readResolve method to preserve singleton property
    private Object readResolve() {
         // Return the one true Elvis and let the garbage collector
         // take care of the Elvis impersonator.
        return INSTANCE;
    }
实现一个单例的第三种方法是声明单一元素的枚举类：

    // Enum singleton - the preferred approach
    public enum Elvis {
        INSTANCE;

        public void leaveTheBuilding() { ... }
    }

　这种方式类似于公共属性方法，但更简洁，无偿地提供了序列化机制，并提供了防止多个实例化的坚固保证，即使是在复杂的序列化或反射攻击的情况下。这种方法可能感觉有点不自然，但是 单一元素枚举类通常是实现单例的最佳方式。注意，如果单例必须继承 Enum 以外的父类 (尽管可以声明一个 Enum 来实现接口)，那么就不能使用这种方法。

# 使用私有构造器执行非实例化

试图通过创建抽象类来强制执行非实例化是行不通的。 该类可以被子类化，并且子类可以被实例化。此外，它误导用户认为该类是为继承而设计的（详见第 19 条）。不过，有一个简单的方法来确保非实例化。只有当类不包含显式构造器时，才会生成一个默认构造器，因此可以通过包含一个私有构造器来实现类的非实例化：

    // Noninstantiable utility class
    public class UtilityClass {
        // Suppress default constructor for noninstantiability
        private UtilityClass() {
            throw new AssertionError();
        }
        ... // Remainder omitted
    }


因为显式构造器是私有的，所以不可以在类的外部访问它。AssertionError 异常不是严格要求的，但是它可以避免不小心在类的内部调用构造器。它保证类在任何情况下都不会被实例化。这个习惯用法有点违反直觉，好像构造器就是设计成不能调用的一样。因此，如前面所示，添加注释是种明智的做法。

　　这种习惯有一个副作用，就是使得一个类不能子类化。所有的构造器都必须显式或隐式地调用父类构造器，而在这群情况下子类则没有可访问的父类构造器来调用。


# 参考资料：
【1】http://sjsdfg.gitee.io/effective-java-3rd-chinese/#/notes/03.%20%E4%BD%BF%E7%94%A8%E7%A7%81%E6%9C%89%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E6%88%96%E6%9E%9A%E7%B1%BB%E5%AE%9E%E7%8E%B0Singleton%E5%B1%9E%E6%80%A7
