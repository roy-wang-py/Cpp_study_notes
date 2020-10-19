# 不需要class自动生成的default函数成员，怎么办？
## 构造函数和析构函数
- 这两个成员函数是class必备的，只要自己定义一个就可以了
----
## copy构造函数和copy assignment操作符
- 当然可以直接仅声明一下（不要实现！！！即使什么都不做），这样当被调用时，就会产生链接错误
- 有没有更好的方法呢？当然有！定义一个base class，将其copy构造函数和copy assginment操作符都设为private，然后不需要copy构造函数和copy assinment操作符的class，只要继承此基类就可以了
```
    class Uncopyable{
    protect:
        Uncopyable(){};    //允许devried class进行构造和析构
        ~Uncopyable(){};
    private:
        Uncopyable(const Uncopyable&);
        Uncopyable& operator=(const Uncopyable&); //禁止devired class进行copy构造和copy assignment操作符
    };
    
    //使用
    class A:private Uncopyable{
        ...
    };
```