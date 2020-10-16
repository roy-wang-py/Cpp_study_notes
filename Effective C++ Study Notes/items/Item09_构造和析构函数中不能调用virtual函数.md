# virtual函数不能出现在构造函数和析构函数中
## 为啥会有这样的要求？
- 在derived class的构造函数中，会先进行base的构造，而此时base中的虚函数无法下沉到derived class，最终在derived class构造过程中执行的是base class的函数（你预期的应该在derived class构造过程中，执行derived class实现的虚函数，而非base class的虚函数）
- 析构函数存在同样的问题，derived class析构时，先将自己的部分析构后，进行base class的析构，此时base class只能调用自己的虚函数了
## 解决方案？
- 将base class的virtual改为non-virtual函数
- 在deribed class构造过程中，向base class传递必要信息，来进行base class的构造
```
    class Transanction{
    public:
        
        explicit Transanction(const std::string& logInfo);//explicit关键字防止隐式转换
        void logTransaction(const std::string& logInfo) const;//改为non-virtual    
        ...
    }
    Transanction::Transanction(const std::string& logInfo){
        ...
        logTransaction(logInfo);//构造函数中调用logTransaction
    }
    
    class BuyTransaction:public Transanction{
    public:
        BuyTransaction(parameters):Transanction(createLogString(parameters)){
            ...
        }
        ...
    private:
        static std::string createLogString(parameters);//parameters代表传入的参数，
                                                        //通过该函数在构造时向base class传递必要信息
                                                        //注意该函数为static，
                                                        //防止构造过程中使用尚未初始化的成员变量
    }
```
