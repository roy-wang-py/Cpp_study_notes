# 以对象管理资源
## 资源泄漏是一个很麻烦的事情
- 我们在程序中，需要注意每个申请的资源都要在使用完成后，归还给系统，否则就会造成资源泄漏
- 一个简单已经的方法就是，将资源封装到类中，以对象的方式对其进行管理，这样在完成使用，跳出作用域时，就可以通过类的析构函数释放资源，防止内存泄漏
- 智能指针属于类指针对象，可以将获得的heap based资源放入到只能指针中
```
    class Investment{...};//类
    Investment* createInvestment(...);//工厂函数
    std::auto_ptr<Investment*>pInv(createInvestment(...));//将工厂函数申请的资源翻入到只能指针中进行管理
                                                          //另外说明，createInvestment直接返回一个raw pointer（未加工指针）
                                                          //本身就存在风险，在其他item中会进行说明
```