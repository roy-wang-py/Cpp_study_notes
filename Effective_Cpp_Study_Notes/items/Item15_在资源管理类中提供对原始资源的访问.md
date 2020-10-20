# 必须使用raw pointer的场景
## 通常不建议直接使用raw pointer(未经修饰的指针)
- 在一些场景下，我们无法做到这种要求，由于一些接口需要使用原始指针
- 这时我们使用对象管理资源的同时，需要提供一个方法来获取原始指针
```
    FontHandle getFont();            //c api 申请资源
    void releaseFont(FontHandle fh); //c api 释放资源
    
    //使用Font对FontHandle资源进行封装
    class Font{
    public:
        explicit Font(FontHandle fh)
            :f(fh){
                ...
        }
        ~Font(){
            releaseFont(f);
        }
        FontHandle get() const {     //显式返回原始资源
            return f;
        }
        operator FontHandle() const{ //隐式转换，通常不建议使用，可以引起在一些条件下，错误的进行了隐式转换
            return f;
        }
    private:
        FontHandle f;
    }
```