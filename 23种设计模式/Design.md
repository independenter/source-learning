# Design Learn
本文档记录对于设计模式的学习,文档需要的图片资源可以发邮件给m15501951002@163.com，或者到src下面获得。

:100:鸣谢
- 感谢[C语言中文网](http://c.biancheng.net/view/1317.html)的案例
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
	- [代理](#代理)
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
:star: 将对象组合成树形结构以表示"部分-整体"的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
```
package javat.structure.composite;

import java.util.ArrayList;
public class ShoppingTest
{
    public static void main(String[] args)
    {
        float s=0;
        Bags BigBag,mediumBag,smallRedBag,smallWhiteBag;
        Goods sp;
        BigBag=new Bags("大袋子");
        mediumBag=new Bags("中袋子");
        smallRedBag=new Bags("红色小袋子");
        smallWhiteBag=new Bags("白色小袋子");               
        sp=new Goods("婺源特产",2,7.9f);
        smallRedBag.add(sp);
        sp=new Goods("婺源地图",1,9.9f);
        smallRedBag.add(sp);       
        sp=new Goods("韶关香菇",2,68);
        smallWhiteBag.add(sp);
        sp=new Goods("韶关红茶",3,180);
        smallWhiteBag.add(sp);       
        sp=new Goods("景德镇瓷器",1,380);
        mediumBag.add(sp);
        mediumBag.add(smallRedBag);       
        sp=new Goods("李宁牌运动鞋",1,198);
        BigBag.add(sp);
        BigBag.add(smallWhiteBag);
        BigBag.add(mediumBag);
        System.out.println("您选购的商品有：");
        BigBag.show();
        s=BigBag.calculation();       
        System.out.println("要支付的总价是："+s+"元");
    }
}
//抽象构件：物品
interface Articles
{
    public float calculation(); //计算
    public void show();
}
//树叶构件：商品
class Goods implements Articles
{
    private String name;     //名字
    private int quantity;    //数量
    private float unitPrice; //单价
    public Goods(String name,int quantity,float unitPrice)
    {
        this.name=name;
        this.quantity=quantity;
        this.unitPrice=unitPrice;
    }   
    public float calculation()
    {
        return quantity*unitPrice; 
    }
    public void show()
    {
        System.out.println(name+"(数量："+quantity+"，单价："+unitPrice+"元)");
    }
}
//树枝构件：袋子
class Bags implements Articles
{
    private String name;     //名字   
    private ArrayList<Articles> bags=new ArrayList<Articles>();   
    public Bags(String name)
    {
        this.name=name;       
    }
    public void add(Articles c)
    {
        bags.add(c);
    }   
    public void remove(Articles c)
    {
        bags.remove(c);
    }   
    public Articles getChild(int i)
    {
        return bags.get(i);
    }   
    public float calculation()
    {
        float s=0;
        for(Object obj:bags)
        {
            s+=((Articles)obj).calculation();
        }
        return s;
    }
    public void show()
    {
        for(Object obj:bags)
        {
            ((Articles)obj).show();
        }
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-composite.png)

### 装饰
:star: 动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
```
package javat.structure.decorator;
//装饰器模式
public class DecoratorPattern
{
    public static void main(String[] args)
    {
        Component p=new ConcreteComponent();
        p.operation();
        System.out.println("---------------------------------");
        Component d=new ConcreteDecorator(p);
        d.operation();
    }
}
//抽象构件角色
interface  Component
{
    public void operation();
}
//具体构件角色
class ConcreteComponent implements Component
{
    public ConcreteComponent()
    {
        System.out.println("创建具体构件角色");
    }
    public void operation()
    {
        System.out.println("调用具体构件角色的方法operation()");
    }
}
//抽象装饰角色
class Decorator implements Component
{
    private Component component;
    public Decorator(Component component)
    {
        this.component=component;
    }
    public void operation()
    {
        component.operation();
    }
}
//具体装饰角色
class ConcreteDecorator extends Decorator
{
    public ConcreteDecorator(Component component)
    {
        super(component);
    }
    public void operation()
    {
        super.operation();
        addedFunction();
    }
    public void addedFunction()
    {
        System.out.println("为具体构件角色增加额外的功能addedFunction()");
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-decorator.png)

### 外观
:star: 为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
```
package javat.structure.facade;
//外观模式
import java.awt.*;
import javax.swing.*;
import javax.swing.event.*;
import javax.swing.tree.DefaultMutableTreeNode;
public class WySpecialtyFacade
{
    public static void main(String[] args)
    {
        JFrame f=new JFrame ("外观模式: 婺源特产选择测试");
        Container cp=f.getContentPane();
        WySpecialty wys=new WySpecialty();
        JScrollPane treeView=new JScrollPane(wys.tree);
        JScrollPane scrollpane=new JScrollPane(wys.label);
        JSplitPane splitpane=new JSplitPane(JSplitPane.HORIZONTAL_SPLIT,true,treeView,scrollpane); //分割面版
        splitpane.setDividerLocation(230);     //设置splitpane的分隔线位置
        splitpane.setOneTouchExpandable(true); //设置splitpane可以展开或收起                       
        cp.add(splitpane);
        f.setSize(650,350);
        f.setVisible(true);
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
class WySpecialty extends JPanel implements TreeSelectionListener
{
    private static final long serialVersionUID=1L;
    final JTree tree;
    JLabel label;
    private Specialty1 s1=new Specialty1();
    private Specialty2 s2=new Specialty2();
    private Specialty3 s3=new Specialty3();
    private Specialty4 s4=new Specialty4();
    private Specialty5 s5=new Specialty5();
    private Specialty6 s6=new Specialty6();
    private Specialty7 s7=new Specialty7();
    private Specialty8 s8=new Specialty8();
    WySpecialty(){
        DefaultMutableTreeNode top=new DefaultMutableTreeNode("婺源特产");
        DefaultMutableTreeNode node1=null,node2=null,tempNode=null;
        node1=new DefaultMutableTreeNode("婺源四大特产（红、绿、黑、白）");
        tempNode=new DefaultMutableTreeNode("婺源荷包红鲤鱼");
        node1.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源绿茶");
        node1.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源龙尾砚");
        node1.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源江湾雪梨");
        node1.add(tempNode);
        top.add(node1);
        node2=new DefaultMutableTreeNode("婺源其它土特产");
        tempNode=new DefaultMutableTreeNode("婺源酒糟鱼");
        node2.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源糟米子糕");
        node2.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源清明果");
        node2.add(tempNode);
        tempNode=new DefaultMutableTreeNode("婺源油煎灯");
        node2.add(tempNode);
        top.add(node2);
        tree=new JTree(top);
        tree.addTreeSelectionListener(this);
        label=new JLabel();
    }
    public void valueChanged(TreeSelectionEvent e)
    {
        if(e.getSource()==tree)
        {
            DefaultMutableTreeNode node=(DefaultMutableTreeNode) tree.getLastSelectedPathComponent();
            if(node==null) return;
            if(node.isLeaf())
            {
                Object object=node.getUserObject();
                String sele=object.toString();
                label.setText(sele);
                label.setHorizontalTextPosition(JLabel.CENTER);
                label.setVerticalTextPosition(JLabel.BOTTOM);
                sele=sele.substring(2,4);
                if(sele.equalsIgnoreCase("荷包")) label.setIcon(s1);
                else if(sele.equalsIgnoreCase("绿茶")) label.setIcon(s2);
                else if(sele.equalsIgnoreCase("龙尾")) label.setIcon(s3);
                else if(sele.equalsIgnoreCase("江湾")) label.setIcon(s4);
                else if(sele.equalsIgnoreCase("酒糟")) label.setIcon(s5);
                else if(sele.equalsIgnoreCase("糟米")) label.setIcon(s6);
                else if(sele.equalsIgnoreCase("清明")) label.setIcon(s7);
                else if(sele.equalsIgnoreCase("油煎")) label.setIcon(s8);
                label.setHorizontalAlignment(JLabel.CENTER);
            }
        }
    }
}
class Specialty1 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty1()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty11.jpg");
    }
}
class Specialty2 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty2()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty12.jpg");
    }
}
class Specialty3 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty3()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty13.jpg");
    }
}
class Specialty4 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty4()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty14.jpg");
    }
}
class Specialty5 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty5()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty21.jpg");
    }
}
class Specialty6 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty6()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty22.jpg");
    }
}
class Specialty7 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty7()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty23.jpg");
    }
}
class Specialty8 extends ImageIcon
{
    private static final long serialVersionUID=1L;
    Specialty8()
    {
        super("E:\\workspacecrm\\myboss\\src\\facade/Specialty24.jpg");
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-facade.png)

### 享元
:star: 运用共享技术有效地支持大量细粒度的对象。
```
package javat.structure.flyweight;

import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import javax.swing.*;
public class WzqGame
{
    public static void main(String[] args)
    {
        new Chessboard();
    }
}
//棋盘
class Chessboard extends MouseAdapter
{
    WeiqiFactory wf;
    JFrame f;
    Graphics g;
    JRadioButton wz;
    JRadioButton bz;
    private final int x=50;
    private final int y=50;
    private final int w=40;    //小方格宽度和高度
    private final int rw=400;    //棋盘宽度和高度
    Chessboard()
    {
        wf=new WeiqiFactory();
        f=new JFrame("享元模式在五子棋游戏中的应用");
        f.setBounds(100,100,500,550);
        f.setVisible(true);
        f.setResizable(false);
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        JPanel SouthJP=new JPanel();
        f.add("South",SouthJP);
        wz=new JRadioButton("白子");
        bz=new JRadioButton("黑子",true);
        ButtonGroup group=new ButtonGroup();
        group.add(wz);
        group.add(bz);
        SouthJP.add(wz);
        SouthJP.add(bz);
        JPanel CenterJP=new JPanel();
        CenterJP.setLayout(null);
        CenterJP.setSize(500, 500);
        CenterJP.addMouseListener(this);
        f.add("Center",CenterJP);
        try
        {
            Thread.sleep(500);
        }
        catch(InterruptedException e)
        {
            e.printStackTrace();
        }
        g=CenterJP.getGraphics();
        g.setColor(Color.BLUE);
        g.drawRect(x, y, rw, rw);
        for(int i=1;i<10;i++)
        {
            //绘制第i条竖直线
            g.drawLine(x+(i*w),y,x+(i*w),y+rw);
            //绘制第i条水平线
            g.drawLine(x,y+(i*w),x+rw,y+(i*w));
        }
    }
    public void mouseClicked(MouseEvent e)
    {
        Point pt=new Point(e.getX()-15,e.getY()-15);
        if(wz.isSelected())
        {
            ChessPieces c1=wf.getChessPieces("w");
            c1.DownPieces(g,pt);
        }
        else if(bz.isSelected())
        {
            ChessPieces c2=wf.getChessPieces("b");
            c2.DownPieces(g,pt);
        }
    }
}
//抽象享元角色：棋子
interface ChessPieces
{
    public void DownPieces(Graphics g,Point pt);    //下子
}
//具体享元角色：白子
class WhitePieces implements ChessPieces
{
    public void DownPieces(Graphics g,Point pt)
    {
        g.setColor(Color.WHITE);
        g.fillOval(pt.x,pt.y,30,30);
    }
}
//具体享元角色：黑子
class BlackPieces implements ChessPieces
{
    public void DownPieces(Graphics g,Point pt)
    {
        g.setColor(Color.BLACK);
        g.fillOval(pt.x,pt.y,30,30);
    }
}
//享元工厂角色
class WeiqiFactory
{
    private ArrayList<ChessPieces> qz;
    public WeiqiFactory()
    {
        qz=new ArrayList<ChessPieces>();
        ChessPieces w=new WhitePieces();
        qz.add(w);
        ChessPieces b=new BlackPieces();
        qz.add(b);
    }
    public ChessPieces getChessPieces(String type)
    {
        if(type.equalsIgnoreCase("w"))
        {
            return (ChessPieces)qz.get(0);
        }
        else if(type.equalsIgnoreCase("b"))
        {
            return (ChessPieces)qz.get(1);
        }
        else
        {
            return null;
        }
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-flyweight.png)

### 代理
:star: 为其他对象提供一种代理以控制对这个对象的访问。
```
package javat.structure.proxy;

import java.awt.*;
import javax.swing.*;
public class WySpecialtyProxy
{
    public static void main(String[] args)
    {
        SgProxy proxy=new SgProxy();
        proxy.display();
    }
}
//抽象主题：特产
interface Specialty
{
    void display();
}
//真实主题：婺源特产
class WySpecialty extends JFrame implements Specialty
{
    private static final long serialVersionUID=1L;
    public WySpecialty()
    {
        super("韶关代理婺源特产测试");
        this.setLayout(new GridLayout(1,1));
        JLabel l1=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\proxy/WuyuanSpecialty.jpg"));
        this.add(l1);
        this.pack();
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
    public void display()
    {
        this.setVisible(true);
    }
}
//代理：韶关代理
class SgProxy implements Specialty
{
    private WySpecialty realSubject=new WySpecialty();
    public void display()
    {
        preRequest();
        realSubject.display();
        postRequest();
    }
    public void preRequest()
    {
        System.out.println("韶关代理婺源特产开始。");
    }
    public void postRequest()
    {
        System.out.println("韶关代理婺源特产结束。");
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-proxy.png)

### 适配器
:star: 将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
```
package javat.structure.adapter;

public class ObjectAdapterTest {
    public static void main(String[] args)
    {
        System.out.println("对象适配器模式测试：");
        Adaptee adaptee = new Adaptee();
        Target target = new ObjectAdapter(adaptee);
        target.request();
    }
}
//对象适配器类
class ObjectAdapter implements Target
{
    private Adaptee adaptee;
    public ObjectAdapter(Adaptee adaptee)
    {
        this.adaptee=adaptee;
    }
    public void request()
    {
        adaptee.specificRequest();
    }
}
package javat.structure.adapter;

public class ClassAdapterTest {
    public static void main(String[] args)
    {
        System.out.println("类适配器模式测试：");
        Target target = new ClassAdapter();
        target.request();
    }
}

//目标接口
interface Target
{
    public void request();
}
//适配者接口
class Adaptee
{
    public void specificRequest()
    {
        System.out.println("适配者中的业务代码被调用！");
    }
}
//类适配器类
class ClassAdapter extends Adaptee implements Target
{
    public void request()
    {
        specificRequest();
    }
}
package javat.structure.adapter;

public class InterfaceAdapter {

    public static void main(String[] args) {
        Sourceable source = new SourceSub1();
    }
}

interface Sourceable {
    public void method1();
    public void method2();
}

abstract class Wrapper2 implements Sourceable {
    public void method1() {};
    public void method2() {};
}

class SourceSub1 extends Wrapper2 {
    @Override
    public void method1() {

    }
}

class SourceSub2 extends Wrapper2 {
    @Override
    public void method1() {

    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-adapter.png)

### 桥接
:star: 将抽象部分与实现部分分离，使它们都可以独立的变化。
```
package javat.structure.bridge;
//桥接模式
import java.awt.*;
import javax.swing.*;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
public class BagManage
{
    public static void main(String[] args)
    {
        Color color;
        Bag bag;
        color=(Color)ReadXML.getObject("color");
        bag=(Bag)ReadXML.getObject("bag");
        bag.setColor(color);
        String name=bag.getName();
        show(name);
    }
    public static void show(String name)
    {
        JFrame jf=new JFrame("桥接模式测试");
        Container contentPane=jf.getContentPane();
        JPanel p=new JPanel();
        JLabel l=new JLabel(new ImageIcon("E:\\workspacecrm\\myboss\\src\\bridge/"+name+".jpg"));
        p.setLayout(new GridLayout(1,1));
        p.setBorder(BorderFactory.createTitledBorder("女士皮包"));
        p.add(l);
        contentPane.add(p, BorderLayout.CENTER);
        jf.pack();
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
//实现化角色：颜色
interface Color
{
    String getColor();
}
//具体实现化角色：黄色
class Yellow implements Color
{
    public String getColor()
    {
        return "yellow";
    }
}
//具体实现化角色：红色
class Red implements Color
{
    public String getColor()
    {
        return "red";
    }
}
//抽象化角色：包
abstract class Bag
{
    protected Color color;
    public void setColor(Color color)
    {
        this.color=color;
    }
    public abstract String getName();
}
//扩展抽象化角色：挎包
class HandBag extends Bag
{
    public String getName()
    {
        return color.getColor()+"HandBag";
    }
}
//扩展抽象化角色：钱包
class Wallet extends Bag
{
    public String getName()
    {
        return color.getColor()+"Wallet";
    }
}

class ReadXML
{
    public static Object getObject(String args)
    {
        try
        {
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;
            doc=builder.parse(new File("E:\\workspacecrm\\myboss\\src\\bridge/config.xml"));
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=null;
            if(args.equals("color"))
            {
                classNode=nl.item(0).getFirstChild();
            }
            else if(args.equals("bag"))
            {
                classNode=nl.item(1).getFirstChild();
            }
            String cName="javat.structure.bridge."+classNode.getNodeValue();
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
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/structure-bridge.png)

## 行为型

### 策略模式
:star: 定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换
```
package javat.behavior.strategy;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
public class CrabCookingStrategy implements ItemListener
{
    private JFrame f;
    private JRadioButton qz,hs;
    private JPanel CenterJP,SouthJP;
    private Kitchen cf;    //厨房
    private CrabCooking qzx,hsx;    //大闸蟹加工者   
    CrabCookingStrategy()
    {
        f=new JFrame("策略模式在大闸蟹做菜中的应用");
        f.setBounds(100,100,500,400);
        f.setVisible(true);
        f.setResizable(false);
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        SouthJP=new JPanel();
        CenterJP=new JPanel();
        f.add("South",SouthJP);
        f.add("Center",CenterJP);
        qz=new JRadioButton("清蒸大闸蟹");
        hs=new JRadioButton("红烧大闸蟹");
        qz.addItemListener(this);
        hs.addItemListener(this);
        ButtonGroup group=new ButtonGroup();
        group.add(qz);
        group.add(hs);
        SouthJP.add(qz);
        SouthJP.add(hs);
        //---------------------------------
        cf=new Kitchen();    //厨房
        qzx=new SteamedCrabs();    //清蒸大闸蟹类
        hsx=new BraisedCrabs();    //红烧大闸蟹类
        //qzx.CookingMethod();
    }
    public void itemStateChanged(ItemEvent e)
    {
        JRadioButton jc=(JRadioButton) e.getSource();
        if(jc==qz)
        {
            cf.setStrategy(qzx);
            cf.CookingMethod(); //清蒸
        }
        else if(jc==hs)
        {
            cf.setStrategy(hsx);
            cf.CookingMethod(); //红烧
        }
        CenterJP.removeAll();
        CenterJP.repaint();
        CenterJP.add((Component)cf.getStrategy());
        f.setVisible(true);
    }
    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(100);
        new CrabCookingStrategy();
    }
}
//抽象策略类：大闸蟹加工类
interface CrabCooking
{
    public void CookingMethod();    //做菜方法
}
//具体策略类：清蒸大闸蟹
class SteamedCrabs extends JLabel implements CrabCooking
{
    private static final long serialVersionUID=1L;
    public void CookingMethod()
    {
        this.setIcon(new ImageIcon("E:\\workspacecrm\\myboss\\src\\strategy\\SteamedCrabs.jpg"));
        this.setHorizontalAlignment(CENTER);
    }
}
//具体策略类：红烧大闸蟹
class BraisedCrabs extends JLabel implements CrabCooking
{
    private static final long serialVersionUID=1L;
    public void CookingMethod()
    {
        this.setIcon(new ImageIcon("E:\\workspacecrm\\myboss\\src\\strategy\\BraisedCrabs.jpg"));
        this.setHorizontalAlignment(CENTER);
    }
}
//环境类：厨房
class Kitchen
{
    private CrabCooking strategy;    //抽象策略
    public void setStrategy(CrabCooking strategy)
    {
        this.strategy=strategy;
    }
    public CrabCooking getStrategy()
    {
        return strategy;
    }
    public void CookingMethod()
    {
        strategy.CookingMethod();    //做菜   
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-strategy.png)

### 模板方法
:star: 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
```
package javat.behavior.templateMethod;
public class StudyAbroadProcess
{
    public static void main(String[] args)
    {
        StudyAbroad tm=new StudyInAmerica();
        tm.TemplateMethod();
    }
}
//抽象类: 出国留学
abstract class StudyAbroad
{
    public void TemplateMethod() //模板方法
    {
        LookingForSchool(); //索取学校资料
        ApplyForEnrol();    //入学申请      
        ApplyForPassport(); //办理因私出国护照、出境卡和公证
        ApplyForVisa();     //申请签证
        ReadyGoAbroad();    //体检、订机票、准备行装
        Arriving();         //抵达
    }              
    public void ApplyForPassport()
    {
        System.out.println("三.办理因私出国护照、出境卡和公证：");
        System.out.println("  1）持录取通知书、本人户口簿或身份证向户口所在地公安机关申请办理因私出国护照和出境卡。");
        System.out.println("  2）办理出生公证书，学历、学位和成绩公证，经历证书，亲属关系公证，经济担保公证。");
    }
    public void ApplyForVisa()
    {
        System.out.println("四.申请签证：");
        System.out.println("  1）准备申请国外境签证所需的各种资料，包括个人学历、成绩单、工作经历的证明；个人及家庭收入、资金和财产证明；家庭成员的关系证明等；");
        System.out.println("  2）向拟留学国家驻华使(领)馆申请入境签证。申请时需按要求填写有关表格，递交必需的证明材料，缴纳签证。有的国家(比如美国、英国、加拿大等)在申请签证时会要求申请人前往使(领)馆进行面试。");
    }
    public void ReadyGoAbroad()
    {
        System.out.println("五.体检、订机票、准备行装：");
        System.out.println("  1）进行身体检查、免疫检查和接种传染病疫苗；");
        System.out.println("  2）确定机票时间、航班和转机地点。");
    }
    public abstract void LookingForSchool();//索取学校资料
    public abstract void ApplyForEnrol();   //入学申请
    public abstract void Arriving();        //抵达
}
//具体子类: 美国留学
class StudyInAmerica extends StudyAbroad
{
    @Override
    public void LookingForSchool()
    {
        System.out.println("一.索取学校以下资料：");
        System.out.println("  1）对留学意向国家的政治、经济、文化背景和教育体制、学术水平进行较为全面的了解；");
        System.out.println("  2）全面了解和掌握国外学校的情况，包括历史、学费、学制、专业、师资配备、教学设施、学术地位、学生人数等；");
        System.out.println("  3）了解该学校的住宿、交通、医疗保险情况如何；");
        System.out.println("  4）该学校在中国是否有授权代理招生的留学中介公司？");
        System.out.println("  5）掌握留学签证情况；");
        System.out.println("  6）该国政府是否允许留学生合法打工？");
        System.out.println("  8）毕业之后可否移民？");
        System.out.println("  9）文凭是否受到我国认可？");
    }
    @Override
    public void ApplyForEnrol()
    {
        System.out.println("二.入学申请：");
        System.out.println("  1）填写报名表；");
        System.out.println("  2）将报名表、个人学历证明、最近的学习成绩单、推荐信、个人简历、托福或雅思语言考试成绩单等资料寄往所申请的学校；");
        System.out.println("  3）为了给签证办理留有充裕的时间，建议越早申请越好，一般提前1年就比较从容。");       
    }
    @Override
    public void Arriving()
    {
        System.out.println("六.抵达目标学校：");
        System.out.println("  1）安排住宿；");
        System.out.println("  2）了解校园及周边环境。");
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-templateMethod.png)

### 责任链模式
:star: 避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。
```
package javat.behavior.chainOfResponsibility;
public class LeaveApprovalTest
{
    public static void main(String[] args)
    {
        //组装责任链 
        Leader teacher1=new ClassAdviser();
        Leader teacher2=new DepartmentHead();
        Leader teacher3=new Dean();
        //Leader teacher4=new DeanOfStudies();
        teacher1.setNext(teacher2);
        teacher2.setNext(teacher3);
        //teacher3.setNext(teacher4);
        //提交请求 
        teacher1.handleRequest(3);
    }
}
//抽象处理者：领导类
abstract class Leader
{
    private Leader next;
    public void setNext(Leader next)
    {
        this.next=next; 
    }
    public Leader getNext()
    { 
        return next; 
    }   
    //处理请求的方法
    public abstract void handleRequest(int LeaveDays);       
}
//具体处理者1：班主任类
class ClassAdviser extends Leader
{
    public void handleRequest(int LeaveDays)
    {
        if(LeaveDays<=2) 
        {
            System.out.println("班主任批准您请假" + LeaveDays + "天。");       
        }
        else
        {
            if(getNext() != null) 
            {
                getNext().handleRequest(LeaveDays);             
            }
            else
            {
                  System.out.println("请假天数太多，没有人批准该假条！");
            }
        } 
    } 
}
//具体处理者2：系主任类
class DepartmentHead extends Leader
{
    public void handleRequest(int LeaveDays)
    {
        if(LeaveDays<=7) 
        {
            System.out.println("系主任批准您请假" + LeaveDays + "天。");       
        }
        else
        {
            if(getNext() != null) 
            {
                  getNext().handleRequest(LeaveDays);             
            }
            else
            {
                System.out.println("请假天数太多，没有人批准该假条！");
           }
        } 
    } 
}
//具体处理者3：院长类
class Dean extends Leader
{
    public void handleRequest(int LeaveDays)
    {
        if(LeaveDays<=10) 
        {
            System.out.println("院长批准您请假" + LeaveDays + "天。");       
        }
        else
        {
              if(getNext() != null) 
            {
                getNext().handleRequest(LeaveDays);             
            }
            else
            {
                  System.out.println("请假天数太多，没有人批准该假条！");
            }
        } 
    } 
}
//具体处理者4：教务处长类
class DeanOfStudies extends Leader
{
    public void handleRequest(int LeaveDays)
    {
        if(LeaveDays<=20) 
        {
            System.out.println("教务处长批准您请假"+LeaveDays+"天。");       
        }
        else
        {
              if(getNext()!=null) 
            {
                getNext().handleRequest(LeaveDays);          
            }
            else
            {
                  System.out.println("请假天数太多，没有人批准该假条！");
            }
        } 
    } 
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-chainOfResponsibility.png)

### 迭代器模式
:star: 提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示。
```
package javat.behavior.iterator;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import javax.swing.*;
public class PictureIterator{
    public static void main(String[] args)
    {
        new PictureFrame();
    }
}
//相框类
class PictureFrame extends JFrame implements ActionListener
{
    private static final long serialVersionUID=1L;
    ViewSpotSet ag; //婺源景点集接口
    ViewSpotIterator it; //婺源景点迭代器接口
    WyViewSpot ob;    //婺源景点类
    PictureFrame()
    {
        super("中国最美乡村“婺源”的部分风景图");
        this.setResizable(false);
        ag=new WyViewSpotSet(); 
        ag.add(new WyViewSpot("江湾","江湾景区是婺源的一个国家5A级旅游景区，景区内有萧江宗祠、永思街、滕家老屋、婺源人家、乡贤园、百工坊等一大批古建筑，精美绝伦，做工精细。")); 
        ag.add(new WyViewSpot("李坑","李坑村是一个以李姓聚居为主的古村落，是国家4A级旅游景区，其建筑风格独特，是著名的徽派建筑，给人一种安静、祥和的感觉。")); 
        ag.add(new WyViewSpot("思溪延村","思溪延村位于婺源县思口镇境内，始建于南宋庆元五年（1199年），当时建村者俞氏以（鱼）思清溪水而名。"));
        ag.add(new WyViewSpot("晓起村","晓起有“中国茶文化第一村”与“国家级生态示范村”之美誉，村屋多为清代建筑，风格各具特色，村中小巷均铺青石，曲曲折折，回环如棋局。")); 
        ag.add(new WyViewSpot("菊径村","菊径村形状为山环水绕型，小河成大半圆型，绕村庄将近一周，四周为高山环绕，符合中国的八卦“后山前水”设计，当地人称“脸盆村”。")); 
        ag.add(new WyViewSpot("篁岭","篁岭是著名的“晒秋”文化起源地，也是一座距今近六百历史的徽州古村；篁岭属典型山居村落，民居围绕水口呈扇形梯状错落排布。"));
        ag.add(new WyViewSpot("彩虹桥","彩虹桥是婺源颇有特色的带顶的桥——廊桥，其不仅造型优美，而且它可在雨天里供行人歇脚，其名取自唐诗“两水夹明镜，双桥落彩虹”。")); 
        ag.add(new WyViewSpot("卧龙谷","卧龙谷是国家4A级旅游区，这里飞泉瀑流泄银吐玉、彩池幽潭碧绿清新、山峰岩石挺拔奇巧，活脱脱一幅天然泼墨山水画。"));
        it = ag.getIterator(); //获取婺源景点迭代器 
        ob = it.first(); 
        this.showPicture(ob.getName(),ob.getIntroduce());
    }
    //显示图片
    void showPicture(String Name,String Introduce)
    {       
        Container cp=this.getContentPane();       
        JPanel picturePanel=new JPanel();
        JPanel controlPanel=new JPanel();       
        String FileName="E:\\workspacecrm\\myboss\\src\\iterator/Picture/"+Name+".jpg";
        JLabel lb=new JLabel(Name,new ImageIcon(FileName),JLabel.CENTER);   
        JTextArea ta=new JTextArea(Introduce);       
        lb.setHorizontalTextPosition(JLabel.CENTER);
        lb.setVerticalTextPosition(JLabel.TOP);
        lb.setFont(new Font("宋体",Font.BOLD,20));
        ta.setLineWrap(true);       
        ta.setEditable(false);
        //ta.setBackground(Color.orange);
        picturePanel.setLayout(new BorderLayout(5,5));
        picturePanel.add("Center",lb);       
        picturePanel.add("South",ta);       
        JButton first, last, next, previous;
        first=new JButton("第一张");
        next=new JButton("下一张");
        previous=new JButton("上一张");
        last=new JButton("最末张");
        first.addActionListener(this);
        next.addActionListener(this);
        previous.addActionListener(this);
        last.addActionListener(this);        
        controlPanel.add(first);
        controlPanel.add(next);
        controlPanel.add(previous);
        controlPanel.add(last);       
        cp.add("Center",picturePanel);
        cp.add("South",controlPanel);
        this.setSize(630, 550);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
    @Override
    public void actionPerformed(ActionEvent arg0)
    {
        String command=arg0.getActionCommand();
        if(command.equals("第一张"))
        {
            ob=it.first(); 
            this.showPicture(ob.getName(),ob.getIntroduce());
        }
        else if(command.equals("下一张"))
        {
            ob=it.next(); 
            this.showPicture(ob.getName(),ob.getIntroduce());
        }
        else if(command.equals("上一张"))
        {
            ob=it.previous(); 
            this.showPicture(ob.getName(),ob.getIntroduce());
        }
        else if(command.equals("最末张"))
        {
            ob=it.last(); 
            this.showPicture(ob.getName(),ob.getIntroduce());
        }
    }
}
//婺源景点类
class WyViewSpot
{
    private String Name;
    private String Introduce;
    WyViewSpot(String Name,String Introduce)
    {
        this.Name=Name;
        this.Introduce=Introduce;
    }
    public String getName()
    {
        return Name;
    }
    public String getIntroduce()
    {
        return Introduce;
    }
}
//抽象聚合：婺源景点集接口
interface ViewSpotSet
{ 
    void add(WyViewSpot obj); 
    void remove(WyViewSpot obj); 
    ViewSpotIterator getIterator(); 
}
//具体聚合：婺源景点集
class WyViewSpotSet implements ViewSpotSet
{ 
    private ArrayList<WyViewSpot> list=new ArrayList<WyViewSpot>(); 
    public void add(WyViewSpot obj)
    { 
        list.add(obj); 
    }
    public void remove(WyViewSpot obj)
    { 
        list.remove(obj); 
    }
    public ViewSpotIterator getIterator()
    { 
        return(new WyViewSpotIterator(list)); 
    }     
}
//抽象迭代器：婺源景点迭代器接口
interface ViewSpotIterator
{
    boolean hasNext();
    WyViewSpot first();
    WyViewSpot next();
    WyViewSpot previous();
    WyViewSpot last(); 
}
//具体迭代器：婺源景点迭代器
class WyViewSpotIterator implements ViewSpotIterator
{ 
    private ArrayList<WyViewSpot> list=null; 
    private int index=-1;
    WyViewSpot obj=null;
    public WyViewSpotIterator(ArrayList<WyViewSpot> list)
    {
        this.list=list; 
    } 
    public boolean hasNext()
    { 
        if(index<list.size()-1)
        { 
            return true;
        }
        else
        {
            return false;
        }
    }
    public WyViewSpot first()
    {
        index=0;
        obj=list.get(index);
        return obj;
    }
    public WyViewSpot next()
    {         
        if(this.hasNext())
        { 
            obj=list.get(++index);
        } 
        return obj; 
    }
    public WyViewSpot previous()
    { 
        if(index>0)
        { 
            obj=list.get(--index); 
        } 
        return obj; 
    }
    public WyViewSpot last()
    {
        index=list.size()-1;
        obj=list.get(index);
        return obj;
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-iterator.png)

### 解释器模式
:star: 给定一个语言，定义它的文法表示，并定义一个解释器，这个解释器使用该标识来解释语言中的句子。
```
package javat.behavior.interpreterPattern;
import java.util.*;
/*文法规则
  <expression> ::= <city>的<person>
  <city> ::= 韶关|广州
  <person> ::= 老人|妇女|儿童
*/
public class InterpreterPatternDemo
{
    public static void main(String[] args)
    {
        Context bus=new Context();
        bus.freeRide("韶关的老人");
        bus.freeRide("韶关的年轻人");
        bus.freeRide("广州的妇女");
        bus.freeRide("广州的儿童");
        bus.freeRide("山东的儿童");
    }
}
//抽象表达式类
interface Expression
{
    public boolean interpret(String info);
}
//终结符表达式类
class TerminalExpression implements Expression
{
    private Set<String> set= new HashSet<String>();
    public TerminalExpression(String[] data)
    {
        for(int i=0;i<data.length;i++)set.add(data[i]);
    }
    public boolean interpret(String info)
    {
        if(set.contains(info))
        {
            return true;
        }
        return false;
    }
}
//非终结符表达式类
class AndExpression implements Expression
{
    private Expression city=null;    
    private Expression person=null;
    public AndExpression(Expression city,Expression person)
    {
        this.city=city;
        this.person=person;
    }
    public boolean interpret(String info)
    {
        String s[]=info.split("的");       
        return city.interpret(s[0])&&person.interpret(s[1]);
    }
}
//环境类
class Context
{
    private String[] citys={"韶关","广州"};
    private String[] persons={"老人","妇女","儿童"};
    private Expression cityPerson;
    public Context()
    {
        Expression city=new TerminalExpression(citys);
        Expression person=new TerminalExpression(persons);
        cityPerson=new AndExpression(city,person);
    }
    public void freeRide(String info)
    {
        boolean ok=cityPerson.interpret(info);
        if(ok) System.out.println("您是"+info+"，您本次乘车免费！");
        else System.out.println(info+"，您不是免费人员，本次乘车扣费2元！");   
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-interpreterPattern.png)

### 命令模式
:star: 将一个请求封装成一个对象，从而使您可以用不同的请求对客户进行参数化。
```
package javat.behavior.command;
import javax.swing.*;
public class CookingCommand
{
    public static void main(String[] args)
    {
        Breakfast food1=new ChangFen();
        Breakfast food2=new HunTun();
        Breakfast food3=new HeFen();
        Waiter fwy=new Waiter();
        fwy.setChangFen(food1);//设置肠粉菜单
        fwy.setHunTun(food2);  //设置河粉菜单
        fwy.setHeFen(food3);   //设置馄饨菜单
        fwy.chooseChangFen();  //选择肠粉
        fwy.chooseHeFen();     //选择河粉
        fwy.chooseHunTun();    //选择馄饨
    }
}
//调用者：服务员
class Waiter
{
    private Breakfast changFen,hunTun,heFen;
    public void setChangFen(Breakfast f)
    {
        changFen=f;
    }
    public void setHunTun(Breakfast f)
    {
        hunTun=f;
    }
    public void setHeFen(Breakfast f)
    {
        heFen=f;
    }
    public void chooseChangFen()
    {
        changFen.cooking();
    }
    public void chooseHunTun()
    {
        hunTun.cooking();
    }
    public void chooseHeFen()
    {
        heFen.cooking();
    }
}
//抽象命令：早餐
interface Breakfast
{
    public abstract void cooking();
}
//具体命令：肠粉
class ChangFen implements Breakfast
{
    private ChangFenChef receiver;
    ChangFen()
    {
        receiver=new ChangFenChef();
    }
    public void cooking()
    {       
        receiver.cooking();
    }
}
//具体命令：馄饨
class HunTun implements Breakfast
{
    private HunTunChef receiver;
    HunTun()
    {
        receiver=new HunTunChef();
    }
    public void cooking()
    {
        receiver.cooking();
    }
}
//具体命令：河粉
class HeFen implements Breakfast
{
    private HeFenChef receiver;
    HeFen()
    {
        receiver=new HeFenChef();
    }
    public void cooking()
    {
        receiver.cooking();
    }
}
//接收者：肠粉厨师
class ChangFenChef extends JFrame
{   
    private static final long serialVersionUID = 1L;
    JLabel l=new JLabel();
    ChangFenChef()
    {
        super("煮肠粉");
        l.setIcon(new ImageIcon("E:\\workspacecrm\\myboss\\src\\command/ChangFen.jpg"));
        this.add(l);
        this.setLocation(30, 30);
        this.pack();
        this.setResizable(false);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);   
    }
    public void cooking()
    {
        this.setVisible(true);
    }
}
//接收者：馄饨厨师
class HunTunChef extends JFrame
{
    private static final long serialVersionUID=1L;
    JLabel l=new JLabel();
    HunTunChef()
    {
        super("煮馄饨");
        l.setIcon(new ImageIcon("E:\\workspacecrm\\myboss\\src\\command/HunTun.jpg"));
        this.add(l);
        this.setLocation(350, 50);
        this.pack();
        this.setResizable(false);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    
    }
    public void cooking()
    {
        this.setVisible(true);
    }
}
//接收者：河粉厨师
class HeFenChef extends JFrame
{
    private static final long serialVersionUID=1L;
    JLabel l=new JLabel();
    HeFenChef()
    {
        super("煮河粉");
        l.setIcon(new ImageIcon("E:\\workspacecrm\\myboss\\src\\command/HeFen.jpg"));
        this.add(l);
        this.setLocation(200, 280);
        this.pack();
        this.setResizable(false);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
    public void cooking()
    {
        this.setVisible(true);
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-command.png)

### 备忘录模式
:star: 在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。
```
package javat.behavior.memento;
import java.awt.GridLayout;
import java.awt.event.*;
import javax.swing.*;
public class DatingGame
{
    public static void main(String[] args)
    {
        new DatingGameWin();
    }
}
//客户窗体类
class DatingGameWin extends JFrame implements ActionListener
{
    private static final long serialVersionUID=1L;   
    JPanel CenterJP,EastJP;
    JRadioButton girl1,girl2,girl3,girl4;
    JButton button1,button2;
    String FileName;
    JLabel g;
    You you;
    GirlStack girls;
    DatingGameWin()
    {
        super("利用备忘录模式设计相亲游戏");
        you=new You();
        girls=new GirlStack();       
        this.setBounds(0,0,900,380);            
        this.setResizable(false);
        FileName="E:\\workspacecrm\\myboss\\src\\memento/Photo/四大美女.jpg";
        g=new JLabel(new ImageIcon(FileName),JLabel.CENTER);
        CenterJP=new JPanel();
        CenterJP.setLayout(new GridLayout(1,4));
        CenterJP.setBorder(BorderFactory.createTitledBorder("四大美女如下："));
        CenterJP.add(g);   
        this.add("Center",CenterJP);
        EastJP=new JPanel();
        EastJP.setLayout(new GridLayout(1,1));
        EastJP.setBorder(BorderFactory.createTitledBorder("您选择的爱人是："));
        this.add("East",EastJP);
        JPanel SouthJP=new JPanel();      
        JLabel info=new JLabel("四大美女有“沉鱼落雁之容、闭月羞花之貌”，您选择谁？");
        girl1=new JRadioButton("西施",true);
        girl2=new JRadioButton("貂蝉");
        girl3=new JRadioButton("王昭君");       
        girl4=new JRadioButton("杨玉环");
        button1=new JButton("确定");
        button2=new JButton("返回");
        ButtonGroup group=new ButtonGroup();
        group.add(girl1);
        group.add(girl2);
        group.add(girl3);
        group.add(girl4);
        SouthJP.add(info);
        SouthJP.add(girl1);
        SouthJP.add(girl2);
        SouthJP.add(girl3);
        SouthJP.add(girl4);
        SouthJP.add(button1);
        SouthJP.add(button2);
        button1.addActionListener(this);
        button2.addActionListener(this);
        this.add("South",SouthJP);        
        showPicture("空白");
        you.setWife("空白");
        girls.push(you.createMemento());    //保存状态
    }
    //显示图片
    void showPicture(String name)
    {
        EastJP.removeAll(); //清除面板内容
        EastJP.repaint(); //刷新屏幕
        you.setWife(name);        
        FileName="E:\\workspacecrm\\myboss\\src\\memento/Photo/"+name+".jpg";
        g=new JLabel(new ImageIcon(FileName),JLabel.CENTER);                   
        EastJP.add(g);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);            
    }
    @Override
    public void actionPerformed(ActionEvent e)
    {
        boolean ok=false;
        if(e.getSource()==button1)
        {
            ok=girls.push(you.createMemento());    //保存状态  
            if(ok && girl1.isSelected())
            {
                showPicture("西施");
            }
            else if(ok && girl2.isSelected())
            {
                showPicture("貂蝉");
            }
            else if(ok && girl3.isSelected())
            {
                showPicture("王昭君");
            }
            else if(ok && girl4.isSelected())
            {
                showPicture("杨玉环");
            }               
        }
        else if(e.getSource()==button2)
        {
            you.restoreMemento(girls.pop()); //恢复状态
            showPicture(you.getWife());
        }
    }
}
//备忘录：美女
class Girl
{ 
  private String name;
  public Girl(String name)
  { 
      this.name=name; 
  }     
  public void setName(String name)
  { 
      this.name=name; 
  }
  public String getName()
  { 
      return name; 
  }
}
//发起人：您
class You
{ 
    private String wifeName;    //妻子
    public void setWife(String name)
    { 
        wifeName=name; 
    }
    public String getWife()
    { 
        return wifeName; 
    }
    public Girl createMemento()
    { 
        return new Girl(wifeName); 
    }
    public void restoreMemento(Girl p)
    { 
        setWife(p.getName());
    } 
}
//管理者：美女栈
class GirlStack
{ 
    private Girl girl[];
    private int top;
    GirlStack()
    {
        girl=new Girl[5];
        top=-1;
    }
    public boolean push(Girl p)
    {
        if(top>=4)
        {
            System.out.println("你太花心了，变来变去的！");
            return false;
        }
        else
        {
            girl[++top]=p;
            return true;
        }
    }
    public Girl pop()
    {
        if(top<=0)
        {
            System.out.println("美女栈空了！");
            return girl[0];
        }
        else return girl[top--]; 
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-memento.png)

### 状态模式
:star: 定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
```
package javat.behavior.state;
public class ScoreStateTest
{
    public static void main(String[] args)
    {
        ScoreContext account=new ScoreContext();
        System.out.println("学生成绩状态测试：");
        account.add(30);
        account.add(40);
        account.add(25);
        account.add(-15);
        account.add(-25);
    }
}
//环境类
class ScoreContext
{
    private AbstractState state;
    ScoreContext()
    {
        state=new LowState(this);
    }
    public void setState(AbstractState state)
    {
        this.state=state;
    }
    public AbstractState getState()
    {
        return state;
    }   
    public void add(int score)
    {
        state.addScore(score);
    }
}
//抽象状态类
abstract class AbstractState
{
    protected ScoreContext hj;  //环境
    protected String stateName; //状态名
    protected int score; //分数
    public abstract void checkState(); //检查当前状态
    public void addScore(int x)
    {
        score+=x;       
        System.out.print("加上："+x+"分，\t当前分数："+score );
        checkState();
        System.out.println("分，\t当前状态："+hj.getState().stateName);
    }   
}
//具体状态类：不及格
class LowState extends AbstractState
{
    public LowState(ScoreContext h)
    {
        hj=h;
        stateName="不及格";
        score=0;
    }
    public LowState(AbstractState state)
    {
        hj=state.hj;
        stateName="不及格";
        score=state.score;
    }
    public void checkState()
    {
        if(score>=90)
        {
            hj.setState(new HighState(this));
        }
        else if(score>=60)
        {
            hj.setState(new MiddleState(this));
        }
    }   
}
//具体状态类：中等
class MiddleState extends AbstractState
{
    public MiddleState(AbstractState state)
    {
        hj=state.hj;
        stateName="中等";
        score=state.score;
    }
    public void checkState()
    {
        if(score<60)
        {
            hj.setState(new LowState(this));
        }
        else if(score>=90)
        {
            hj.setState(new HighState(this));
        }
    }
}
//具体状态类：优秀
class HighState extends AbstractState
{
    public HighState(AbstractState state)
    {
        hj=state.hj;
        stateName="优秀";
        score=state.score;
    }           
    public void checkState()
    {
        if(score<60)
        {
            hj.setState(new LowState(this));
        }
        else if(score<90)
        {
            hj.setState(new MiddleState(this));
        }
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-state.png)

### 中介者模式
:star: 用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
```
package javat.behavior.mediator;
import java.awt.BorderLayout;
import java.awt.Container;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;
public class DatingPlatform
{
    public static void main(String[] args)
    {
        Medium md=new EstateMedium();    //房产中介
        Customer member1,member2;
        member1=new Seller("张三(卖方)");
        member2=new Buyer("李四(买方)");
        md.register(member1); //客户注册
        md.register(member2);
    }
}
//抽象中介者：中介公司
interface Medium
{
    void register(Customer member); //客户注册
    void relay(String from,String ad); //转发
}
//具体中介者：房地产中介
class EstateMedium implements Medium
{
    private List<Customer> members=new ArrayList<Customer>();   
    public void register(Customer member)
    {
        if(!members.contains(member))
        {
            members.add(member);
            member.setMedium(this);
        }
    }   
    public void relay(String from,String ad)
    {
        for(Customer ob:members)
        {
            String name=ob.getName();
            if(!name.equals(from))
            {
                ((Customer)ob).receive(from,ad);
            }   
        }
    }
}
//抽象同事类：客户
abstract class Customer extends JFrame implements  ActionListener
{
    private static final long serialVersionUID=-7219939540794786080L;
    protected Medium medium;
    protected String name;   
    JTextField SentText;
    JTextArea ReceiveArea;   
    public Customer(String name)
    {
        super(name);
        this.name=name;       
    }
    void ClientWindow(int x,int y)
    {       
        Container cp;        
        JScrollPane sp;
        JPanel p1,p2;        
        cp=this.getContentPane();       
        SentText=new JTextField(18);
        ReceiveArea=new JTextArea(10,18);
        ReceiveArea.setEditable(false);
        p1=new JPanel();
        p1.setBorder(BorderFactory.createTitledBorder("接收内容："));       
        p1.add(ReceiveArea);
        sp=new JScrollPane(p1);
        cp.add(sp,BorderLayout.NORTH);        
        p2=new JPanel();
        p2.setBorder(BorderFactory.createTitledBorder("发送内容："));       
        p2.add(SentText);   
        cp.add(p2,BorderLayout.SOUTH);       
        SentText.addActionListener(this);       
        this.setLocation(x,y);
        this.setSize(250, 330);
        this.setResizable(false); //窗口大小不可调整
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);      
        this.setVisible(true);       
    }
    public void actionPerformed(ActionEvent e)
    {
        String tempInfo=SentText.getText().trim();
        SentText.setText("");
        this.send(tempInfo);
    }
    public String getName()
    {    return name;   }
    public void setMedium(Medium medium)
    {      this.medium=medium;  }   
    public abstract void send(String ad);
    public abstract void receive(String from,String ad);
}
//具体同事类：卖方
class Seller extends Customer
{
    private static final long serialVersionUID=-1443076716629516027L;
    public Seller(String name)
    {
        super(name);
        ClientWindow(50,100);
    }   
    public void send(String ad)
    {
        ReceiveArea.append("我(卖方)说: "+ad+"\n");
        //使滚动条滚动到最底端
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
        medium.relay(name,ad);
    }
    public void receive(String from,String ad)
    {
        ReceiveArea.append(from +"说: "+ad+"\n");
        //使滚动条滚动到最底端
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
    }
}
//具体同事类：买方
class Buyer extends Customer
{
    private static final long serialVersionUID = -474879276076308825L;
    public Buyer(String name)
    {
        super(name);
        ClientWindow(350,100);
    }   
    public void send(String ad)
    {
        ReceiveArea.append("我(买方)说: "+ad+"\n");
        //使滚动条滚动到最底端
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
        medium.relay(name,ad);
    }
    public void receive(String from,String ad)
    {
          ReceiveArea.append(from +"说: "+ad+"\n");
        //使滚动条滚动到最底端
        ReceiveArea.setCaretPosition(ReceiveArea.getText().length());
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-mediator.jpg)

### 观察者
:star: 指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式，它是对象行为型模式。
```
package javat.behavior.observer;
import java.util.*;
public class RMBrateTest
{
    public static void main(String[] args)
    {
        Rate rate=new RMBrate();         
        Company watcher1=new ImportCompany(); 
        Company watcher2=new ExportCompany();           
        rate.add(watcher1); 
        rate.add(watcher2);           
        rate.change(10);
        rate.change(-9);
    }
}
//抽象目标：汇率
abstract class Rate
{
    protected List<Company> companys=new ArrayList<Company>();   
    //增加观察者方法
    public void add(Company company)
    {
        companys.add(company);
    }    
    //删除观察者方法
    public void remove(Company company)
    {
        companys.remove(company);
    }   
    public abstract void change(int number);
}
//具体目标：人民币汇率
class RMBrate extends Rate 
{
    public void change(int number)
    {       
        for(Company obs:companys)
        {
            ((Company)obs).response(number);
        }       
    }

}
//抽象观察者：公司
interface Company
{
    void response(int number);
}
//具体观察者1：进口公司 
class ImportCompany implements Company 
{
    public void response(int number) 
    {
        if(number>0)
        {
            System.out.println("人民币汇率升值"+number+"个基点，降低了进口产品成本，提升了进口公司利润率。"); 
        }
        else if(number<0)
        {
              System.out.println("人民币汇率贬值"+(-number)+"个基点，提升了进口产品成本，降低了进口公司利润率。"); 
        }
    } 
} 
//具体观察者2：出口公司
class ExportCompany implements Company 
{
    public void response(int number) 
    {
        if(number>0)
        {
            System.out.println("人民币汇率升值"+number+"个基点，降低了出口产品收入，降低了出口公司的销售利润率。"); 
        }
        else if(number<0)
        {
              System.out.println("人民币汇率贬值"+(-number)+"个基点，提升了出口产品收入，提升了出口公司的销售利润率。"); 
        }
    } 
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-observer.png)

### 访问者模式
:star: 主要将数据结构与数据操作分离。
```
package javat.behavior.visitor;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;
public class VisitorProducer
{
    public static void main(String[] args)
    {
        new MaterialWin();       
    }
}
//窗体类
class MaterialWin extends JFrame implements ItemListener
{
    private static final long serialVersionUID=1L;   
    JPanel CenterJP;
    SetMaterial os;    //材料集对象
    Company visitor1,visitor2;    //访问者对象
    String[] select;
    MaterialWin()
    {
        super("利用访问者模式设计艺术公司和造币公司");
        JRadioButton Art;
        JRadioButton mint;           
        os=new SetMaterial();     
        os.add(new Cuprum());
        os.add(new Paper());
        visitor1=new ArtCompany();//艺术公司
        visitor2=new Mint(); //造币公司      
        this.setBounds(10,10,900,350);
        this.setResizable(false);       
        CenterJP=new JPanel();       
        this.add("Center",CenterJP);      
        JPanel SouthJP=new JPanel();
        JLabel yl=new JLabel("原材料有：铜和纸，请选择生产公司：");
        Art=new JRadioButton("艺术公司",true);
        mint=new JRadioButton("造币公司");
        Art.addItemListener(this);
        mint.addItemListener(this);       
        ButtonGroup group=new ButtonGroup();
        group.add(Art);
        group.add(mint);
        SouthJP.add(yl);
        SouthJP.add(Art);
        SouthJP.add(mint);
        this.add("South",SouthJP);       
        select=(os.accept(visitor1)).split(" ");    //获取产品名
        showPicture(select[0],select[1]);    //显示产品
    }
    //显示图片
    void showPicture(String Cuprum,String paper)
    {
        CenterJP.removeAll();    //清除面板内容
        CenterJP.repaint();    //刷新屏幕
        String FileName1="E:\\workspacecrm\\myboss\\src\\visitor/Picture/"+Cuprum+".jpg";
        String FileName2="E:\\workspacecrm\\myboss\\src\\visitor/Picture/"+paper+".jpg";
        JLabel lb=new JLabel(new ImageIcon(FileName1),JLabel.CENTER);
        JLabel rb=new JLabel(new ImageIcon(FileName2),JLabel.CENTER);
        CenterJP.add(lb);
        CenterJP.add(rb);
        this.setVisible(true);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);            
    }
    @Override
    public void itemStateChanged(ItemEvent arg0)
    {
        JRadioButton jc=(JRadioButton) arg0.getSource();
        if (jc.isSelected())
        {
            if (jc.getText()=="造币公司")
            {
                select=(os.accept(visitor2)).split(" ");
            }
            else
            {            
                select=(os.accept(visitor1)).split(" ");
            }
            showPicture(select[0],select[1]);    //显示选择的产品
        }    
    }
}
//抽象访问者:公司
interface Company
{
    String create(Paper element);
    String create(Cuprum element);
}
//具体访问者：艺术公司
class ArtCompany implements Company
{
    public String create(Paper element)
    {
        return "讲学图";
    }
    public String create(Cuprum element)
    {
        return "朱熹铜像";
    }
}
//具体访问者：造币公司
class Mint implements Company
{
    public String create(Paper element)
    {
        return "纸币";
    }
    public String create(Cuprum element)
    {
        return "铜币";
    }
}
//抽象元素：材料
interface Material
{
    String accept(Company visitor);
}
//具体元素：纸
class Paper implements Material
{
    public String accept(Company visitor)
    {
        return(visitor.create(this));
    }
}
//具体元素：铜
class Cuprum implements Material
{
    public String accept(Company visitor)
    {
        return(visitor.create(this));
    }
}
//对象结构角色:材料集
class SetMaterial
{   
    private List<Material> list=new ArrayList<Material>();   
    public String accept(Company visitor)
    {
        Iterator<Material> i=list.iterator();
        String tmp="";
        while(i.hasNext())
        {
            tmp+=((Material) i.next()).accept(visitor)+" ";
        }
        return tmp; //返回某公司的作品集     
    }
    public void add(Material element)
    {
        list.add(element);
    }
    public void remove(Material element)
    {
        list.remove(element);
    }
}
```
![Alt text](https://github.com/independenter/source-learning/blob/master/23%E7%A7%8D%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/res/behavior-visitor.png)
