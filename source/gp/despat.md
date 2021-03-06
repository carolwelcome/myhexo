### 设计原则

#### 开闭原则
定义：一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。用抽象构建框架，用实现扩展细节
优点：提高软件系统的可复用性及可维护性

代码
```
Class Task{
	
	public void createTask(String name){
		if(name.equals("DAY")){
			System.out.println("创建每日任务");
		}else if(name.equals("MONTH")){
			System.out.println("创建每月任务");
		}else{
			System.out.println("创建年度任务");
		}
		//.....，以后想加还得在这儿改，烦死
	}
}

public interface Task{
	
	public void createTask();
}

```

#### 依赖倒置原则
定义：高层模块不应该依赖低层模块，二者都应该依赖其抽象 ；
抽象不应该依赖细节，细节应该依赖抽象
针对接口编程，不要针对实现编程

优点：可以减少类间的耦合性、提高系统稳定性，提高代码可读性要可维护性，可降低修改程序所造成的风险

代码
```


```

#### 单一职责原则
定义：不要存在多于一个导致类变更的原因
一个类、接口、方法只负责一项职责

优点：降低类的复杂度
提高类的可读性
提高系统 的可维护性
降低变更引起的风险

代码

```

```

#### 接口隔离原则 

定义：用多个专门的接口，而不是使用单一的总接口，客户端不应该依赖它不需要接口

注意：一个类对应一个类的依赖应该建立在最小的接口上
建立单一接口，不要建立 庞大臃肿的接口
尽量细化接口，接口中方法尽量少
注意适度原则 ，一定要适度

优点：符合我们常说的高内聚，低耦合的设计思想
从而使得类具有很的可读性、可扩展性和可维护性

代码
```
动物，
```

#### 迪米特法则
定义：一个对象应该对其他对象保持最少的了解。最少知道原则 
尽量降低类与类之间耦合
优点：降低类之间的耦合，
强调只和朋友 交流，不和陌生人说话
朋友：出现在成员变量、方法的输入、输出 参数的类成为成员朋友 类，而出现在方法体内部的类不属于朋友 类

#### 里氏替换原则
定义：如果对每一个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都替换成o2，程序P的行为没有发生变化 ，那么类型T2是类型T1的子类型。
定义扩展：一个软件实体如果适用一个父类的话，那一定适用于其子类，所有引用父类的地方必须 能透明的使用其子类的对象，子类对象能够替换父类对象，而程序的逻辑不变。

引申意义：子类可以扩展父类的功能，但不能改变父类原有的功能
子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法。
子类中可以增加自己特有的方法
当子类的方法重载父类的方法时，方法的前置条件（即方法的输入、入参）要比父类方法的输入参数更宽松
当子类的方法实现父类的方法时（重写、重载或实现抽象方法）方法的后置条件（即输出、返回值）要比父类更为严格或相等

优点：
约束继承泛滥，开闭原则 的一种体现
加强程序的健壮性，同时变更 时可以做非常好的兼容性，提高程序 的维护性，扩展性，降低需求变更 时引入 的风险


#### 合成利用原则 

定义：尽量使用对象组合、聚合，而不是继承关系达到软件复用的目的
聚合：has-a 组合 contains-a
继承 is -a


优点：可以使系统更加灵活，降低类与类之间的耦合度，一个类的变化 对其他类造成的影响相对较少


生命周期与组合的

什么时候用聚合、组合、继承 
继承 ：关联关系非常强，（狗与动物），顶层抽象
组合：独立的物体，合到一起之后，形成了一个具有相同生命周期的设计（人体与四肢的关系，电脑与U盘是聚合关系）
聚合：不具有相同生命周期组合。dao,数据库连接关联
