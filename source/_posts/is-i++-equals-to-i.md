title: "i++ == i?"
date: 2015-04-26 10:51:59
tags:
- "碎片"
- "JAVA"
---
研究这个问题的原因是我在一次写循环的时候，把

	while(i<arr.length){
		arr[i] = tmp[i++];
	}

写成了

	while(i<arr.length){
		arr[i++] = tmp[i];
	}
<!-- more -->
导致了数组越界异常，当时觉得很奇怪，以前上课的时候老师不是说i++的意思就是执行完当前操作再自增吗？这样的话`arr[i++] = tmp[i]`和`arr[i] = tmp[i++]`不是应该等价的吗？而且等号的计算不是应该先算右边的嘛？当时就凌乱了，于是去oracle官网翻了翻官方文档，发现上面是这样写的

>* First, the left-hand operand is evaluated to produce a variable. If this evaluation completes abruptly, then the assignment expression completes abruptly for the same reason; the right-hand operand is not evaluated and no assignment occurs.  
>* Otherwise, the right-hand operand is evaluated. If this evaluation completes abruptly, then the assignment expression completes abruptly for the same reason and no assignment occurs.  
>* Otherwise, the value of the right-hand operand is converted to the type of the left-hand variable, is subjected to value set conversion (§5.1.13) to the appropriate standard value set (not an extended-exponent value set), and the result of the conversion is stored into the variable.

原来在JAVA里赋值的运算是先计算等号左边的表达式，计算完成通过后再计算等号右边的表达式，最后再执行赋值操作
而关于++，文档里是这么说的
>* The type of the postfix increment expression is the type of the variable. The result of the postfix increment expression is not a variable, but a value.  
>* At run time, if evaluation of the operand expression completes abruptly, then the postfix increment expression completes abruptly for the same reason and no incrementation occurs. Otherwise, the value 1 is added to the value of the variable and the sum is stored back into the variable. Before the addition, binary numeric promotion (§5.6.2) is performed on the value 1 and the value of the variable. If necessary, the sum is narrowed by a narrowing primitive conversion (§5.1.3) and/or subjected to boxing conversion (§5.1.7) to the type of the variable before it is stored. The value of the postfix increment expression is the value of the variable before the new value is stored.

也就是说++符号在执行到的时候就会为这个变量自增一，而整个++表达式的值是执行自增操作前的变量值。  
这样一来上面的问题就很清楚了，在`arr[i++] = tmp[i]`式子中先计算等号左边的表达式，此时i的值已经完成自增，但在左边的表达式中使用的还是原来的值。到计算右边部分的时候使用的i则是已经自增后的变量，自然会导致越界

最后，写个小程序来验证一下，代码如下

````
public class TestIncre {
	public static void main(String[] args) {
		int i = 1;
		System.out.println(i++ == i);
	}
}
````
用javap命令来看一下这段代码的字节码

![程序的字节码](/css/images/is-i++-equals-to-i/1.jpg)

其中第5-10行执行的操作顺序为

* 将局部变量表slot1的值放入操作数栈->变量自增->将局部变量表slot1的值放入操作数栈->比较数值

很明显自增操作在第二次取值操作之前发生，这也验证了上面文档所说的执行顺序

