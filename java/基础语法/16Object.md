# 16Object

根类 

java的每一个类  包括我们自己创建的  都会默认继承 Object

每个类的祖先都是Object （根类   root java这个树 的 树根）

```java
class One extends Object{
    
}
```



## 功能

1.表示万能的数据类型

​			Object a = 赋值任何数据都可以

​			public  void add(Object obj){}  obj是什么类型都可以



2.提供了5个超级函数 （最重要的5个方法   我们可以继承使用）

alt+shift+s



### toString()

​			负责类的对象默认的打印输出内容

​			如果想修改一个类的对象默认打印输出内容 重写 toString()

## equals()

​			负责一个类的不同对象之间的比较

​				this == obj

​			如果想让一个类的不同对象相等重写equals()

## hashcode()  

​			获取一个类的对象的哈希码  （哈希算法  哈希code）

​			a         a           提前计算 （哪个地址是空   生成一个对应的编码 hashcode）

​			1		1			1

​			计算有时候不准  算完了是空  但是有值

​			1.再造哈希法

​			2.再造定址法

## 	clone()





## 	finalize() 

垃圾回收



```
@Override

	public int hashCode() {
		// TODO Auto-generated method stub
		return super.hashCode();
	}

	@Override
	public boolean equals(Object obj) {
		// TODO Auto-generated method stub
		return super.equals(obj);
	}

	@Override
	protected Object clone() throws CloneNotSupportedException {
		// TODO Auto-generated method stub
		return super.clone();
	}

	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return super.toString();
	}

	@Override
	protected void finalize() throws Throwable {
		// TODO Auto-generated method stub
		super.finalize();
	}
	
```



## 基本数据类型的封装类

```java
java语言一共8个基本类型
byte		替代品当关键字不能使用时候我们可以使用封装类 Byte
short		Short
int			Integer.paresInt("字符串转换成int类型")
long		Long
float   	Float
double		Double.paresDouble("字符串转换成double类型")
char		character
boolean		Boolean
做开发使用基本数据类型 使用关键字就行了
后期发现有很多场景你无法使用基本数据类型的关键字
例如   ArrayList<String> list = new ArrayList<String>(); //只能装字符串
				<int> 不好使 不能使用基本数据类型的关键字
```



