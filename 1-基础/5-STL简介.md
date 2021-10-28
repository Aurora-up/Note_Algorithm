[TOC]



### STL   简介

```c++
vector, 变长数组，倍增的思想
    size()  返回元素个数
    empty()  返回是否为空，返回的是 int 类型
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

    
    
pair<int, int>    其中两个元素的类型可以是任意的   ----->   类似于帮助我们实现一个二元结构体，并且加了一个比较器
    
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）
    也可以通过函数 make_pair(type_var,type_var)来为 pair 赋值
    
    
string，字符串
    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址

    
    
    
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
     
    
    
priority_queue, 优先队列，默认是大根堆   ---->   t
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    
    对于数字来来说，我们可以在插入的时候插入负数即可，输出的时候输出 x 即可
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;
	



stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

    
    
    
deque, 双端队列 ，支持队头队尾的插入删除，也支持随意访问。加强版的 vector,但是速度很慢
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

    
    
    
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

    
    
    // 无序的
unordered_set, unordered_map, unordered_multiset, unordered_multimap,    基于哈希表
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

    
    
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

```c++
	string
        构造函数：
        string s(char * a);    // 将字符数组转换为字符串， 
		string s(int n,char c);   // 用 n 个字符 c 来初始化 s
		
		常用成员函数：
        char&  at(int n);    //
		char&
            
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

