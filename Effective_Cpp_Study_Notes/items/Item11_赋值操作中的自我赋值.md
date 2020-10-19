# 类中自我赋值的注意事项
## 多个指针， 引用指向同一个对象实例时，可能会出现自我赋值，如果处理不当，可能会引起异常
- 要十分注意成员变量中存在指针/引用的情况
```
    class Bytes{...};
    
    class Mem{
    public:
        ...
        Mem& operator=(const Mem &rhs){
            delete pMem;                    //先删除pMem，如果是自我赋值，此时也就删除了rhs.pMem
            pMem = new Bytes(*rhs.pMem);    //发现问题了吧，使用了已经被删除的rhs.pMem
            return *this;
        }
    private:
        Bytes *pMem;
    };
    
```
----
## 处理方案
- 方法1：增加证同测试（检查当前实例与入参是否为同一实例）
```
    class Mem{
    public:
        ...
        Mem& operator=(const Mem &rhs){
            if(this == &rhs){       //证同测试
                return *this;
            }
            delete pMem;                    
            pMem = new Bytes(*rhs.pMem);    
            return *this;
        }
    private:
        Bytes *pMem;
    };
    
```

- 方法2：注意考虑执行的顺序，保证赋值操作执行完成后，进行删除操作
```
    class Mem{
    public:
        ...
        Mem& operator=(const Mem &rhs){
            Bytes *pTemp = pMem;        //先将自己的成员变量保存            
            pMem = new Bytes(*rhs.pMem);//一旦这里出现异常，pMem也没有被修改或着删除，说明这里是异常安全的    
            delete pTemp;               //全部处理完成后，通过副本pTemp删除原有内容
            return *this;
        }
    private:
        Bytes *pMem;
    };
```
- 通常方法2是更好的选择，不过有时为了执行效率，可以先进行证同测试后，再使用方法2，不过需要确认证同测试的执行频率，频率太低，就没有必要了