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
## array
### 初始化
需指定元素类型和大小
```C
array<int, 42>
array<string, 10>