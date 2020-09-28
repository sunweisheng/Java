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


