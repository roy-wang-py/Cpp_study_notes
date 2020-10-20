# 资源管理类中的copying行为
## 资源管理中的copying行为需要格外注意，针对copying行为通常有两种方式：
- 禁止copying，在Item06中有讨论过如何禁止生成拷贝构造函数和赋值操作
- 对底层资源使用“引用计数法”，通过副本保存，通常可以考虑使用智能指针std::shared_ptr
```
    class Lock{
    public:
        explicit Lock(Mutem* pm):       //使用默认的析构函数，并在shared_ptr中指定删除器
            mutexPtr(pm, unlock){       //使得默认析构函数被调用时，执行shared_ptr的删除器进行解锁操作
                lock(mutexPtr.get());   //而不是直接删除pm
        }
    private:
        std::shared_ptr<Mutem> mutexPtr; //使用shared_ptr，通过引用计数的方法，进行操作，保护资源
    };
```