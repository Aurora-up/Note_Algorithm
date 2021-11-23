[TOC]



### STL   简介

#### vector

```
vector, 变长数组，倍增的思想
    size()  返回元素个数
    empty()  返回是否为空，返回的是 int 类型
    clear()  清空
    front()/back()
    push_back()/pop_back()
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
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()
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

```c++
// 常用：
bool comp(int i ,int j){ return (i < j)}; 自定义比较函数传参达到自定义。

    
// 返回 第一个  大于或等于 val 的数的指针位置， 可以使用 * 解引用将 数取出。   
lower_bound(first , last , val , comp) 


// 返回 最后一个 大于 val 的数的指针位置，可以使用 * 解引用将其取出。
// 也 可以传入 迭代器 ，迭代器重载了 - 号，可以得出索引位置。
upper_bound(first , last ,val, comp)


sort(first , last , 仿函数 / 自定义比较函数)
//  greater<T>   


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
    for(vector<int>::iterator i = a2.begin() ;i != a2.end() ; i++) cout<<*i<<' ';

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

