# 尽量不要使用#define
## #define的问题
- 问题1：编译器不认识哦！一旦编译报错，将会是一个灾难
```
    //进行预处理后，编译器不认识A_FLOAT,仅知道6.11，
    //对于开发人员来说，这个6.11是啥？在哪里？
    #define A_FLOAT 6.11
```
- 问题2：对于宏定义中定义的复杂表达式，存在不可预期的异常
```
    //发现异常了吗？a++在FUNC中做了多少次，是否符合预期？
    #define FUNC(x,y) f((x)>(y)?(x):(y))

    int a = 3, b= 2;
    FUNC(a++,b+5);//a++只进行了一次
    FUNC(a++,b);//相当于f((a++)>(b)?(a++):(b)),a++执行了多少次？（没错，2次！）
```
----
## 应该怎么版呢？试试const，enum，inline吧
- 针对问题1，const和enum都是不错的选择
```
    //使用const，基本与#define无差别，不过const修饰的值可以进行取址操作
    const static float A_FLOAT = 6.11
    //使用enum，enum和#define更像，因为无法对其进行取址操作
    enum{
        ANOTHOR_FLOAT = 6.11
    }
```
- 针对问题2，inline可以很好地消除歧义，避免发生不可预期的异常
```
    inline int FUNC(int x, int y){
        return f(x>y ? x : y);
    }
    int a = 3, b= 2;
    FUNC(a++,b);//a++只进行了一次，不会出现歧义呦
```