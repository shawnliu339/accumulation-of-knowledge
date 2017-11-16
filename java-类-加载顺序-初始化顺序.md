# Java类加载及初始化顺序
**keywords: Java，类，加载顺序，初始化顺序**  
* [Java程序执行过程](#execution)
* [具体实例](#examples)
  + [执行流程](#execution-steps)
  + [初始化](#initialization)
* [参考](#reference)

## <a name="execution"></a>Java程序的执行过程
Java程序的执行可以简单概括为：  
&emsp;&emsp;加载（Loading）=> 连接（Linking）=> 初始化（Initializing）  

**加载：** 将尚未加载到内存的类加载至内存。  
**链接：** 将运行时状态连接至被加载的类。链接分为三个步骤：验证，准备和解析符号引用。  
**初始化：** 将加载和链接完成的类初始化。  
1. 初始化的顺序为：
  + 初始化某一个类前，必须首先初始化它的所有父类。  
    &emsp;父类 => 子类
  + 首先初始化类变量（静态变量），再初始化对象变量（成员变量）。  
    &emsp;类变量 => 对象变量  

2. 关于何时初始化：
  * T为一个类，且T的实例被创建。
  * 在T中声明的静态方法被调用。
  * 在T中声明的静态变量被赋值。
  * 在T中声明的静态变量被使用，且不是常量。
  * T是父类，当其子类被执行时。

## <a name="examples"></a>具体实例
### <a name="execution-steps"></a>执行流程
command line:
```
java Test reboot
```
java code:
```
public class Test {

	public static void main(String[] args) { }

}
```

根据命令行指令，系统会试图执行Test类的main方法，但是，由于虚拟机中Test类还未加载，所以，类加载器会首先加载Test类，然后，进行连接操作。  
由于类尚未初始化，所以仍无法执行main方法，因此，需对Test类进行初始化。
在初始化Test类前，首先需要初始化它的父类。该例中Test的父类只有Object类，因此，首先初始化Object类，然后，初始化Test。  
完成初始化后，main方法接受从命令行传来的参数reboot并执行。

### <a name="initialization"></a>初始化
1. 父类在子类前被初始化  
```
class Super {
    static { System.out.print("Super "); }
}
class One {
    static { System.out.print("One "); }
}
class Two extends Super {
    static { System.out.print("Two "); }
}
class Test {
    public static void main(String[] args) {
        One o = null;
        Two t = new Two();
        System.out.println((Object)o == (Object)t);
    }
}
```
输出结果：  
```
Super Two false
```
由于类One并没有被使用，且没有被链接，因此，没有初始化。类Two在父类Super之后初始化。

2. 只初始化真正声明静态变量的类
```
class Super {
    static int taxi = 1729;
}
class Sub extends Super {
    static { System.out.print("Sub "); }
}
class Test {
    public static void main(String[] args) {
        System.out.println(Sub.taxi);
    }
}
```
输出结果：
```
1729
```
类Sub并没有初始化。因为，真正声明静态变量taxi的位置在父类Super中。

3. 接口初始化并不需要初始化父接口
```
interface I {
    int i = 1, ii = Test.out("ii", 2);
}
interface J extends I {
    int j = Test.out("j", 3), jj = Test.out("jj", 4);
}
interface K extends J {
    int k = Test.out("k", 5);
}
class Test {
    public static void main(String[] args) {
        System.out.println(J.i);
        System.out.println(K.j);
    }
    static int out(String s, int i) {
        System.out.println(s + "=" + i);
        return i;
    }
}
```
输出结果：
```
1
j=3
jj=4
3
```
J.i是常量（Java中接口的变量均为public static final?），所以，I不被初始化。  
K.j实际在接口J中声明，且不为常量，所以，接口J被初始化。J的父接口和K没有被初始化。

## <a name="reference"></a>参考
[1] Java Language Specification, Chapter12: https://docs.oracle.com/javase/specs/jls/se8/html/jls-12.html
