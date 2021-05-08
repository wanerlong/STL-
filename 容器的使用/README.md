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
malloc每次申请内存都会有额外的开销，比如cookie上记录了每次申请的内存的大小。

![图片1](https://user-images.githubusercontent.com/72439295/116352880-6b763800-a828-11eb-9c67-42afd708d495.png)

std::alloc内部维护一个链表数组,数组中的每个链表保存某个尺寸的对象,减少了调用malloc的次数,从而减小了malloc带来的额外开销.

![图片2](https://user-images.githubusercontent.com/72439295/116370370-e9443e80-a83c-11eb-97b9-738d93bc225c.png)

容器

STL容器的各实现类关系如下图所示,以缩排形式表示衍生关系(主要是复合关系).于是可以说stack里面有一个deque,或者说set里面有一个rb_tree.

![图片3](https://user-images.githubusercontent.com/72439295/116371085-a040ba00-a83d-11eb-97a1-36e9da8be9ba.png)

容器list

gcc2.9中list如下所示:

![图片4](https://user-images.githubusercontent.com/72439295/116380521-9f605600-a846-11eb-8ec1-8e71c8a006f5.png)

迭代器__list_iterator重载了指针的*,->,++,--等运算符,并定义了iterator_category、value_type、difference_type、pointer和pointer5个关联类型(associated types),这些特征将被STL算法使用.

![图片5](https://user-images.githubusercontent.com/72439295/116410545-77cdb580-a867-11eb-85e1-314af1af76c1.png)

迭代器的5个关联类型在类中均有定义,但是指针类型的关联类型需要根据指针类别进行确定,为了使STL算法同时兼容迭代器和一般指针,就在迭代器(指针)和算法之间加一个中间层萃取器(traits).

迭代器萃取器iterator_traits能够兼容迭代器和一般指针,获取其5个关联类型:iterator_category、value_type、difference_type、pointer和pointer.

![图片6](https://user-images.githubusercontent.com/72439295/116410729-a0ee4600-a867-11eb-982a-2ba7171ef1fc.png)

容器vector

![图片7](https://user-images.githubusercontent.com/72439295/116498919-76d76b00-a8dd-11eb-966e-8a0ceaa56700.png)

容器vector的迭代器start指向第一个元素,迭代器finish指向最后一个元素的下一个元素,这两个迭代器对应begin()和end()的返回值,维持了前闭后开的特性.

vector对使用者是连续的,因此重载了[]运算符.

vector的实现也是连续的,因此使用指针类型做迭代器(即迭代器vector<T>::iterator的实际类型是原生指针T*).

容器deque

容器deque内部是分段连续的,对使用者表现为连续的.
![ETCZOP$0$(@VIT(1)G2YYRX](https://user-images.githubusercontent.com/72439295/117256389-36e42b00-ae7d-11eb-8323-5f7e18505c36.png)
deque也可叫双端队列，容器内部展现为指针，指向每一个buffer，扩充的时候就添加buffer再用指针指向它。

容器queue和stack

![图片1](https://user-images.githubusercontent.com/72439295/117523061-64e67e00-afe9-11eb-9475-ee58cf91e81c.png)
![图片2](https://user-images.githubusercontent.com/72439295/117523063-66b04180-afe9-11eb-950c-2a92c5e0d5c5.png)

容器queue和stack作为deque的适配器(adapter),其内部均默认封装了一个deque作为底层容器,通过该deque执行具体操作.容器queue和stack的元素进出是严格有序的,因此两个容器都不允许遍历,其内部没有定义iterator.

容器rbtree
 
红黑树是一种二叉查找树，但在每个结点上增加一个存储位表示结点的颜色，可以是red或black。红黑树满足以下五个性质：

1. 每个结点或是红色或是黑色；

2. 根结点是黑色；

3. 每个叶结点是黑的；

4. 如果一个结点是红的，则它的两个儿子均是黑色；

5. 每个结点到其子孙结点的所有路径上包含相同数目的黑色结点。

容器rb_tree封装了红黑树,是有序容器,提供了迭代器iterator用以遍历,但不应使用iterator直接改变元素值(虽然编程层面并没有禁止这样做).

rb_tree提供两种插入操作:insert_unique和insert_equal,前者表示节点的key一定在整棵树中独一无二,否则插入失败;后者表示节点的key可重复.

对于rb_tree,定义一个概念:节点的value包括其key和data,这里的data表示一般说法中的value.

![图片3](https://user-images.githubusercontent.com/72439295/117523167-d7eff480-afe9-11eb-91c8-fc4a71e9aad6.png)


