# 劣质代码带来的劣质体验

## 通过读者行为反映代码问题

* 一直滚动鼠标滚轮——方法过长
* 总是全局搜索——方法过长
* 反复地查看方法实现——命名无法反映其真是含义
* 不停地查看调用方法实现——调用层数太多
* 反复查看全局常量的值——常量定义方式无法反映其实质
* 画流程图——嵌套太多

## 命名类的问题

要求代码能够用尽可能短的命名来表征尽可能多的含义。

### 缺乏统一性

不同人书写代码的命名习惯和风格应该保持统一性，这样才能使读者读起来感觉使同一个人写的。

### 没有考虑调用时的情形

Grade(班级)类,获得学生数量。

```java
//get有些多余
grade.getStudentsCount();

//给人一种“需要时间”的感觉
grade.countStudents();

//比较不容易理解
grade.students.size();

//误解为班级的房间大小
grade.size();
```

```java
//比较好的方式
grade.studentsConut();
```

### 本地语言命名

不要使用汉语拼音。

### 命名用词不当

fasten 表示固定而不是加速，加速应该是accelerate。

### 超长的命名

例如Internationalization（国际化）这个词可以被缩略为i18n

或者例如：

```java
public class Keyboard {
    //输入的硬键盘的关键字符
    String theInputtedKeyCharacterOfHardKeyboard;
}
```

可以根据其所属关系，修改为：

```java
public class HardKeyboard extends Keyboard {
    public String input;
}
```

这样会简短很多。

### 命名和行为不一致

承诺多做得少；承诺少做得多。

```java
public void update(String[] data) {
    showData(data);
    writeToFile(data);
}
```

显示数据和写文件，但方法名只提到了刷新，如果为了刷新显示，就可能写入错误的数据。

### 否定式命名

```java
//不好理解
if ((hardKeyboardHidden == 0) == false)
```

```java
//容易理解
if (hardKeyboard.isConnected())
```

### 无意义的命名

temp、result、retVal等，不表征具体的含义，这样的命名包含信息太少，在阅读时必须参考其他信息才行。

### 匈牙利命名法

随着现在面向对象的编程方式，已经不用了。比如：student.strScore也不如student.score读起来顺畅。

## 注释类问题

### 每步皆注释

如果每一步都加上注释了，那么会影响整体代码的阅读连续性，从而导致不易阅读。

### 修改履历注释

以前版本管理不是很强大，所以修改代码都要留下履历式的注释，比如什么样的修改理由和方式等，现在这些信息应该在版本管理工具中记录。

### 缺少注释

```java
public void calculateTax() {
    tax = (salary - nonTaxBase) * percentage - tail;
}
```

个人所得税这样的比较专业的计算，如果没有注释代码就很难维护了。

对于try-catch中的catch处理，如果打算忽略Exception，那么就应该把忽略的理由作为注释。

## 风格类问题

### 长方法

一般不超过200行，对于只完成一件事情的方法来说，长度不会超过20行，最长时也应该在60行内。

### 长参数列表

一般建议参数不要超过3个，最好是一个参数都不要。

### 长判断语句

一连串的if，或多个条件并列（用AND和OR链接），再用括号把关系理清楚，这样的判定语句比较容易出现错误。

### 长分支

if-else形式和switch-case形式，这两种形式不要过长（不需要滚动滚动条），否则容易出错。

### 魔法数字

魔法数字是在代码中固定书写一个数字的情形，比如width = 200;之类。

### 字符串直接引用

和魔法数字一样会降低代码的可维护性，另外，系统需要多语言化时，字符串散落在代码各处就不好处理了。

### 变量意思不稳定

变量在同一个方法的不同区块中的含义不同，容易会引发错误。

### 返回值意思不稳定

返回值在0-10时表示某种状态，返回值在11-100时表示另一种状态，处理这种代码必须查字典。

### 无用的方法或变量

如果一个类内部需要访问某个属性，则直接使用该属性即可，不必绕道访问一个private的getter。

### 诡异代码

```java
b = (--c<<a>>3) * (a++)
```

不要卖弄。

## 结构类问题

### 全局变量做返回值

当一个方法需要返回值时，不应该用全局变量，而应该用局部变量，getter不在此范围。

### 不必要的Guard代码

```java
public void process(Object object) {
    if(object == null) {
        return;
    }
    //process
}
```

```java
Object object = ObjectFactor.create();
process(object);
```

在工厂类ObjectFactor中确保object不为null。

## 架构类问题

### 循环引用

一个类中能否包含另外一个类的对象主要还是看二者是否真的具有包含关系，而不是看是否需要引用。

```java
public class InputMethod {
    public Keyboard keyboard;
}

public class Keyboard {
    public InputMethod inputMethod;
}
```

如果Keyboard确实需要访问InputMethod中的某些属性如Context，以便于操作，那么就不该这么定义，可以考虑采用通过中间类实现这个关系。

```java
public class Environment {
    publilc Context context;
    .....

    public static Environment getInstance() {
        .....
    }

    public Context getContext() {
        .....
    }
}
```

调用的时候:

```java
public class InputMethod {
    public void onCreated() {
        Environment environment = Environment.getInstance().getContext();
    }
}
```

这样就解决了循环引用的问题。

### 重复和类似

要杜绝重复和类似。

### 墨守成规

```java
public void writeData() {
    //业务处理
    handler.post(new Runnable()) {
        public void run() {
            updateUI();
        }
    }
}
```

写数据和更新UI本身是两件事，更新UI不应该属于书写数据这个方法的职责，如果改成用注解可以将二者分开。

```java
public void writeData() {
    //业务处理
    .....
}

@After
private void updateUI() {
    .....
}
```

## 代码的可测试性问题

* 难以构建测试夹具
* 难以拆分做单元测试

## 需求变更难以应付

* 粘滞（牵一发而动全身）
* 脆弱（一旦修改就会破坏原有结构）
* 僵硬（代码间总是互相牵制，导致难以入手）
