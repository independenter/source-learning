## Design Learn
本文档记录对于设计模式的学习,文档需要的图片资源可以发邮件给m15501951002@163.com，或者到src下面获得。

:100:鸣谢
- 感谢学习本笔记的每一个观众
- 感谢修改本笔记的每一个贡献者

## 目录
- [设计模式六大原则](#设计模式六大原则)
- [设计模式法则](#设计模式法则)
- 23种设计模式
  - [创建型](#创建型)
    - [建造者](#建造者)
	- [原型](#原型)
	- [单例](#单例)
	- [工厂方法](#工厂方法)
	- [抽象工厂](#抽象工厂)
  - [结构型](#结构型)
    - [组合](#组合)
	- [装饰](#装饰)
	- [外观](#外观)
	- [享元](#享元)
	- [代理](代理)
	- [适配器](#适配器)
	- [桥接](#桥接)
  - [行为型](#行为型)
    - [策略模式](#策略模式)
	- [模板方法](#模板方法)
	- [责任链模式](#责任链模式)
	- [迭代器模式](#迭代器模式)
	- [解释器模式](#解释器模式)
	- [命令模式](#命令模式)
	- [备忘录模式](#备忘录模式)
	- [状态模式](#状态模式)
	- [中介者模式](#中介者模式)
	- [观察者](#观察者)
	- [访问者模式](#访问者模式)

## 设计模式六大原则
1：单一职责,一个类只能干一件事情
2：里式替换原则,任何基类可以出现的地方，子类一定可以出现
3：依赖倒置原则，面向接口编程，依赖于抽象而不依赖于具体
4：接口隔离原则，每个接口中不存在子类用不到却必须实现的方法
5：开闭原则，对扩展开放，对修改关闭（不去修改代码，而是扩展原有代码） 
6：合成复用原则，尽量首先使用合成/聚合的关系，而不是使用继承
## 设计模式法则
迪米特法则，一个类对自己依赖的类知道的越少越好

## 创建型

### 建造者
:star: 将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。
```
package javat.create.build;
import java.awt.*;
import javax.swing.*;
public class ParlourDecorator
{
    public static void main(String[] args)
    {
        try
        {
            Decorator d;
            d=(Decorator) ReadXML.getObject();
            ProjectManager m=new ProjectManager(d);       
            Parlour p=m.decorate();
            p.show();
        }
        catch(Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
//产品：客厅
class Parlour
{
    private String wall;    //墙
    private String TV;    //电视
    private String sofa;    //沙发  
    public void setWall(String wall)
    {
        this.wall=wall;
    }
    public void setTV(String TV)
    {
        this.TV=TV;
    }
    public void setSofa(String sofa)
    {
        this.sofa=sofa;
    }   
    public void show()
    {
        JFrame jf=new JFrame("建造者模式测试");
        Container contentPane=jf.getContentPane();
        JPanel p=new JPanel();   
        JScrollPane sp=new JScrollPane(p);  
        String parlour=wall+TV+sofa;
        JLabel l=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\Builder/"+parlour+".jpg"));
        //JLabel l=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\Builder\\w1TV1sf1.jpg"));
        p.setLayout(new GridLayout(1,1));
        p.setBorder(BorderFactory.createTitledBorder("客厅"));
        p.add(l);
        contentPane.add(sp,BorderLayout.CENTER);       
        jf.pack();  
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }   
}
//抽象建造者：装修工人
abstract class Decorator
{
    //创建产品对象
    protected  Parlour product=new Parlour();
    public  abstract void buildWall();
    public  abstract void buildTV();
    public  abstract void buildSofa();
    //返回产品对象
    public  Parlour getResult()
    {
        return  product;
    }
}
//具体建造者：具体装修工人1
class ConcreteDecorator1  extends Decorator
{
    public void buildWall()
    {
        product.setWall("w1");
    }
    public void buildTV()
    {
        product.setTV("TV1");
    }
    public void buildSofa()
    {
        product.setSofa("sf1");
    }
}
//具体建造者：具体装修工人2
class ConcreteDecorator2 extends Decorator
{
    public void buildWall()
    {
        product.setWall("w2");
      }
      public void buildTV()
      {
          product.setTV("TV2");
      }
      public void buildSofa()
      {
          product.setSofa("sf2");
      }
}
//指挥者：项目经理
class ProjectManager
{
    private Decorator builder;
    public ProjectManager(Decorator builder)
    {
          this.builder=builder;
    }
    //产品构建与组装方法
    public Parlour decorate()
    {
          builder.buildWall();
        builder.buildTV();
        builder.buildSofa();
        return builder.getResult();
    }
}

package javat.create.build;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML
{
    public static Object getObject()
    {
        try
        {
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;                           
            doc=builder.parse(new File("E:\\workspacecrm\\myboss\\src\\Builder/config.xml"));
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName="javat.create.build."+classNode.getNodeValue();
            System.out.println("新类名："+cName);
            Class<?> c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
         }  
         catch(Exception e)
         {
                   e.printStackTrace();
                   return null;
         }
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/create-build.png)

### 原型
:star: 用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
* 1.浅拷贝拷贝外层对象，对象里面的引用对象不进行拷贝。
* 2.深拷贝需要进行内部的拷贝（人为进行拷贝）。
```
package javat.create.proto;

import java.awt.*;
import javax.swing.*;
class SunWukong extends JPanel implements Cloneable
{
    private static final long serialVersionUID = 5543049531872119328L;
    public SunWukong()
    {
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\proto/Wukong.jpg"));
        this.add(l1);   
    }
    public Object clone()
    {
        SunWukong w=null;
        try
        {
            w=(SunWukong)super.clone();
        }
        catch(CloneNotSupportedException e)
        {
            System.out.println("拷贝悟空失败!");
        }
        return w;
    }
}
public class ProtoTypeWukong
{
    public static void main(String[] args)
    {
        JFrame jf=new JFrame("原型模式测试");
        jf.setLayout(new GridLayout(1,2));
        Container contentPane=jf.getContentPane();
        SunWukong obj1=new SunWukong();
        contentPane.add(obj1);       
        SunWukong obj2=(SunWukong)obj1.clone();
        contentPane.add(obj2);   
        jf.pack();       
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);   
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/create-proto.png)

### 单例
:star: 保证一个类仅有一个实例，并提供一个访问它的全局访问点。多线程模式下需要双重检查
```
package javat.create.singleton;

import java.awt.*;
import javax.swing.*;
public class SingletonEager
{
    public static void main(String[] args)
    {
        JFrame jf=new JFrame("饿汉单例模式测试");
        jf.setLayout(new GridLayout(1,2));
        Container contentPane=jf.getContentPane();
        Bajie obj1=Bajie.getInstance();
        contentPane.add(obj1);    
        Bajie obj2=Bajie.getInstance(); 
        contentPane.add(obj2);
        if(obj1==obj2)
        {
            System.out.println("他们是同一人！");
        }
        else
        {
            System.out.println("他们不是同一人！");
        }   
        jf.pack();       
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
class Bajie extends JPanel
{
    private static Bajie instance=new Bajie();
    private Bajie()
    { 
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\singleton/Bajie.jpg"));
        this.add(l1);   
    }
    public static Bajie getInstance()
    {
        return instance;
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/create-singleton.png)

### 工厂方法
:star: 定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。
```
package javat.create.FactoryMethod;
import java.awt.*;
import javax.swing.*;
public class AnimalFarmTest
{
    public static void main(String[] args)
    {
        try
        {
            Animal a;
            AnimalFarm af;
            af=(AnimalFarm) ReadXML2.getObject();
            a=af.newAnimal();
            a.show();
        }
        catch(Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
//抽象产品：动物类
interface Animal
{
    public void show();
}
//具体产品：马类
class Horse implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("工厂方法模式测试");
    public Horse()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：马"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\FactoryMethod/A_Horse.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    //用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//具体产品：牛类
class Cattle implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("工厂方法模式测试");
    public Cattle()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：牛"));
        sp=new JScrollPane(p1);
        contentPane.add(sp,BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\FactoryMethod/A_Cattle.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    //用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//抽象工厂：畜牧场
interface AnimalFarm
{
    public Animal newAnimal();
}
//具体工厂：养马场
class HorseFarm implements AnimalFarm
{
    public Animal newAnimal()
    {
        System.out.println("新马出生！");
        return new Horse();
    }
}
//具体工厂：养牛场
class CattleFarm implements AnimalFarm
{
    public Animal newAnimal()
    {
        System.out.println("新牛出生！");
        return new Cattle();
    }
}
package javat.create.FactoryMethod;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML2
{
    public static Object getObject()
    {
        try
        {
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;                           
            doc=builder.parse(new File("E:\\workspacecrm\\myboss\\src\\FactoryMethod/config2.xml"));
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName="javat.create.FactoryMethod."+classNode.getNodeValue();
            System.out.println("新类名："+cName);
            Class<?> c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
        }  
        catch(Exception e)
        {
               e.printStackTrace();
               return null;
        }
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/create-factoryMethod.png)

### 抽象工厂
:star: 定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。
```
package javat.create.AbstractFactory;
import java.awt.*;
import javax.swing.*;
public class FarmTest
{
    public static void main(String[] args)
    {
        try
        {          
            Farm f;
            Animal a;
            Plant p;
            f=(Farm) ReadXML.getObject();
            a=f.newAnimal();
            p=f.newPlant();
            a.show();
            p.show();
        }
        catch(Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
//抽象产品：动物类
interface Animal
{
    public void show();
}
//具体产品：马类
class Horse implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("抽象工厂模式测试");
    public Horse()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：马"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\FactoryMethod/A_Horse.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//具体产品：牛类
class Cattle implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("抽象工厂模式测试");
    public Cattle() {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：牛"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\abstractFactory/A_Cattle.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//抽象产品：植物类
interface Plant
{
    public void show();
}
//具体产品：水果类
class Fruitage implements Plant
{
    JScrollPane sp;
    JFrame jf=new JFrame("抽象工厂模式测试");
    public Fruitage()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("植物：水果"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\abstractFactory/P_Fruitage.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//具体产品：蔬菜类
class Vegetables implements Plant
{
    JScrollPane sp;
    JFrame jf=new JFrame("抽象工厂模式测试");
    public Vegetables()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("植物：蔬菜"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\abstractFactory/P_Vegetables.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//抽象工厂：农场类
interface Farm
{
    public Animal newAnimal();
    public Plant newPlant();
}
//具体工厂：韶关农场类
class SGfarm implements Farm
{
    public Animal newAnimal()
    {
        System.out.println("新牛出生！");
        return new Cattle();
    }
    public Plant newPlant()
    {
        System.out.println("蔬菜长成！");
        return new Vegetables();
    }
}
//具体工厂：上饶农场类
class SRfarm implements Farm
{
    public Animal newAnimal()
    {
        System.out.println("新马出生！");
        return new Horse();
    }
    public Plant newPlant()
    {
        System.out.println("水果长成！");
        return new Fruitage();
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/create-abstractFactory.png)

## 结构型

### 组合
:star: 
```
```

### 装饰
:star: 
```
```

### 外观
:star: 
```
```

### 享元
:star: 
```
```

### 代理
:star: 
```
```

### 适配器
:star: 
```
```

### 桥接
:star: 
```
```

## 行为型

### 策略模式
:star: 
```
```

### 模板方法
:star: 
```
```

### 责任链模式
:star: 
```
```

### 迭代器模式
:star: 
```
```

### 解释器模式
:star: 
```
```

### 命令模式
:star: 
```
```

### 备忘录模式
:star: 
```
```

### 状态模式
:star: 
```
```

### 中介者模式
:star: 
```
```

### 观察者
:star: 
```
```

### 访问者模式
:star: 
```
```