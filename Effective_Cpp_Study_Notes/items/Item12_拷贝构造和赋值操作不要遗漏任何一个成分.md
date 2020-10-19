# 拷贝构造和赋值操作不要遗漏任何一个成分
##开玩笑？！这怎么可能忘掉呢！！！好吧，来看看下面两个场景：
- 一个已经完成的类，后因扩展需求，新增一个成员变量，你是否同步检查修改了构造拷贝和赋值操作？
```
    //原class
    class Transcation{
    public:
        Transcation(const Transcation &rhs):_ts(rhs._ts){
            ...
        }
        Transcation& operator=(const Transcation &rhs){
            _ts(rhs._ts);
            return *this;
        }
        ...
    private:
        date  _ts;
    };

    //现在有了新需求，需要记录客户信息        
    class Transcation{
    public:
        Transcation(const Transcation &rhs):_ts(rhs._ts){ //拷贝构造是否同步增加新成员的处理？
            ...
        }
        Transcation& operator=(const Transcation &rhs){   //赋值操作是否同步增加新成员的处理？
            _ts(rhs._ts);
            return *this;
        }
        ...
    private:
        Date  _ts; //交易日期
        Turnover _a; //成交额
    };

```
- 继承！derived class的构造函数和赋值操作，是否对base class的成员进行了拷贝？
```
class SaleTranscation:public Transcation{
public:
    BuyTranscation(const BuyTranscation& rhs):
        Transcation(rhs), //不要忘了base class中的成分，无此行的话，会调用base class的default构造函数，
        _c(rhs._c){       //其生成的_ts和 _a都是默认值，而非rhs的_ts和_a
        ...
    }
    BuyTranscation& operator=(const BuyTranscation& rhs){
        Transcation::=(rhs); //同样不要忘了base class的成员
        _c = rhs._c;
        return *this;
    }
    ...
private:
    Custmoer _c; //客户
};

```
----
## 注意！拷贝构造函数不可以调用赋值操作符，同样赋值操作不可以调用拷贝构造函数
- 拷贝构造函数本质是构造新实例，而赋值操作仅是对已经存在的实例的成员变量进行赋值，不会产生新的实例，两者本质不通，不可以互相调用
- 如果两者代码有较多重复，可以考虑将重复部分单独编写为一个私有函数成员，供拷贝构造和赋值操作分别调用