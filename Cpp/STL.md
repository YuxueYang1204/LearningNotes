# 标准模板库
## 公共点
### 初始化
```C
vector<T> vl
vector<T> v2 (vl) //拷贝复制
vector<T> v2 = vl //拷贝复制
vector<T> v {a,b,c... }
vector<T> v5 = {a,b,c...}
vector<T> v3 (n, val) //n个val
vector<T> v4 (n)
```
**注：如果列表初始化中提供的值无法用来初始化，就要考虑用该值构造vector对象**
```C
vector<string> v1{10}; // 构造出10个默认初始化的元素
vector<string> v2{10, "hi"}; // 构造出10个值为"hi"的元素
```
### 下标访问
遍历时下标访问采用`vector<T>::size_type i`而不是int
### 迭代器
迭代器运算
```C
*iter
iter->mem
++iter
--iter
iter1 == iter2
iter1 != iter2
```
对象是常量则只能用const_iterator迭代，且begin和end返回的也是const_iterator  
**空容器的begin和end都是尾后迭代器(off-the-end iterator)**
**凡是使用了迭代器的循环体，都不要向迭代器所属的容器添加元素**