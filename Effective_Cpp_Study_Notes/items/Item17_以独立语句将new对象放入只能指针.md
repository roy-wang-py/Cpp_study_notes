# 以独立语句将new对象放入智能指针
## 此条主要针对异常安全
- 如果将“new对象放入智能指针”的操作放在函数的入参位置，由于函数入参初始化的执行顺序不确定，可能导致new的对象，在出现异常后无法释放
```
    int priority();
    processWidget(std::share_ptr<Widget> pw, int priority);
    
    processWidget(std::share_ptr<Widget>(new Widget), priority());//此行的入参初始化，总共有3个操作：
                                                                  //new Wiget， share_ptr构造函数，调用priority
                                                                  //3者顺序不确定
```
- 在上述例子中如果执行以下调用顺序：

> - new Widget
> - 调用priority()
> - share_ptr构造函数

如果第二部出现异常，那么已经new 的资源Widget，无法得到回收，就会出现资源泄漏
- 正确处理如下
```
    std::share_ptr<Widget> ptr(new Widget);
    processWidget(ptr, priority());
```    
