# 标准模板库
## vector
### 初始化
```C
vector<T> vl
vector<T> v2 (vl) //拷贝复制
vector<T> v2 = vl //拷贝复制
vector<T> v3 (n, val) //n个val
vector<T> v4 (n)
vector<T> v5 {a,b,c... }
vector<T> v5 = {a,b,c...)
```
**注：如果列表初始化中提供的值无法用来初始化，就要考虑用该值构造vector对象**
```C
vector<string> v1{10}; // 构造出10个默认初始化的元素
vector<string> v2{10, "hi"}; // 构造出10个值为"hi"的元素
```
### 下标访问
遍历时下标访问采用`vector<T>::size_type i`而不是int
### 迭代器
**空容器的begin和end都是尾后迭代器(off-the-end iterator)**
```
＊江er 返回迭代器iter所指元素的引用
1.ter->mem 解引用iter井获取该元素的名为mem的成员， 等价千(*iter).mem
++iter 令iter指示容器中的下一个元素
-
－ iter 令iter指示容器中的上一个元素
iterl == iter2 判断两个迭代器是否相等（不相等）， 如果两个迭代器指示的是同一个元
iterl != iter2 素或者它们是同一个容器的尾后迭代器， 则相等；反之，不相等