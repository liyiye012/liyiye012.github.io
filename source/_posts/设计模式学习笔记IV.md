---
title: 设计模式学习笔记IV
date: 2020-1-3 15:55:12
categories: 2020年1月
tags: [Design Pattern，Java]

---

建造者（Builder）模式：
经典Builder模式；
变种Builder模式；

<!-- more -->

建造者模式是日常开发中比较常见的设计模式，它的主要作用就是将复杂事物创建的过程抽象出来，该抽象的不同实现方式不同，创建出的对象也不同。通俗的讲，创建一个对象一般都会有一个固定的步骤，这个固定的步骤我们把它抽象出来，每个抽象步骤都会有不同的实现方式，不同的实现方式创建出的对象也将不同。举个常见的例子，想必大家都买过电脑，电脑的生产或者组装其实就是属于建造者模式，我们知道，电脑的生产都需要安装CPU、内存条、硬盘等元器件。我们可以把这个安装步骤抽象出来，至于到底装哪种CPU，比如i5还是i7就是对该抽象安装步骤的具体实现。

建造者模式分为两种，一种为经典建造者模式，另一种为变种建造者模式。我们来挨个看下：

# 经典Builder模式
经典Buider模式中有四个角色：

要建造的产品Product -- 组装的电脑
抽象的Builder -- 装CPU、内存条、硬盘等抽象的步骤
Builder的具体实现ConcreteBuilder -- 对上述抽象步骤的实现，比如装i5CPU、8G内存条、1T硬盘
使用者Director -- 电脑装机人员
接下来我们来看下用代码如何实现上述组装电脑的过程：
## 首先我们先来创建一个Computer类：

    public class Computer {
        /*CPU*/
        private String CPU;
        /*内存*/
        private String memory;
        /*硬盘*/
        private String hardDisk;
        /*键盘*/
        private String keyboard;
        /*鼠标*/
        private String mouse;

        public String getCPU() {
            return CPU;
        }

        public void setCPU(String CPU) {
            this.CPU = CPU;
        }

        public String getMemory() {
            return memory;
        }

        public void setMemory(String memory) {
            this.memory = memory;
        }

        public String getHardDisk() {
            return hardDisk;
        }

        public void setHardDisk(String hardDisk) {
            this.hardDisk = hardDisk;
        }

        public String getKeyboard() {
            return keyboard;
        }

        public void setKeyboard(String keyboard) {
            this.keyboard = keyboard;
        }

        public String getMouse() {
            return mouse;
        }

        public void setMouse(String mouse) {
            this.mouse = mouse;
        }

        @Override
        public String toString() {
            return "Computer{" +
                    "CPU='" + CPU + '\'' +
                    ", memory='" + memory + '\'' +
                    ", hardDisk='" + hardDisk + '\'' +
                    ", keyboard='" + keyboard + '\'' +
                    ", mouse='" + mouse + '\'' +
                    '}';
        }
    }
很简单，可以看到这个Computer类中有五个基本属性CPU、内存条、硬盘、键盘和鼠标，然后还有一个toString方法，用于之后方便打印信息用。

## 接下来我们来创建一个抽象的电脑组装过程的Builder类：

    public interface ComputerConfigBuilder {
        void setCPU();
        void setMemery();
        void setHardDisk();
        void setKeyboard();
        void setMouse();
        Computer getComputer();
    }

电脑组装一般都需要安装CPU、内存条、硬盘、键盘鼠标等，我们把这一安装过程给抽象出来，也就是这里的ComputerConfigBuilder ，至于具体安装什么需要其实现类来实现，另外其中还定义了一个获取Conputer的方法。

## 创建具体的实现类
好了，有了抽象的组装过程，接下来我们就需要创建具体的实现类。我们知道电脑一般都有低配版和高配版，不同配置，组装成的电脑自然就不一样。接下我们首先来创建一个低配版的套餐LowConfigBuilder ，让其实现ComputerConfigBuilder：

    public class LowConfigBuilder implements ComputerConfigBuilder {

        private Computer mComputer;

        public LowConfigBuilder(){
            this.mComputer = new Computer();
        }

        @Override
        public void setCPU() {
            mComputer.setCPU("i5");
        }

        @Override
        public void setMemery() {
            mComputer.setMemory("8G");
        }

        @Override
        public void setHardDisk() {
            mComputer.setHardDisk("500G");
        }

        @Override
        public void setKeyboard() {
            mComputer.setKeyboard("薄膜键盘");
        }

        @Override
        public void setMouse() {
            mComputer.setMouse("有线鼠标");
        }

        @Override
        public Computer getComputer() {
            return mComputer;
        }
    }

