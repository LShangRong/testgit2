# 基于反射的自定义jdbc框架

## 一， javaBean：

​    所有Java对象可以统称为Java Bean。

- 大体又分为两种：

  1. 实体Bean：对应数据库记录的，例如User。


​         2.过程Bean：操作实体Bean的，例如DAO操作User。

- 
     **标准**的Java Bean的具备条件：


​          1.公共的类(public，非抽象)

​           2.必须提供无参数构造器

​           3.字段必须提供标准的set/get方法

​           4.如果需要进行网络传输，必须实现序列化接口（可有可无）



## 二，反射要操作的具体内容（class，Field，Method，Constructor）：

​      

- **Class：**  **每一个Java类都有一个对应的Class，Class表示一个类的类型。**

  **获取方式：**

  

  1. **getClass方法  Class<?> ?表示未知  通常是已知对象获取Class**

  			User u1 = new User();
  		Class<?> c1 = u1.getClass();
  		System.out.println(c1);
  		

    **2.类.class  通常是静态类中获取Class**

  ​		Class<?> c2 = User.class;
  		System.out.println(c2);
  		

    3.**Class<?> forName(String className)  只知道类的全限名时获取Class**

  ​		Class<?> c3 = Class.forName("org.fkjava.bean.User");
  		System.out.println(c3);

- **利用class创建实例**：

  ​               Class<?> forName(String className)  只知道类的全限名时获取Class
  		Class<?> c = Class.forName("org.fkjava.bean.User");
  		创建一个新实例，其实是**调用无参数的构造器**
  		Object o = c.newInstance();//对象创建成功
  		System.out.println(o);

- **Field表示字段**：

    1**.获取要操作的字段属性**，获取方式：

​          Field[] getDeclaredFields()       // 通过Class的方法，获取所有字段

​            Field getDeclaredField(String name)   // 通过Class的方法，name表示字段名，返回单个字段

  

    2.设置某个对象的某个属性值 , 通过Field的方法
           void set(Object obj, Object value)    // value指要设置的值，obj指要设置值得对象

  


    3.获取某个对象的某个属性值
            Object get(Object obj)      // obj指要取值得对象

  


     4.访问权限

​             boolean isAccessible()   // 判断该字段是否可以访问，可以返回true，不可以返回false

​           void setAccessible(boolean flag)    // 修改该字段的访问权限，**这种方式虽然可以达到目的，但是会破会**                         

​                                                                         **Jav aBean类的封装结构**(实际操作不会采取这种方式)

- **Method表示bean的方法：**

  ​    1.获取方式：
  
  ​          Method[] getDeclaredMethods()     // 通过Class的方法，获取所有方法 

  ​          Method getDeclaredMethod(String name,
                              Class<?>... parameterTypes)   // 通过Class的方法，获取单个方法 ，其中 name指方法   

  ​                                                                                      名，parameterTypes指要获取的方法的参数类型

     2.调用方法

  ​          Object invoke(Object obj,  Object... args)       // args表示方法需要的参数，obj指要调用方法的对象

  ​          注意：如果方法是私有，调用时也需要修改访问权限。

- **Constructor表示构造器**（不常用）：


  ​    1.获取方式：

​              Constructor[] getDeclaredConstructors()  // 通过Class的方法，获取所有构造器


    2.通过Class的方法，获取单个构造器
               Method getDeclaredConstructor(
                                  Class<?>... parameterTypes)       // parameterTypes指要获取的方法的参数类型

  ​										

   3.创建实例  initargs指构造器需要的参数

​              T newInstance(Object... initargs)       





