# 多态中的析构如何处理？
## 在多态场景中(指用base class来指代derived class，用来屏蔽用户不关心的底层处理)，在将derived class的实例指针使用base class指代时，需要将base class的析构函数定义为virtual，否则可能引起资源泄漏（仅执行了base class的析构，未执行derived class的析构，导致derived class中的成员未能释放）
```
    //基类中，声明虚析构函数
    // virtual ~Base() = 0;
    class Base{
    public:
        Base(){};
        virtual ~Base()=0;//纯虚析构函数
        ...
    private:
        ...
    };
    
    //derived class
    class Derived:public Base{
    public:
        Derived(){};
        ~Derived(){};
        ...
    private:
        ...
    };
    
    //使用
    Base * ptr = new Derived();
    ...
    delete ptr;//会调先用Derived的析构函数，如果Base不是纯虚基类，那么之后还会再调用Base的虚析构函数
```