[TOC]



## STL   简介

### 方法速览

#### vector

```
vector, 变长数组，倍增的思想
    size()  返回元素个数
    empty()  返回是否为空，返回的是 int 类型
    clear()  清空
    front()/back()
    push_back()/pop_back()
    emplace_back()      // 高效率
    begin()/end()
    []
    支持比较运算，按字典序
```

#### pair

```
pair<int, int>    其中两个元素的类型可以是任意的   ----->   类似于帮助我们实现一个二元结构体，并且加了一个比较器
    
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）
    也可以通过函数 make_pair(type_var,type_var)来为 pair 赋值
```

#### string

```
string，字符串
    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址
    to_string() 将 其他基本数据类型转换为 string 类型

构造函数：
        string s(char * a);    // 将字符数组转换为字符串， 
		string s(int n,char c);   // 用 n 个字符 c 来初始化 s
		
		常用成员函数：
        char&  at(int n);    // 返回 索引 n 位置的 字符
		char*  c_str()   //  返回一个以 null 为 终止的 c 字符串
        char*  data()    //  返回一个以 null 为终止的 c 字符数组   
            
    //把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目
    int copy(char *s, int n ,int pos) 
    
    
```

#### queue

```
queue, 队列
    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素
    
   queue<int> q;
   // queue没有 clear() 函数，清空的话直接赋一个空的 queue即可
   q = queue<int>();
```

#### priority_queue

```
priority_queue, 优先队列，默认是大根堆   ---->   t
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    
    对于数字来来说，我们可以在插入的时候插入负数即可，输出的时候输出 x 即可
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;
```

#### stack

```
stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素
```

#### deque

```
deque, 双端队列 ，支持队头队尾的插入删除，也支持随意访问。加强版的 vector,但是速度很慢
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []
```

#### set,map,multiset,multimap

```
set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护  有序序列
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)
    
    
    
    set/multiset
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)  k 是 x 的个数
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回   大于等于 x   的最小的数的迭代器
            upper_bound(x)  返回   大于 x      的最小的数的迭代器
    
    
    
    map/multimap
    	emplace(key , v) 插入的是一个 pair
        insert()  插入的是一个pair
        erase()  输入的参数是pair或者迭代器
        find(key) 返回的是该 key位置的迭代器 ，反之返回末尾元素的迭代器。
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()
        count(key) 查找当前容器中 键为key 的键值对的个数并返回。由于map 中元素唯一，
        			最大返回 1
```

#### unordered_set,underorder_map,unordered_multiset,unordered_multiamp

```
 // 无序的 ， 基于哈希表
unordered_set, unordered_map, 
unordered_multiset, unordered_multimap,

    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--
```

#### bitset

```
bitset, 圧位
    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反      

```

#### 仿函数

```
C语言使用函数指针和回调函数来实现仿函数，例如一个用来排序的函数可以这样使用仿函数

```

```
要使用 c++ stl 中的 仿函数，需使用  #include<functional> 头文件
1： 算术类 仿函数；
	+ :   plus<T>
	- :   minus<T>
	* :   multipiles<T>
	/ :   divides<T>
	% :   modulus<T>
	~ :   negate<T>     否定
	
```

```c++
#include <iostream>
#include <numeric>
#include <vector> 
#include <functional> 
using namespace std;
 
int main()
{
	int ia[] = { 1,2,3,4,5 };
	vector<int> iv(ia, ia + 5);
	//120
	cout << accumulate(iv.begin(), iv.end(), 1, multiplies<int>()) << endl;
	//15
	cout << multiplies<int>()(3, 5) << endl;
 
	modulus<int>  modulusObj;
	cout << modulusObj(3, 5) << endl; // 3 
	system("pause");
	return 0;
}
```



```
2：关系运算 类仿函数：
等于：equal_to<T>

        不等于：not_equal_to<T>

        大于：greater<T>

        大于等于：greater_equal<T>

        小于：less<T>

        小于等于：less_equal<T>

```

