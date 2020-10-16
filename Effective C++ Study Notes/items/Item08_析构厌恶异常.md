# 析构函数厌恶异常
## 析构函数中存在异常会有啥影响？
- 最直接的影响，就是资源泄漏。析构出现异常了，可以还有部分资源没有释放
- 更多的泄漏：
```
```
## 对于析构存在的异常风险如何处理呢？
- 在析构中catch住所有异常，然后根据实际需要选择忽略改异常，或者直接abort
```
    class DBConnection{
    public:
        ...
        void close(){ //关闭数据库连接，失败则抛出异常
            ...
        }
    }
    
    class DBConn{
    public:
        ~DBConn(){
            try{
                db.close();
            }catch(...){
                ...//忽略并记录异常信息，或者调用std::abort()
            }
        }
    private:
        DBConnection db;
    }
```
- 更好的方式，将可能的异常处理放在析构之外，通知客户，由客户根据需要来自主进行处理
```
    class DBConn{
    public:
        ...
        
        void close(){ //需要客户自主调用进行close操作，针对异常，自行处理
            db.close();
            closed = true;
        }
        
        ~DBConn(){ //如果客户没有进行close操作，那好吧，析构的时候我们记得要处理一下
            if(!closed){
                try{
                    db.close();
                }catch(...){
                    ...//忽略并记录异常信息，或者调用std::abort()
                }  
            }
        }
    private:
        DBConnection db;
        bool closed;
    }
```