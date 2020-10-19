# const是个好工具
## const作用不必多说，能够避免对于变量的误操作
## const类，const类成员函数的重点说明
- const类成员函数，不能修改任何类成员，且未避免误操作，如果返回类成员信息，也需要保证有const修饰，防止用户误操作
- 存在某种情况，const的成员函数和non-const的成员函数，代码实现完全相同，为避免代码重复，且不影响安全性的情况下，可以使用const成员函数来实现non-const成员函数（需要用到static_cast和const_cast）
```
    class Text{
    public:
        const char& getText(int index) const;
        char& getText(int index);
    private:
        char* content;
    }
    const char& Text::getText(int index)const
    {
        ...检查边界条件
        ...留存访问记录
        ...其他可能操作
        
        return content[index]；
    }
    char& Text::getText(int index)
    {
        return const_cast<char&>( static_cast<const Text&>(*this).getText(index));
    }
```
- 对于const成员函数使用的特殊情况(这里的特殊情况主要是存在bitwise constness和logical constness的概念差异，具体参考书中描述)
> const成员函数不能够修改类中的任何成员（部分编译器会进行检查，严格要求，也就是严格遵守bitwise constness规则），但是实际上可能需要遵守logical constness的规则，也就是const成员函数可能需要修改类中的一些成员，此时mutable关键字，就派上用场了
```
class Text{
    public:
        const char& getText(int index) const;
        char& getText(int index);
    private:
        char* content;
        mutable std::string log;//用来留存访问记录等信息
    }
```