```c++
#include <iostream>
#include <algorithm>
#include<functional>
#include <vector> 
 
using namespace std;
 
template <class T>
class display
{
public:
	void operator()(const T &x)
	{
		cout << x << " ";
	}
};
int main()
{
	int ia[] = { 1,5,4,3,2 };
	vector<int> iv(ia, ia + 5);
	sort(iv.begin(), iv.end(), greater<int>());
	for_each(iv.begin(), iv.end(), display<int>());
	system("pause");
	return 0;
}
```



```
 3）逻辑运算仿函数

        逻辑与：logical_and<T>

        逻辑或：logical_or<T>

        逻辑否：logical_no<T>
                 
                 
```

#### 容器适配器

```
标准库提供了三种顺序容器适配器：queue(FIFO队列)、priority_queue(优先级队列)、
stack(栈)

什么是容器适配器？

   ”适配器是使一种事物的行为类似于另外一种事物行为的一种机制”，适配器对容器进行包装，使其表
现出另外一种行为。例如，stack<int, vector<int> >实现了栈的功能，但其内部使用顺序容器
vector<int>来存储数据。（相当于是vector<int>表现出了栈的行为）。

```

| 种类           | 默认顺序容器 | 可用顺序容器        | 说明                             |
| -------------- | ------------ | ------------------- | -------------------------------- |
| stack          | deque        | vector、list、deque |                                  |
| queue          | deque        | list、deque         | 基础容器必须提供push_front()运算 |
| priority_queue | vector       | vector、deque       | 基础容器必须提供随机访问功能     |

```
定义适配器
  1、初始化

        stack<int> stk(dep);

  2、覆盖默认容器类型

       stack<int,vector<int> > stk;

使用适配器

```

#### algorithm

``` c++
// 常用：
bool comp(int i ,int j){ return (i < j);}; 自定义比较函数传参达到自定义。

    
    
    
    
// 返回 第一个  大于或等于 val 的数的指针位置， 可以使用 * 解引用将 数取出。   
lower_bound(first , last , val , comp) 


    
    
    
// 返回 最后一个 大于 val 的数的指针位置，可以使用 * 解引用将其取出。
// 也 可以传入 迭代器 ，迭代器重载了 - 号，可以得出索引位置。
upper_bound(first , last ,val, comp)


    
    
    
//  排序，  对 [first ， last) 区间排序
sort(first , last , 仿函数 / 自定义比较函数)
//  greater<T>   

    
    
// 稳排 , 不改变元素的相对位置。
stable_sort(first , last , 仿函数 / 自定义比较函数)
    
    
    
//  部分排序，使用 vector ,deque ,array , a[]
partial_sort(first , middle, last , 仿函数 / 自定义比较函数)    
    
    
//  翻转， 传的参数是迭代器。
reverser(first, last)
   
    
    
    
//  去重， 
unique(first , last)
/*
会将数组中的重复元素全部移到到数组的最后面，并且返回重复元素部分的 begin()
*/
// 把一个  vector 去重
vector<int> a;
unique(a.begin() , a.end());
int m = unique(a.begin(), a.end()) - a.begin();  // m 表示az






//  查找  在 [first ,end) 这个区间查找 值与 val 相等的值。 first和 end 为迭代器
//  返回值是 顺序找到的与 val 相等元素的迭代器。
find(first , end , const T& val)

    
    
    
    
//  根据指定规则在 [first , end)进行查找,第一个符合规则的数。
find_if(first , end ,mycomp)
find_if_not(first , end ,mycomp) //  查看第一个不符合 规则的数
    
// mycomp() 的两种实现
bool mycomp(int i)
{
    //  查找 % 2 = 1 的数
    return ((i % 2) == 1)
}
class mycomp{
    public:
    	bool operator()(const int& i){
            return ((i % 2 == 1));
        }
}

```

**<一>查找算法(13个)：判断容器中是否包含某个值**

