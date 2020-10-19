# 赋值操作的返回值
## 针对operator=，返回值应该是 reference to *this
- 这条规则应该没有必要做太多解释了,不过需要提醒一下，该规则适用于其他有赋值的操作，类似于： +=， -=， *= 等等
```
    class Example{
    public:
        ...
        Example& operator=(const Example &exp){
            ...
            return *this;
        }
    }
```
