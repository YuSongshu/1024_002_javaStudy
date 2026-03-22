# ArrayList

## 用前导包

import java.util.ArrayList;

## 增（add）、删（remove）、改（set）、查（get）

### 老版本最重要的 2 个特点

##### 1.List list = new ArrayList();

没有 <Person>

##### 2.取出必须强转

Person p = (Person)list.get(j);

调用 `List` 的 `get()` 方法，获取索引 `j` 位置的元素，将转换后的 `Person` 对象赋值给变量 `p`，后续可通过 `p` 调用 `Person` 类的属性 / 方法



`(Person)`：强制类型转换，作用：

由于 `list` 未指定泛型（如 `List<Person>`），`get()` 方法返回的是 `Object` 类型（Java 中所有类的父类）。没有 `Person` 类的专属属性 / 方法，故要强制类型转换，通过 `p` 调用 `Person` 类的属性 / 方法



```
// 老版本：必须先导包
import java.util.ArrayList;
import java.util.List;

class Person {
    private String name;
    private int age;
    private String gender;

    // 构造方法
    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getGender() {
        return gender;
    }
}

public class Test {
    public static void main(String[] args) {
        // 老版本写法：没有 <Person>
        List list = new ArrayList();

        // 添加对象
        list.add(new Person("张三", 20, "男"));
        list.add(new Person("李四", 22, "女"));
        list.add(new Person("王五", 25, "男"));

        // 老版本核心：必须强转 (Person)
        int j = 0;
        Person p = (Person) list.get(j); // 你要的这句代码

        System.out.println("姓名：" + p.getName());
        System.out.println("年龄：" + p.getAge());
        System.out.println("性别：" + p.getGender());

        System.out.println("-------------------");

        // 老版本遍历
        for (int i = 0; i < list.size(); i++) {
            // 每取一次都要强转！
            Person person = (Person) list.get(i);
            System.out.println("姓名：" + person.getName()
                    + "，年龄：" + person.getAge()
                    + "，性别：" + person.getGender());
        }
    }
}
```

## 注：

1. **Test 类不能直接取** `private` 属性
2. **Test 类可以调用** `public` 方法
3. `public` 方法**内部**可以取 `private` 属性
4. 所以：**间接取到了！**