```
adjacent_find() 
在iterator对标识元素范围内，查找一对相邻重复元素，找到则返回指向这对元素的第一个元素的        
ForwardIterator,否则返回last。重载版本使用输入的二元操作符代替相等的判断。


binary_search()
在有序序列中查找value，找到返回true。重载的版本实用指定的比较函数对象或函数指针来判断等。


count()           
利用等于操作符，把标志范围内的元素与输入值比较，返回相等元素个数。


count_if()       
利用输入的操作符，对标志范围内的元素进行操作，返回结果为true的个数。


equal_range()
功能类似equal，返回一对iterator，第一个表示lower_bound，第二个表示upper_bound。


find()               
利用底层元素的等于操作符，对指定范围内的元素与输入值进行比较。当匹配时，结束搜索，返回该元素的                    
一个InputIterator。


find_end()          
在指定范围内查找"由输入的另外一对iterator标志的第二个序列"的最后一次出现。找到则返回最后
一对的第一个ForwardIterator，否则返回输入的"另外一对"的第一个ForwardIterator。重载版
本使用用户输入的操作符代替等于操作。



find_first_of()
在指定范围内查找"由输入的另外一对iterator标志的第二个序列"中任意一个元素的第一次出现。
重载版本中使用了用户自定义操作符。


find_if()
使用输入的函数代替等于操作符执行find。


lower_bound(first , last , val) 
返回 第一个  大于或等于 val 的数的指针位置， 可以使用 * 解引用将 数取出。


upper_bound(first , last ,val)
返回 最后一个 大于 val 的数的指针位置，可以使用 * 解引用将其取出。




search()
给出两个范围，返回一个ForwardIterator，查找成功指向第一个范围内第一次出现子序列
(第二个范围)的位置，查找失败指向last1。重载版本使用自定义的比较操作。



search_n()
在指定范围内查找val出现n次的子序列。重载版本使用自定义的比较操作。

```

**<二>排序和通用算法(14个)：提供元素排序策略**

```
inplace_merge
合并两个有序序列，结果序列覆盖两端范围。重载版本使用输入的操作进行排序。


merge
合并两个有序序列，存放到另一个序列。重载版本使用自定义的比较。


nth_element
将范围内的序列重新排序，使所有小于第n个元素的元素都出现在它前面，而大于它的都出现在后面。重 
载版本使用自定义的比较操作。


partial_sort
对序列做部分排序，被排序元素个数正好可以被放到范围内。重载版本使用自定义的比较操作。


partial_sort_copy
与partial_sort类似，不过将经过排序的序列复制到另一个容器。



partition
对指定范围内元素重新排序，使用输入的函数，把结果为true的元素放在结果为false的元素之前。


random_shuffle
对指定范围内的元素随机调整次序。重载版本输入一个随机数产生操作。


reverse
将指定范围内元素重新反序排序


reverse_copy
与reverse类似，不过将结果写入另一个容器。


rotate
将指定范围内元素移到容器末尾，由middle指向的元素成为容器第一个元素。


rotate_copy
与rotate类似，不过将结果写入另一个容器。


sort
以升序重新排列指定范围内的元素。重载版本使用自定义的比较操作。


stable_sort
与sort类似，不过保留相等元素之间的顺序关系。


stable_partition
与partition类似，不过不保证保留容器中的相对顺序。

```

**<三>删除和替换算法(15个)**

```
copy
复制序列


copy_backward
与copy相同，不过元素是以相反顺序被拷贝。


iter_swap
交换两个ForwardIterator的值。


remove
删除指定范围内所有等于指定元素的元素。注意，该函数不是真正删除函数。内置函数不适合使用
remove和remove_if函数。


remove_copy
将所有不匹配元素复制到一个制定容器，返回OutputIterator指向被拷贝的末元素的下一个位置。


remove_if
删除指定范围内输入操作结果为true的所有元素。


remove_copy_if
将所有不匹配元素拷贝到一个指定容器。

replace
将指定范围内所有等于vold的元素都用vnew代替。

replace_copy
与replace类似，不过将结果写入另一个容器。


replace_if
将指定范围内所有操作结果为true的元素用新值代替。


replace_copy_if
与replace_if，不过将结果写入另一个容器。


swap
交换存储在两个对象中的值。


swap_range
将指定范围内的元素与另一个序列元素值进行交换。

unique
清除序列中重复元素，和remove类似，它也不能真正删除元素。重载版本使用自定义比较操作。


unique_copy
与unique类似，不过把结果输出到另一个容器。

```