可以看到这个低配版的配置为：i5的CPU、8G内存、500G硬盘、薄膜键盘和有线鼠标。

接着我们再创建一个高配版的套餐：

    public class HighConfigBuider implements ComputerConfigBuilder {

        private Computer mComputer;

        public HighConfigBuider(){
            this.mComputer = new Computer();
        }

        @Override
        public void setCPU() {
            mComputer.setCPU("i7");
        }

        @Override
        public void setMemery() {
            mComputer.setMemory("16G");
        }

        @Override
        public void setHardDisk() {
            mComputer.setHardDisk("1T");
        }

        @Override
        public void setKeyboard() {
            mComputer.setKeyboard("机械键盘");
        }

        @Override
        public void setMouse() {
            mComputer.setMouse("无线鼠标");
        }

        @Override
        public Computer getComputer() {
            return mComputer;
        }
    }
可以看到这个高配版的配置为：i7的CPU、16G内存、1T硬盘、机械键盘和无线鼠标。

## 上面我们已经定义好了两种配置方案，接下我们还需要一名装机人员Director：

    public class Director {
        private ComputerConfigBuilder mBuilder;
        public void setBuilder(ComputerConfigBuilder builder){
            this.mBuilder = builder;
        }
        public void createComputer(){
            mBuilder.setCPU();
            mBuilder.setMemery();
            mBuilder.setHardDisk();
            mBuilder.setKeyboard();
            mBuilder.setMouse();
        }
        public Computer getComputer(){
            return mBuilder.getComputer();
        }
    }

我们需要通过setBuilder来告诉他电脑需要什么配置，然后就可以通过createComputer来一步步组装电脑，组装完之后就可以调用getComputer方法来获取我们需要的电脑啦。
## 演示
接下来我们就来创建一台电脑试下，首先我们先创建一个
低配版的：

    Director director = new Director();//创建装机人员
    director.setBuilder(new LowConfigBuilder()); //告诉装机人员电脑配置，这里为低配版
    director.createComputer(); //装机人员开始组装
    Computer computer = director.getComputer(); //从装机人员获取组装好的电脑
    System.out.print("电脑配置：" + computer.toString());  //查看电脑配置
    --------------------------------------
    输出结果：
    电脑配置：Computer{CPU='i5', memory='8G', hardDisk='500G', keyboard='薄膜键盘', mouse='有线鼠标'}
    --------------------------------------
# 变种Builder模式

需要创建一个不可变的Person对象，这个Person可以拥有以下几个属性：名字、性别、年龄、职业、车、鞋子、衣服、钱、房子。其中名字和性别是必须有的。

我们首先想到的是给出以下的一个符合要求的Person类

    public class Person {
        /*名字（必须）*/
        private final String name;
        /*性别（必须）*/
        private final String gender;
        /*年龄（非必须）*/
        private final String age;
        /*鞋子（非必须）*/
        private final String shoes;
        /*衣服（非必须）*/
        private final String clothes;
        /*钱（非必须）*/
        private final String money;
        /*房子（非必须）*/
        private final String house;
        /*汽车（非必须）*/
        private final String car;
        /*职业（非必须）*/
        private final String career;

        public Person(String name,String gender,String age,String shoes,String clothes,String money,String house,String car,String career){
            this.name = name;
            this.gender = gender;
            this.age = age;
            this.shoes = shoes;
            this.clothes = clothes;
            this.money = money;
            this.house = house;
            this.car = car;
            this.career = career;
        }

        public Person(String name, String gender){
            this(name,gender,null,null,null,null,null,null,null);
        }

    }

