# 标准模板库
## 公共接口
### 初始化
```C
vector<T> vl
vector<T> v2 (vl) // 拷贝复制
vector<T> v2 = vl // 拷贝复制
vector<T> v3(b, e) // 通过迭代器构造（array不支持）
vector<T> v4 {a,b,c... }
vector<T> v4 = {a,b,c...}
// 以下两个视具体容器而定，只适用于顺序容器（不包括array）
vector<T> v5 (n, val) // n个val
vector<T> v6 (n) // 构造n个元素，调用默认构造函数
```
**注：如果列表初始化中提供的值无法用来初始化，就要考虑用该值构造vector对象**
```C
vector<string> v1{10}; // 构造出10个默认初始化的元素
vector<string> v2{10, "hi"}; // 构造出10个值为"hi"的元素
```
容器拷贝需满足容器类型和元素类型均匹配，但是迭代器拷贝不需要，只要元素类型可以转换即可
```C
list<string> list1 = {"Mike", "John", "Adam"};
vector<const char*> articles = {"a", "an", "the"};
list<string> list2(list1);
forward_list<string> words(articles.begin(), articles.end*());
```
### 下标访问
遍历时下标访问采用`size_type i`而不是int
下标访问操作只适用于string、vector、deque和array
### 迭代器
迭代器运算
```C
*iter
iter->mem
++iter
--iter // forward_list不支持
iter1 == iter2
iter1 != iter2
```
对象是常量则只能用const_iterator迭代，且begin和end返回的也是const_iterator  
**注：**
1. **当不需要对数据进行更改时，应使用cbegin及cend**
2. **空容器的begin和end都是尾后迭代器(off-the-end iterator)**
3. **凡是使用了迭代器的循环体，都不要向迭代器所属的容器添加元素**
### 赋值
所有容器均支持
```C
c1 = c2; // 两者需要具有相同类型
c1 = {a, b, c...}; // 列表赋值（array不支持）
```
除关联容器和array外均支持
```C
seq.assign(b,e); // 替换为迭代器b，e间的元素，b和e不能指向seq中的元素
seq.assign(il); // 替换为初始化列表il中的元素
seq.assign(n,t); // 替换为n个t
```
assign只需要类型能够相互转换即可，例如string->const char*  
**注：赋值相关运算会导致左边容器的迭代器、引用和指针失效**
### 交换
```C
a.swap(b); //a、b需相同类型
swap(a, b);
```
**注：**
1. **除string外，交换后原来a的迭代器、引用和指针会指向b**
2. **string交换后迭代器、引用和指针失效**
### 添加
```C
c.insert(p, t) 	   // 在迭代器p之前插入t或由args创建的元素
c.emplace(p, args) // 返回新添加元素的迭代器
c.insert(p, n, t)  // 迭代器p之前插入n个t，返回第一个元素的迭代器
c.insert(p, b, e)  // 迭代器p之前插入b和e范围内的元素，b和e不能指向c中元素，返回第一个元素的迭代器
c.inser(p, il)	   // 迭代器p之前插入元素值列表，返回第一个元素的迭代器
```
**注：**
1. **由于vector、string、deque添加元素会重新分配空间，所以迭代器、指针、引用将会失效**  
2. **`emplace()、emplace_back()、emplace_front()`的原理是先调用构造函数，在容器管理的内存空间直接创建对象；而`push()`是先创建局部临时对象，再压入容器**
### 删除
```C
c.pop_back()
c.pop_front()
c.erase(p) // 删除迭代器p所指的元素，返回被删元素之后的元素的迭代器。p不能是尾后迭代器
c.erase(b,e) // 删除b和e范围内的元素，返回最后一个被删元素之后元素的迭代器
c.clear() // 清空
```
---
## array
### 初始化
需指定元素类型和大小
```C
array<int, 42> a;
array<string, 10> b;
array<int, 10> c = {1, 2, 3, 4}; // 列表初始化个数要小于等于设定长度
array<int, 10> d = c; // 只要元素类型和大小相等，可以拷贝
d = {1}; // ❌错误，可以列表初始化但不能列表赋值
```
### 特点
1. Sequence
2. Contiguous storage
3. Fixed-size aggregate
4. swap会真正交换元素，效率低