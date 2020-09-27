# String,StringBuffer, StringBuilder的区别

# Java异常以及常用工具类



## Error和Exception的区别

 

<img src="F:\java\Notes\剑指java面试\Java常用类库与技巧.assets\image-20200927102909651.png" alt="image-20200927102909651" style="zoom:50%;" />

Error：程序无法处理的系统错误，编译器不做检查

Exception:程序可以处理的异常，捕获后能恢复

RuntimeException：不可预知的，程序应当自行避免

## 常见Error以及Exception

### RuntimeException

NullPointerException-控制着引用异常

ClassCastException-类型强制转换异常

IllegalArgumentException-传递非法参数异常

IndexOutOfBoundsException-下标越界异常

NumberFormatException-数组格式异常

### 非RuntimeException

ClassNotFoundException - 找不到指定Class

IOException - IO操作异常

### Error

NoClassDefFoundError - 找不到class定义

StackOverFlowError - 深度递归导致栈被耗尽而抛出异常

OutOfMemoryError - 内存溢出异常



# Collection

![image-20200927104447925](F:\java\Notes\剑指java面试\Java常用类库与技巧.assets\image-20200927104447925.png)

# HashMap

## put方法逻辑

![image-20200927104750663](F:\java\Notes\剑指java面试\Java常用类库与技巧.assets\image-20200927104750663.png)