```c++
	string
        构造函数：
        string s(char * a);    // 将字符数组转换为字符串， 
		string s(int n,char c);   // 用 n 个字符 c 来初始化 s
		
		常用成员函数：
        char&  at(int n);    // 返回 索引 n 位置的 字符
		char*  c_str()   //  返回一个以 null 为 终止的 c 字符串
        char*  data()    //  返回一个以 null 为终止的 c 字符数组   
            
    //把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目
    int copy(char *s, int n ,int pos) 
		
            
           
```

```c++
#include<iostream>
#include<string.h>

int main(){
    char s[] = {'a','b','c','\0'};
    
    string p1(s);
    
    string p2(10,'c');
    
    
    return 0;
}
```



## 详解

### c++关联式容器

| 关联式容器名称 |                             特点                             |
| -------------- | :----------------------------------------------------------: |
| map            | 定义在 <map> 头文件中，使用该容器存储的数据，其各个元素的键必须是唯一的（即不能重复），该容器会根据各元素键的大小，默认进行升序排序（调用 std::less<T>）。 |
| set            | 定义在 <set> 头文件中，使用该容器存储的数据，各个元素键和值完全相同，且各个元素的值不能重复（保证了各元素键的唯一性）。该容器会自动根据各个元素的键（其实也就是元素值）的大小进行升序排序（调用 std::less<T>）。 |
| multimap       | 定义在 <map> 头文件中，和 map 容器唯一的不同在于，multimap 容器中存储元素的键可以重复。 |
| multiset       | 定义在 <set> 头文件中，和 set 容器唯一的不同在于，multiset 容器中存储元素的值可以重复（一旦值重复，则意味着键也是重复的）。 |

#### map

http://c.biancheng.net/view/7173.html

**模板参数**

```c++
template < class Key, 	   // 指定键（key）的类型
           class T,        // 指定值（value）的类型
           class Compare = less<Key>,                     // 指定排序规则
           class Alloc = allocator<pair<const Key,T> >  // 指定分配器对象的类型
           > 
class map;
```

```c++
默认的  map 是按照 键的升序排列。
对于 string 是默认按照字典序来排列。

//  降序 int 排列
map<int,string,greater<int>> mp;

//  倒字典序 string 排列
map<string , int ,greater<string>> mp;

```



```c++
#include<map>
#include<iostream>
#include<cstring>
#include<vector>
using namespace std;


int main()
{
	
}
```









### c++ 无序容器

又称哈希容器。

底层使用 哈希表，相比 红黑树 更快。

| 无序容器           | 功能                                                         |
| ------------------ | ------------------------------------------------------------ |
| unordered_map      | 存储键值对 <key, value> 类型的元素，其中各个键值对键的值不允许重复，且该容器中存储的键值对是无序的。 |
| unordered_multimap | 和 unordered_map 唯一的区别在于，该容器允许存储多个键相同的键值对。 |
| unordered_set      | 不再以键值对的形式存储数据，而是直接存储数据元素本身（当然也可以理解为，该容器存储的全部都是键 key 和值 value 相等的键值对，正因为它们相等，因此只存储 value 即可）。另外，该容器存储的元素不能重复，且容器内部存储的元素也是无序的。 |
| unordered_multiset | 和 unordered_set 唯一的区别在于，该容器允许存储值相同的元素。 |

#### unordered_map

```c++
// 模板参数
template < class Key,                        //键值对中键的类型
           class T,                          //键值对中值的类型
           class Hash = hash<Key>,           //容器内部存储键值对所用的哈希函数
           class Pred = equal_to<Key>,       //判断各个键值对键相同的规则
           class Alloc = allocator< pair<const Key,T> >  // 指定分配器对象的类型
           > class unordered_map;

```

