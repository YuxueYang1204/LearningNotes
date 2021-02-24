# 顺序容器
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
### 添加(不包括forward_list)
```C
c.insert(p, t) 	   // 在迭代器p之前插入t或由args创建的元素
c.emplace(p, args) // 返回新添加元素的迭代器
c.insert(p, n, t)  // 迭代器p之前插入n个t，返回第一个元素的迭代器
c.insert(p, b, e)  // 迭代器p之前插入b和e范围内的元素，b和e不能指向c中元素，返回第一个元素的迭代器
c.insert(p, il)	   // 迭代器p之前插入元素值列表，返回第一个元素的迭代器
```
**注：**
1. **由于vector、string、deque添加元素会重新分配空间，所以迭代器、指针、引用将会失效**  
2. **`emplace()、emplace_back()、emplace_front()`的原理是先调用构造函数，在容器管理的内存空间直接创建对象；而`push()`是先创建局部临时对象，再压入容器**
### 删除(不包括forward_list)
```C
c.pop_back()
c.pop_front()
c.erase(p) // 删除迭代器p所指的元素，返回被删元素之后的元素的迭代器。p不能是尾后迭代器
c.erase(b,e) // 删除b和e范围内的元素，返回最后一个被删元素之后元素的迭代器
c.clear() // 清空
```
### 迭代器、指针和引用失效问题
|操作|容器|效果|
|-|-|-|
|添加|vector/string|存储空间重新分配，三者全部失效<br>为重新分配，插入之前的有效，之后的无效|
||deque|在首尾位置插入，仅迭代器失效<br>在中间插入，三者全部失效|
||list/forward_list|全部有效|
|删除|vector/string|被删元素之前的有效|
||deque|删除尾元素，仅尾后迭代器失效<br>删除首元素，全部有效<br>删除中间元素，全部失效|
||list/forward_list|全部有效|

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

---
## forward_list
对于单向链表而言，只知道头，不知道尾
### 添加
```C
c.insert_after(p, t) 	   // 在迭代器p之后插入t或由args创建的元素
c.emplace_after(p, args)   // 返回新添加元素的迭代器
c.insert_after(p, n, t)    // 迭代器p之后插入n个t，返回最后一个元素的迭代器
c.insert_after(p, b, e)    // 迭代器p之后插入b和e范围内的元素，b和e不能指向c中元素，返回最后一个元素的迭代器
c.insert_after(p, il)	   // 迭代器p之后插入元素值列表，返回最后一个元素的迭代器
```
### 删除
```C
c.erase_after(p) // 删除迭代器p之后的元素，返回被删元素之后的元素的迭代器。p不能是尾后迭代器
c.erase_after(b,e) // 删除b和e范围内的元素，返回最后一个被删元素之后元素的迭代器
```
# 关联容器
## pair
定义在头文件utility中
### 操作
|操作|含义|
|-|-|
|`pair<T1, T2> p`|-|
|`pair<T1, T2> p(v1, v2)`|用v1和v2为p.first和p.second初始化|
|`pair<T1, T2> p{v1, v2}`|同上|
|`make_pair(v1, v2)`|通过v1和v2初始化pair|
|`p1 `*`relop`*` p2`|通过关系运算符（<,>,<=,>=)运算，`p1.first *relop* p2.first`或者<br>`!(p1.first *relop* p2.first) && (p1.second *relop* p2.second)`时返回True|
|`p1 == p2`|first和second同时相等返回True|
### 创建pair对象的函数
```C
pair<string, int> process(vector<string> &v){
    if(v.empty())
	return pair<string, int>();		// 返回空值
    else
	return {v.back(), v.back().size()};	// 列表初始化构造
	// 等价于 return make_pair(v.back(), v.back().size());
}
```
## 关联容器操作
# 泛型算法
## 分类（常见算法举例）
### 只读算法
#### `find()`（定义在algorithm头文件中）
```C
int ia[] = {27, 10, 29};
int val = 10;
int *ptr = find(begin(ia), end(ia), val); // 找到就返回指针，找不到就返回end(ia)
```
#### `find_if()`（定义在algorithm头文件中）
第三个参数为一元谓词，返回第一个使谓词非0的元素的迭代器
```C
vector<int> myvector = {1, 2, 3, 3, 4};
auto iter = find_if(myvector.cbegin(), myvector.cend(), IsOdd);
```
#### `equal()`（定义在algorithm头文件中）
```C
int myints[] = {20,40,60,80,100};
vector<int>myvector (myints,myints+5)；
equal(myvector.begin(), myvector.end(),myints)；
```
**注：**
1. **第三个参数表示序列首元素迭代器，需满足第二个序列长度大于等于第一序列**
2. **容器类型可不同，但是元素之间必须能通过==判断**
#### `accumulate()`（定义在numeric头文件中）
```C
int sum1 = accumulate(vec.cbegin(), vec.cend(), 0); // 第三个参数为初始值
string sum2 = accumulate(s.cbegin(), s.cend(), string("")); // ""为const char*类型，不可加，需要构造string
```
**注：求和需满足元素之间可加**
  
---
### 写容器算法
#### `fill()`（定义在algorithm头文件中）
```C
fill(vec.begin(), vec.begin() + vec.size()/2, 0); // 将序列前一半设为0
```
#### `fill_n()`（定义在algorithm头文件中）
`fill_n(dest, n, val)`从dest开始设置n个val
```C
fill_n(vec.begin(), vec.size()/2, 0); //将序列前一半设为0
```
#### `copy()`（定义在algorithm头文件中）
```C
auto iter = copy(a.cbegin(), a.cend(), b.begin()); // 将a拷贝给b且返回第三个参数递增之后的值
```
#### `replace()`（定义在algorithm头文件中）
此算法接受4个参数：前两个是迭代器，表示输入序列，后两个一个是要搜索的值，另一个是新值
```C
replace(a.begin(), a.end(), 0, 42); // 将a中0换成42
```
#### `replace_copy()`（定义在algorithm头文件中）
此算法接受额外第三个迭代器参数，指出调整后序列的保存位置
```C
replace_copy(a.cbegin(), a.cend(), b.begin(), 0, 42); // 将a中0换成42的序列存入b，a不变
```
#### `for_each()`（定义在algorithm头文件中）
此算法第三个参数为可调用对象，且对序列每个元素调用该对象
```C
for_each(a.begin(), a.end(), pred)
```
---
### 重排容器算法
#### `sort()`（定义在algorithm头文件中）
```C
sort(a.begin(), a.end(), pred); // 将a元素从小到大排序
```
通过lambda表达式进行排序
```C
sort(a.begin(), a.end(), [](const string &a, const string &b) {return a.size() < b.size();});
```
**注：需满足元素间可比，无pred则通过"<"比较，否则通过二元谓词pred比较**
#### `unique()`（定义在algorithm头文件中）
将**相邻的**重复的元素放到容器的末尾，返回值是去重之后的尾地址
```C
vector<int> myvector = {10,20,20,20,30,30,20,20,10};
auto it = unique (myvector.begin(), myvector.end()); // 10,20,30,20,10,?,?,?,?
// it指向myvector[5]
```
**注：因为只是去掉相邻重复元素，所以一般先sort再unique**