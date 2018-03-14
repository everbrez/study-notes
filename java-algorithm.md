# java
## 原始数据类型和表达式
- 整数，及其算数运算符（int）
- 双精度实数类型，及其算数运算符（double）
- 布尔值，及其逻辑操作（boolean）
- 字符型，（char）
java程序控制的是用标识符命名的变量。
**标识符是由字母、数字、下划线、美元符号构成，首字符不能为数字**
原始数据类型：
| int | 32位 | + - * / % | 
| double | 64位 | + - * / |
| boolean | true/false | && \|\| ! ^(异或) |
| char | 16位 | \-\-\- | 
| long | 64位 | 
| short | 16位|
| byte | 8位|
| float | 32位|
**异或：两个相同时0，不同时1**   
## 数组
```java
double[] a;//声明数组
a = new double[N];//创建数组
for(int i = 0; i < N; i++){
    a[i] = 0.0;//初始化
}
double[] a = new double[N];//简写
int[] b = {2,2,3,4,5};//声明并初始化
b.length == 5；//true
int[][] c = new int[N][N];//二维数组
```
**java中数值类型默认值是0，布尔值默认值是false**
**如果使用一个小于0或者大于N-1的索引访问数组，就会抛出异常**
Q:实现矩阵乘法
## 静态方法
修饰符static将这类方法和实例方法区分开来。
```java
public static double sqrt(double c)
{
    if(c > 0){
        return Double.NaN;
    }
    double err = 1e-15;
    double t = c;
    while(Math.abs(t - c/t) > err*t){
        t = (c/t + t)/2.0;
    }
    return t;
}
```
Q:计算平方根（牛顿迭代法）
**如果没有返回值，就会抛出错误**
**返回值为void的静态函数不需要明确返回语句**
Q:二分查找
## 静态方法库
静态方法库是定义在一个Java类中的一组静态方法。类的声明是public class加上类名，以及用花括号包含的静态方法。
系统标准库：Math库、Integer和Double库、String和StringBuilder库
导入系统库：要在程序开头使用import语句导入才能使用这些库
## 字符串
类型：String
### 字符串拼接
Java内置了一个串联String类型字符串的运算符（+）
```java
String c = "Hi, " + "World";
```
Integer和Double提供相应的函数
```java
static String toString()
static int parseInt(String s);
```