| 参数                 | 含义                                                         |
| -------------------- | ------------------------------------------------------------ |
| <key,T>              | 前 2 个参数分别用于确定键值对中键和值的类型，也就是存储键值对的类型。 |
| Hash = hash<Key>     | 用于指明容器在存储各个键值对时要使用的哈希函数，默认使用 STL 标准库提供的 hash<key> 哈希函数。注意，默认哈希函数只适用于基本数据类型（包括 string 类型），而不适用于自定义的结构体或者类。 |
| Pred = equal_to<Key> | 要知道，unordered_map 容器中存储的各个键值对的**键是不能相等的**，而判断是否相等的规则，就由此参数指定。默认情况下，使用 STL 标准库中提供的 equal_to<key> 规则，该规则仅支持可直接用 == 运算符做比较的数据类型。 |

**方法**

| 成员方法           | 功能                                                         |
| ------------------ | ------------------------------------------------------------ |
| begin()            | 返回指向容器中第一个键值对的正向迭代器。                     |
| end()              | 返回指向容器中最后一个键值对之后位置的正向迭代器。           |
| cbegin()           | 和 begin() 功能相同，只不过在其基础上增加了 const 属性，即该方法返回的迭代器不能用于修改容器内存储的键值对。 |
| cend()             | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，即该方法返回的迭代器不能用于修改容器内存储的键值对。 |
| **empty()**        | 若容器为空，则返回 true；否则 false。                        |
| **size()**         | 返回当前容器中存有键值对的个数。                             |
| max_size()         | 返回容器所能容纳键值对的最大个数，不同的操作系统，其返回值亦不相同。 |
| operator[key]      | 该模板类中重载了 [] 运算符，其功能是可以向访问数组中元素那样，只要给定某个键值对的键 key，就可以获取该键对应的值。注意，如果当前容器中没有以 key 为键的键值对，则其会使用该键向当前容器中插入一个新键值对。 |
| at(key)            | 返回容器中存储的键 key 对应的值，如果 key 不存在，则会抛出 out_of_range 异常。 |
| **find(key)**      | 查找以 key 为键的键值对，如果找到，则返回一个指向该键值对的正向迭代器；反之，则返回一个指向容器中最后一个键值对之后位置的迭代器（如果 end() 方法返回的迭代器）。 |
| **count(key)**     | 在容器中查找以 key 键的键值对的个数。                        |
| equal_range(key)   | 返回一个 pair 对象，其包含 2 个迭代器，用于表明当前容器中键为 key 的键值对所在的范围。 |
| **emplace()**      | 向容器中添加新键值对，效率比 insert() 方法高。               |
| emplace_hint()     | 向容器中添加新键值对，效率比 insert() 方法高。               |
| **insert()**       | 向容器中添加新键值对。                                       |
| **erase()**        | 删除指定键值对。                                             |
| **clear()**        | 清空容器，即删除容器中存储的所有键值对。                     |
| swap()             | 交换 2 个 unordered_map 容器存储的键值对，前提是必须保证这 2 个容器的类型完全相等。 |
| bucket_count()     | 返回当前容器底层存储键值对时，使用桶（一个线性链表代表一个桶）的数量。 |
| max_bucket_count() | 返回当前系统中，unordered_map 容器底层最多可以使用多少桶。   |
| bucket_size(n)     | 返回第 n 个桶中存储键值对的数量。                            |
| bucket(key)        | 返回以 key 为键的键值对所在桶的编号。                        |
| load_factor()      | 返回 unordered_map 容器中当前的负载因子。负载因子，指的是的当前容器中存储键值对的数量（size()）和使用桶数（bucket_count()）的比值，即 load_factor() = size() / bucket_count()。 |
| max_load_factor()  | 返回或者设置当前 unordered_map 容器的负载因子。              |
| rehash(n)          | 将当前容器底层使用桶的数量设置为 n。                         |
| reserve()          | 将存储桶的数量（也就是 bucket_count() 方法的返回值）设置为至少容纳count个元（不超过最大负载因子）所需的数量，并重新整理容器。 |
| hash_function()    | 返回当前容器使用的哈希函数对象。                             |

#### unordered_map 的一些常用操作和注意事项

