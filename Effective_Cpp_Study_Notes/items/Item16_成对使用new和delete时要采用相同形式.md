# 成对使用new和delete时要采用相同形式
## 不需要过多解释
- 使用 new 申请资源，就要用delete释放资源，针对单个实例的处理
- 使用new []申请资源，就要使用delete[] 释放资源，针对实例数组的处理
- 需要注意，尽量不要使用typedef来定义数组类型，否则可能引起误解，将一个数组类型误认为单个实例，导致错误使用delete，而非delete []