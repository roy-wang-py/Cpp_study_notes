#使用前需要初始化
## 使用前初始化，能够规避很多不可预期的异常，这里我们主要说类的初始化
- 类成员的初始化，需要尽量在初始化列表中进行（初始化列表中的顺序应该与成员声明的顺序保持一致）
- 处于不同文件中的类，由于无法保证文件编译的顺序，可能导致使用了未经初始化的类，此时对于类实例的调用，需要尽量使用函数封装
```
    //文件A.hpp
    class A{
        ...
        void excute();
    };
    A& theA();
    //文件A.cpp
    void A::excute(){
        ...
    }
    //用函数进行封装，保证函数调用的时候，进行了初始化
    A& theA(){
        static A a;
        return a;
    }
    
    //文件B.hpp
    class B{
        void testCallback();
    }
    //文件B.cpp
    void B::testCallback(){
        theA().excute();
    }
    
```