```c++
//     1：定义：
unordered_map<int,int> mp;

 //    2：元素插入：  key 值不可重复 ,
使用 [] 
    mp[10] = 1;
    mp[10] = 2;       //  会覆盖  mp[10] = 1;
    
使用  emplace() 不能在 已有的 key 下插入 value
	mp.emplace(10,3)      //  不能覆盖 mp[10] = 2
	
使用  insert()   不能在 已有的 key 下插入 value
	mp.insert({10 ,4})    //  不能覆盖 mp[10] = 2

//     3：元素迭代：

for(auto [k , v] : mp)
	cout << k << " : " << v << endl; 

for(auto i = mp.begin() ; i != mp.end() ; i++)
	cout << (*i).first << " : " << (*i).second << endl;
	// cout << i -> first << " : " << i -> second << endl;
 
//     4： 元素清空 : 清空所有元素
mp.clear();


//     5： 判断容器是否为 空  , 不空 返回 0
cout << mp.empty();


//     6： 查找元素 按 "键" 查找,  返回的是 迭代器指针
unordered_map<int,int>::iterator x = mp.find(1) 
//  auto x = mp.find(1);
cout << x -> first << " : " << x -> second;


//     7:  元素删除
mp.earse(mp.begin());   //  删除 当前迭代器指针的键值对
mp.earse(1);            //  删除 以 1 作为 键的 键值对
mp.earse(mp.begin(), mp.end());  // 删除迭代器之间的所有元素
```

```c++
#include<iostream>
#include<unordered_map>
using namespace std;
int main()
{
    unordered_map<int,string> mp;
    
    // 元素插入
    mp[1] = "houdong";
    mp[2] = "hou";
    mp[3] = "hu";
    
    // 无法覆盖
    mp.insert({1 , "h"});
    mp.emplace(1 , "h");
    
    // 普通插入
    mp.insert({4, "ho"});
    mp.emplace(5 , "hod");
    
   	// 元素遍历
    for(auto [k , v] : mp)
        cout << k << " : " << v;
    
    // 元素查找
    auto x = mp.find(1);
    cout << (*x).first << " : " << (*x).second << endl;
    
    return 0;
}


```

#### unordered_multimap

与 unordered_map 唯一的不同是  **可以存储多个键相等的键值对**

```
所在头文件
#include<unordered_map>
```

```c++
需要自定义 哈希函数 和 比较函数
```

#### unordered_set

1. 不再以键值对的形式存储数据，而是直接存储数据的值；
2. 容器内部存储的各个元素的值都互不相等，且不能被修改。

```c++
#include<iostream>
#include<unordered_set>
using namespace std;

int main()
{
	unordered_set<string> mset;
    
    
    return 0;
}
```

#### unordered_multiset

```

```



### c++序列式容器

![](image/RongQi.jpg)

