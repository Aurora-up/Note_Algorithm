## vector class

```c++
 vector 是表示可以改变大小的数组的序列容器。
     
  	1：就像数组一样，向量为它们的元素使用连续的存储位置,这意味着它们的元素也可以使用指向其元素的常规指针上的偏移量来访问，并且与在数组中一样有效。 但与数组不同的是，它们的大小可以动态变化，它们的存储由容器自动处理。    
     	
    2：所以 vector 就是一个动态连续数组。
    3：与数组相比，vector消耗更多内存以换取以高效方式管理存储和动态增长的能力
    4：在内部，vector使用动态分配的数组来存储它们的元素。 这个数组可能需要重新分配，以便在插入新元素时增加大小，这意味着分配一个新数组并将所有元素移动到它。 就处理时间而言，这是一项相对昂贵的任务，因此，每次将元素添加到容器时，vector都不会重新分配。
    5: 优势在于 有效的访问其元素，并且相对有效的从其 添加或删除元素（在末尾），
      对于涉及在末尾以外的位置插入或删除元素的操作，它们的性能比其他操作差，并且迭代器和引用的一致性不如 列表 和 forward_lists 。 
    
     
一：模板
template < class T, class Alloc = allocator<T> > class vector; // generic template


二：构造函数：

    
三：析构函数
 
    
四：成员函数
  1：迭代器：
    begin()       返回第 0 位数
    end()         返回第 n+1 位数
    rbegin()      reverse  begin   反向开始
    rend()        reverse  end     反向结束
    
    const 即仅仅只是遍历，不能去改变 vector 中的值
    cbegin()      const begin
    cend()        const end
    crbegin()     const reverse begin
    crend()       const reverse end
    
  2：容量
    size()         元素个数
    max_size()     设置最大元素个数
    resize()       重新设置元素个数
    capacity()     理论容量（由于是动态数组，所以都是底层根据 元素一定个数 所倍增出来的暂时容量）
    empty()        置空
    reverse()      将元素反转
    shrink_to_fit()  缩短尺寸，如果发生重新分配，则与容器相关的所有迭代器、指针和引用都将失效 否则，没有任何改变
    
   3:元素访问
     []        被重载的[],  vector<int> a; a[N] 去访问 第N个元素 
     at()      a.at(N) 访问的是 第N个元素。
     front()   首元素
     back()    最后一个元素
     data()    指针类型，或者常指针类型，指针类型可用于修改所指向的元素，常指针类可以用访问
       
   4:元素修改：
       增：puch_back() 
       删：
       	   pop_back()     删最后一个
       	   erase()        s
       	   clear()        清空
       
       插：insert()
	其他：
       swap()     交换
       
    分配：assign() 
       
       
       
       void assign (InputIterator first, InputIterator last);  放迭代器的值设定大小
	   void assign (size_type n, const value_type& val);        设定大小及初始值
       void assign (initializer_list<value_type> il);           

      1: a.assign(2,100);
	 2: 
			vector::iterator start_v,end_v;  start_v = a.begin()+1;   end_v =  a,end()-1;
		b.assign(start_v,end_v)
```

