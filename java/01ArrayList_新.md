# ArrayList

## 用前导包

import java.util.ArrayList;

## 语法（创建对象）：

ArrayList<数据类型> 变量名 = new ArrayList<>();

注：

泛型只能用包装类（Integer/String/Double等），不能用基本类型（int/char）

```
 ArrayList<String> list = new ArrayList<>(); 
 ArrayList<Integer> numList = new ArrayList<>();

```



## 常用方法

### 增

list.add("添加的元素");

#### 指定索引插入

list.add(1, "添加的元素"); 

```
list.add("C++");
list.add(1, "Go");
```



### 删

#### 根据索引删除

list.remove(索引值); 

#### 根据元素删除

list.remove("删除的元素");

```
list.remove(3); 
list.remove("Java"); 
```



### 改

list.set(索引值, "修改的元素");

```
list.set(2, "JavaScript");
```



### 查（通过索引）

list.get(索引值);

```
String element = list.get(0);
System.out.println("第一个元素：" + element);
```



### 获取长度

list.size();

```
int size = list.size();
```



### 遍历集合

```
// 方式1：普通for循环
for (int i = 0; i < list.size(); i++) {
	System.out.println(list.get(i));
}
// 方式2：增强for循环（推荐）
for (String s : list) {
	System.out.println(s);
}
```



### 清空集合

list.clear(); 



### 判断是否为空 

list.isEmpty();

```
boolean isEmpty = list.isEmpty();
println(isEmpty);
```



### 判断是否包含元素

list.contains("判断的元素"); 

```
boolean contains = list.contains("Python");
println(contains)
```



## 面向对象

 声明 list 只能存对应的对象

List<创建的对象> list = new ArrayList<>();

本质：数据类型为创建的对象

```
import java.util.ArrayList;
import java.util.List;

class Person {
    // 成员变量
    private String name;  // 姓名					
    private String name;  // 姓名					
    private int age;      // 年龄
    private String gender;// 性别
	
	//加了 private 的变量 / 方法只有当前类能访问，外面的类不能直接访问、不能直接修改
  
  
  // 构造方法：创建对象时赋值
    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    // 获取姓名
    public String getName() {
        return name;
    }

    // 获取年龄
    public int getAge() {
        return age;
    }

    // 获取性别
    public String getGender() {
        return gender;
    }
}

public class Test {
    public static void main(String[] args) {
        //  创建 List<Person> 集合
        List<Person> list = new ArrayList<>();

        //  添加 3 个 Person 对象
        list.add(new Person("张三", 20, "男"));
        list.add(new Person("李四", 22, "女"));
        list.add(new Person("王五", 25, "男"));

        // 取出第 0 个元素（不用强转！）
        Person p = list.get(0);
        System.out.println("姓名：" + p.getName());
        System.out.println("年龄：" + p.getAge());
        System.out.println("性别：" + p.getGender());

        System.out.println("-------------------");

        //  遍历整个集合（最常用）
        for (Person person : list) {
            System.out.println("姓名：" + person.getName()
                    + "，年龄：" + person.getAge()
                    + "，性别：" + person.getGender());
        }
    }
}
```

