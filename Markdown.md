# Markdown

## 标题

- 使用`=`和`-`
  
  一级标题
  =================
  
  二级标题
  -----------------
  
- 使用`#`

  # 一级标题
  ## 二级标题 
  ### 三级标题
  #### 四级标题
  ##### 五级标题
  ###### 六级标题

<br />

## 字体

- 斜体

  *斜体*  
  _斜体_  

- 粗体

  **粗体**  
  __粗体__  

- 粗斜体

  ***粗斜体***  
  ___粗斜体___  

- 删除线

  ~~删除线~~

<br />

## 分割线

三个或者三个以上的`-`或者`*`都可以。 

---
----
***
*****

<br />

## 超链接
- [GitHub - 有title](https://github.com/ "Github")  
- [GitHub - 无title](https://github.com/)
- [GitHub - 参考式][1]
- 直接使用 - <https://github.com/>

[1]:https://github.com/

<br />

## 图片

![我的头像](https://avatars2.githubusercontent.com/u/6275608?s=460&u=2c1e13d851f94cee49bc1b87406e2329d8a60976&v=4 "我的头像")

<br />

## 代码块

- 行内式

  演示各种编程语言的`Hello World`程序

- 包裹式

  - Java
  
  ```java
  public class HelloWorld  
  {  
      public static void main(String[] args)  
      {    
          System.out.println("Hello,World!");  
      }  
  }  
  ```
  
  - C
  ```c
  #include <stdio.h>  
  int main()
  {  
      printf("Hello,World!");
      return 1;
  }
  ```
  
  - C++
  ```cpp
  #include <iostream>  
  #include <stdio.h>
  
  int main()  
  {  
      printf("Hello,World!--Way 1\n");  
      puts("Hello,World!--Way 2");  
      puts("Hello," " " "World!--Way 3");  
      std::cout << "Hello,World!--Way 4" << std::endl;  
      return 1;  
  } 
  ```

  - Python
  ```python 
  print "Hello,World!"   #Python 2.x
  print("Hello,World!")  #Python 3.x
  ```

  - C#
  ```csharp 
  using System;  
  class TestApp  
  {  
      public static void Main()  
      {  
        Console.WriteLine("Hello,World!");  
        Console.ReadKey();  
      }  
  } 
  ```

  - Shell
  ```bash 
  echo "Hello,World!"
  ```
  
<br />

## 列表

### 无序列表

#### 使用*

* 第一项
* 第二项
* 第三项

#### 使用+

+ 第一项
+ 第二项
+ 第三项

#### 使用-

- 第一项
- 第二项
- 第三项

### 有序列表

1. 第一项
2. 第二项
3. 第三项

### 列表嵌套

#### 嵌套1

- 第一项
  - 元素A
  - 元素B
- 第二项
  - 元素c
  - 元素D

#### 嵌套2

1. 第一项
   1. 元素A
   2. 元素B
2. 第二项
   1. 元素c
   2. 元素D

#### 嵌套3

1. 第一项
   - 元素A
   - 元素B
2. 第二项
   - 元素c
   - 元素D

#### 嵌套4

- 第一项
  1. 元素A
  2. 元素B
- 第二项
  1. 元素c
  2. 元素D

<br />

## 引用

### 并列

> 引用1  
> 引用2  
> 引用3  

### 嵌套

> 第一层嵌套   
>> 第二层嵌套  
>>>>> 第五层嵌套

### 引用中使用列表

> 引用
> 1. 第一项
> 2. 第二项
> + 第一项
> + 第二项
> + 第三项

### 列表中使用引用

* 第一项
    > 引用1  
    > 引用2 
* 第二项

<br />

## 表格

| 默认对齐 | 居左对齐 | 居右对齐 | 居中对齐 |
| ------- | :------- | ------: | :-----: |
| 单元格 | 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 | 单元格 |

<br />

## 转义字符

| 字符 | 描述 |
| :-: | :-: |
| \\ | 反斜线 |
| \` | 反引号 |
| \* | 星号 |
| \_  | 下划线 |
| \{\} | 花括号 |
| \[\] | 方括号 |
| \(\) | 小括号 |
| \# | 井号 |
| \+ | 加号 |
| \- | 减号 |
| \. | 英文句点 |
| \! | 感叹号 |

<br />

## 特殊字符

| 字符 | 描述 |
| :-: | :-: |
| &nbsp; | 空格符 |
| &lt; | 小于号 |
| &gt; | 大于号 |
| &amp; | 与号 |
| &yen; | 人民币 |
| &copy; | 版权 |
| &reg; | 注册商标 |
| &deg;C | 摄氏度 |
| &plusmn; | 正负号 |
| &times; | 乘号 |
| &divide; | 除号 |
| &sup2; | 平方(上标²) |
| &sup3; | 平方(上标³) |