- array<T,N>（数组容器）：表示可以存储 N 个 T 类型的元素，是 [C++](http://c.biancheng.net/cplus/) 本身提供的一种容器。此类容器一旦建立，其长度就是固定不变的，这意味着不能增加或删除元素，只能改变某个元素的值；
- vector<T>（向量容器）：用来存放 T 类型的元素，是一个长度可变的序列容器，即在存储空间不足时，会自动申请更多的内存。使用此容器，在尾部增加或删除元素的效率最高（时间复杂度为 O(1) 常数阶），在其它位置插入或删除元素效率较差（时间复杂度为 O(n) 线性阶，其中 n 为容器中元素的个数）；
- deque<T>（双端队列容器）：和 vector 非常相似，区别在于使用该容器不仅尾部插入和删除元素高效，在头部插入或删除元素也同样高效，时间复杂度都是 O(1) 常数阶，但是在容器中某一位置处插入或删除元素，时间复杂度为 O(n) 线性阶；
- list<T>（链表容器）：是一个长度可变的、由 T 类型元素组成的序列，它以双向链表的形式组织元素，在这个序列的任何地方都可以高效地增加或删除元素（时间复杂度都为常数阶 O(1)），但访问容器中任意元素的速度要比前三种容器慢，这是因为 list<T> 必须从第一个元素或最后一个元素开始访问，需要沿着链表移动，直到到达想要的元素。
- forward_list<T>（正向链表容器）：和 list 容器非常类似，只不过它以单链表的形式组织元素，它内部的元素只能从第一个元素开始访问，是一类比链表容器快、更节省内存的容器。

注意，其实除此之外，stack<T> 和 queue<T> 本质上也属于序列容器，只不过它们都是在 deque 容器的基础上改头换面而成，通常更习惯称它们为容器适配器。



 **array、vector 和 deque 容器的函数成员**

| 函数成员         | 函数功能                                                     |
| ---------------- | ------------------------------------------------------------ |
| begin()          | 返回指向容器中第一个元素的迭代器。                           |
| end()            | 返回指向容器最后一个元素所在位置后一个位置的迭代器，通常和 begin() 结合使用。 |
| rbegin()         | 返回指向最后一个元素的迭代器。                               |
| rend()           | 返回指向第一个元素所在位置前一个位置的迭代器。               |
| cbegin()         | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| cend()           | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crbegin()        | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crend()          | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| assign()         | 用新元素替换原有内容。                                       |
| operator=()      | 复制同类型容器的元素，或者用初始化列表替换现有内容。         |
| size()           | 返回实际元素个数。                                           |
| max_size()       | 返回元素个数的最大值。这通常是一个很大的值，一般是 232-1，所以我们很少会用到这个函数。 |
| capacity()       | 返回当前容量。                                               |
| empty()          | 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。 |
| resize()         | 改变实际元素的个数。                                         |
| shrink _to_fit() | 将内存减少到等于当前元素实际所使用的大小。                   |
| front()          | 返回第一个元素的引用。                                       |
| back()           | 返回最后一个元素的引用。                                     |
| operator[]()     | 使用索引访问元素。                                           |
| at()             | 使用经过边界检査的索引访问元素。                             |
| push_back()      | 在序列的尾部添加一个元素。                                   |
| insert()         | 在指定的位置插入一个或多个元素。                             |
| emplace()        | 在指定的位置直接生成一个元素。                               |
| emplace_back()   | 在序列尾部生成一个元素。                                     |
| pop_back()       | 移出序列尾部的元素。                                         |
| erase()          | 移出一个元素或一段元素。                                     |
| clear()          | 移出所有的元素，容器大小变为 0。                             |
| swap()           | 交换两个容器的所有元素。                                     |
| data()           | 返回指向容器中第一个元素的指针。                             |

list 和 forward_list 容器彼此非常相似，forward_list 中包含了 list 的大部分成员函数，而未包含那些需要反向遍历的函数。表 3 展示了 list 和 forward_list 的函数成员。

**list 和 forward_list 的函数成员**

| 函数成员        | 函数功能                                                     |
| --------------- | ------------------------------------------------------------ |
| begin()         | 返回指向容器中第一个元素的迭代器。                           |
| end()           | 返回指向容器最后一个元素所在位置后一个位置的迭代器。         |
| rbegin()        | 返回指向最后一个元素的迭代器。                               |
| rend()          | 返回指向第一个元素所在位置前一个位置的迭代器。               |
| cbegin()        | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| before_begin()  | 返回指向第一个元素前一个位置的迭代器。                       |
| cbefore_begin() | 和 before_begin() 功能相同，只不过在其基础上，增加了 const 属性，即不能用该指针修改元素的值。 |
| cend()          | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crbegin()       | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crend()         | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| assign()        | 用新元素替换原有内容。                                       |
| operator=()     | 复制同类型容器的元素，或者用初始化列表替换现有内容。         |
| size()          | 返回实际元素个数。                                           |
| max_size()      | 返回元素个数的最大值，这通常是一个很大的值，一般是 232-1，所以我们很少会用到这个函数。 |
| resize()        | 改变实际元素的个数。                                         |
| empty()         | 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。 |
| front()         | 返回容器中第一个元素的引用。                                 |
| back()          | 返回容器中最后一个元素的引用。                               |
| push_back()     | 在序列的尾部添加一个元素。                                   |
| push_front()    | 在序列的起始位置添加一个元素。                               |
| emplace()       | 在指定位置直接生成一个元素。                                 |
| emplace_after() | 在指定位置的后面直接生成一个元素。                           |
| emplace_back()  | 在序列尾部生成一个元素。                                     |
| cmplacc_front() | 在序列的起始位生成一个元素。                                 |
| insert()        | 在指定的位置插入一个或多个元素。                             |
| insert_after()  | 在指定位置的后面插入一个或多个元素。                         |
| pop_back()      | 移除序列尾部的元素。                                         |
| pop_front()     | 移除序列头部的元素。                                         |
| reverse()       | 反转容器中某一段的元素。                                     |
| erase()         | 移除指定位置的一个元素或一段元素。                           |
| erase_after()   | 移除指定位置后面的一个元素或一段元素。                       |
| remove()        | 移除所有和参数匹配的元素。                                   |
| remove_if()     | 移除满足一元函数条件的所有元素。                             |
| unique()        | 移除所有连续重复的元素。                                     |
| clear()         | 移除所有的元素，容器大小变为 0。                             |
| swap()          | 交换两个容器的所有元素。                                     |
| sort()          | 对元素进行排序。                                             |
| merge()         | 合并两个有序容器。                                           |
| splice()        | 移动指定位置前面的所有元素到另一个同类型的 list 中。         |
| splice_after()  | 移动指定位置后面的所有元素到另一个同类型的 list 中。         |



```c++
//   vector
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>
#include <vector>
using namespace std;
/*
    系统为某一程序分配空间时，所需时间与空间大小无关。与申请次数有关。
    所以我们的在使用 vector 的时候要控制我们的访问次数，宁可浪费空间的扩大数组

*/
typedef std::vector<int>::iterator it;

int main(){
    // vector 的创建
    vector<int> a;              // 动态数组 
    vector<int> a1(10);         // 长度为 10 的数组
    vector<int> a2(10,3);       //  长度为 10 ，每个元素初始化为 3
    vector<int>  arr[10];       // 定义了 10 个 vector，vector数组


    // 函数
    cout<<a2.size()<<endl;   //返回元素个数

    cout<<a2.empty()<<endl;  // 是否为 空

    // a2.clear();        // 清空 vector

    cout<<a2.front()<<endl;       // 返回 vector 的第一个数

    cout<<a2.back()<<endl;        // 返回 vector 的最后一个数

    a2.push_back(2);   // 向 a2 的后面添加一个数 

    // a2.pop_back();     // 将最后一个数弹出

    // 迭代器
    a2.begin();       // a2 的第 0 个数

    a2.end();         // a2 的第 n+1 个数 

    // 支持比较运算  比较方式就是按照 字典方式一个
    vector<int> ar(10,2),ar1(9,3);

    if(ar < ar1) puts("Yes ");



    // 遍历方式
    for(int i=0;i<a2.size();i++) cout<<a2[i]<<' ';

    // iterator 迭代器
    for(vector<int>::iterator i = a2.begin() ;i != a2.end() ; i++) 		
        cout<<*i<<' ';

    for(auto i = a2.begin() ;i != a2.end() ; i++) cout<<*i<<' ';

    // 增强 for 循环
    for(auto x : a2) cout<<x<<' ';

    return 0;
}
```

```c++
//  pair<type,type>

1：也可以进行比较，并且也是字典序，first为第一关键字，second 为第二关键字。
    所以我们需要对某些元素排序时，并且另一个元素不需要排序，我们可以将其放到这里面
      eg:pair<strind,int> p;      string ：人名   int ：年龄   
	
2：套娃操作
      用来存储三种类型
        pair<int,pair<string,int>>  p;

int main(){
    pair<int,string> p;

    p.first  = 10;
    p.second = "houdong";

    p = make_pair(100,"hello");

    p = {1000,"abc"};

    cout<<p.first;

    return 0;
}
```

```c++
int main(){
    string  str = "houdong";

    str += "dong";

    str += 'a';

    cout<<str.substr(1)<<endl; //oudongdonga  

    printf("%s\n",str.c_str());  //houdongdonga  

    return 0;
}
```

```c++
int main(){
    map<int,string> m;
    m.insert({10,"houdong"});
    m[1] = "hu";
    cout<<m[1];

    map<string,int> m1;
    m1["hou"] = 1;
    cout<<m1["hou"];

    return 0;
}
```

