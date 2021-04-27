STL各组件应用示例

STL六大组件

STL六大组件包括容器(container)、分配器(allocator)、算法(algorithm)、迭代器(iterator)、适配器(adapter)和仿函数(functor).
![图片1](https://user-images.githubusercontent.com/72439295/116250544-64a3e280-a7a0-11eb-962a-1be401ad4c67.png)
容器

STL中的容器大体分为序列容器、关联容器和无序容器.
![图片2](https://user-images.githubusercontent.com/72439295/116250928-c5331f80-a7a0-11eb-8d46-1e50aac50bb7.png)

vector底层是一段连续的内存空间,当容器满时进行扩容,将容器大小扩容为原来的两倍.

list类本身具有sort方法,容器本身实现的sort的性能一般比标准库中的算法sort更好.

multiset和multimap底层是使用红黑树实现的.

multimap支持重复的key,因此不能使用重载的[],其他map键值唯一.

unordered_multiset和unordered_multimap的元素个数小于篮子数目,若元素数目达到篮子个数,则容器扩容,将篮子数组扩充约一倍.

分配器

STL容器默认的分配器是std::allocator,除此之外gcc额外定义了几个分配器,其头文件均在目录ext下.
![图片3](https://user-images.githubusercontent.com/72439295/116251663-73d76000-a7a1-11eb-910e-025e9b59010d.png)

VC6.0的默认分配器只是对::operator new和::operator delete的简单封装.
