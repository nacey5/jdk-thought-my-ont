![image.png](https://note.youdao.com/yws/res/2390/WEBRESOURCE1009114d12d58b4be9e8ed895dd2a1b8)


1.类型比较 `a instance of Object c`
2.文本块 `""" """`
3.record记录类型
~~~java
record Pepple(String name,int age) //不对外提供方法，类型为final
~~~
4.密封的类和接口
![image.png](https://note.youdao.com/yws/res/2414/WEBRESOURCEcca66669e9ff159a58420bb53063a273)
~~~java
public sealed class Person permits Teacher,Student,SportMan,Doctor{}

final class Teacher extends Person{}

sealed class Student extends Person permits MiddleSchoolStudent,UniversityStudent {}

non-sealed class SportMan extends Person{}
non-sealed class Doctor extends Person{}
~~~
5.隐藏类和接口
![image.png](https://note.youdao.com/yws/res/2449/WEBRESOURCE61246894d8f155570e32d65abb57a365)
![image.png](https://note.youdao.com/yws/res/2452/WEBRESOURCEe4e0f4451e0adacddcef0f2e5955ea4e)
![image.png](https://note.youdao.com/yws/res/2457/WEBRESOURCEe267a7741b569e9c7c41ba6710cccc7a)
~~~java
        //jdk 15之前
        Runnable r=new Runnable() {
            @Override
            public void run() {
                System.out.println("Ss");
            }
        };
        //15新特性，隐藏类
        Runnable r=()->{
            System.out.println("Ss");
        };
~~~
------------
其他基于jvm的修改和优化会在之后继续进行总结