由于要创建出的Person对象是不可变的，所以将类中的属性都声明为final的，然后定义了一个参数为所有属性的构造方法，又因为name和gender为必须项，所以为了调用者方便又单独定义了一个参数为name和gender的构造方法。这样Person类就好了，但这样存在一个问题，就是如果需要传入非必须属性的时候，这个构造方法调用起来不是很方便，因为这个构造方法参数太多了，很容易传错。所以我们改用set方法设置：

    public class Person {
        /*名字（必须）*/
        private String name;
        /*性别（必须）*/
        private String gender;
        /*年龄（非必须）*/
        private String age;
        /*鞋子（非必须）*/
        private String shoes;
        /*衣服（非必须）*/
        private String clothes;
        /*钱（非必须）*/
        private String money;
        /*房子（非必须）*/
        private String house;
        /*汽车（非必须）*/
        private String car;
        /*职业（非必须）*/
        private String career;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getGender() {
            return gender;
        }

        public void setGender(String gender) {
            this.gender = gender;
        }

        public String getAge() {
            return age;
        }

        public void setAge(String age) {
            this.age = age;
        }

        public String getShoes() {
            return shoes;
        }

        public void setShoes(String shoes) {
            this.shoes = shoes;
        }

        ......

    }

如果要创建对象的话只用如下操作就行了：

    Person person = new Person();
    person.setName("张三");
    person.setAge("22");
    person.setGender("男");
    person.setCareer("程序员");
    ......


这样看上去比较清晰了，只要创建一个对象，想要赋什么值set上去就可以了，不过你细细看了下，还是发现了不少的问题的，首先用这个set方法，违背了刚开始这个对象不可变的需求，其次用这种set方法一条一条赋值，很可能会得到一个不完整的Person对象，因为当建完了Person对象，可能出于各方面的原因有些信息忘记set了，那么得到的Person对象就不是预期的对象。这时我们只能用下面这种变种的Builder模式：

    public class Person {
        /*名字（必须）*/
        private final String name;
        /*性别（必须）*/
        private final String gender;
        /*年龄（非必须）*/
        private final String age;
        /*鞋子（非必须）*/
        private final String shoes;
        /*衣服（非必须）*/
        private final String clothes;
        /*钱（非必须）*/
        private final String money;
        /*房子（非必须）*/
        private final String house;
        /*汽车（非必须）*/
        private final String car;
        /*职业（非必须）*/
        private final String career;


        private Person(Builder builder) {
            this.name = builder.name;
            this.gender = builder.gender;
            this.age = builder.age;
            this.shoes = builder.shoes;
            this.clothes = builder.clothes;
            this.money = builder.money;
            this.house = builder.house;
            this.car = builder.car;
            this.career = builder.career;
        }

        public static class Builder {
            private final String name;
            private final String gender;
            private String age;
            private String shoes;
            private String clothes;
            private String money;
            private String house;
            private String car;
            private String career;

            public Builder(String name,String gender) {
                this.name = name;
                this.gender = gender;
            }

            public Builder age(String age) {
                this.age = age;
                return this;
            }

            public Builder car(String car) {
                this.car = car;
                return this;
            }

            public Builder shoes(String shoes) {
                this.shoes = shoes;
                return this;
            }

            public Builder clothes(String clothes) {
                this.clothes = clothes;
                return this;
            }

            public Builder money(String money) {
                this.money = money;
                return this;
            }

            public Builder house(String house) {
                this.house = house;
                return this;
            }

            public Builder career(String career) {
                this.career = career;
                return this;
            }

            public Person build(){
                return new Person(this);
            }
        }
由于这个Person对象是不可变的，所以毫无疑问我们给他的所有属性都加了final修饰，当然如果没有不可变的需求也是可以不加的，然后在Person类中定义一个内部类Builder，这个Builder内部类中的属性要和Person中的相同，并且必须有的属性要用final修饰，防止这些属性没有被赋值，其他非必须的属性不能用final，因为如果加了final，就必须对其进行初始化，这样这些非必须的属性又变成必须的。然后内部类中定义了一个构造方法，传入必须有的属性。其他非必须的属性都通过方法设置，每个方法都返回Builder对象自身。最后定义了一个build方法，将Builder对象传入Person的私有构造方法，最终返回一个对象。

接下来我们来看下Person的创建：

        Person person = new Person.Builder("张三","男")
                .age("12")
                .money("1000000")
                .car("宝马")
                .build();

非必须的属性可以根据需要任意设置，非常灵活，而且这样先设置属性再创建对象，最终获取的对象一定是你预期的完整对象，不会像用之前set的方法创建的对象可能还没有设置完全。

# 参考资料
【1】https://www.jianshu.com/p/afe090b2e19c
