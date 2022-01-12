### Arrays 工具类

```java
System.arraycopy(Object src, int srcPos,Object dest, int destPos,int length);  // System类中拷贝数组的方法。
```

```java
// 排序
void parallelSort(xxx[] a) // 数字升序排列（数据量大时使用）
    
void parallelSort(xxx[] a ,int fromIndex,int toIndex)

void sort(xxx[] a) // 数字升序排列（数据量小时使用）
    
void sort(xxx[] a ,int fromIndex,int toIndex)
    
<T> void parallelSort(T[] a, Comparator<? super T> c)  //

<T> void sort(T[] a, Comparator<? super T> c)  // 指定比较器
```

```java
// 搜索
int binarySearch(xxx[] a , xxx  b); 
```

```java
// 比较
int compare(xxx_1[]  a  ,  xxx_2[]  b)
    
int compare(xxx_1[] a , int begin_1 , int end_2 , xxx_1[] b , int begin_2 , int end_2);//比较

int compareUnsigned(xxx_1[] a , int begin_1 , int end_2 , xxx_1[] b , int begin_2 , int end_2) // byte int long double short
    
    
```

```java
// 数组元素计算 , 可以在后面使用 Lamda 表达式作为函数

setAll(int[] a , IntUnaryOperator generator)

setAll(double[] a , IntToDoubleFunction generator)
   
setAll(long[] a , IntToLongFunction generator)
    
parallelSetAll(xxx[] a , Funcation generator)
```

```java
// 并行遍历
Spliterator.Ofxxx  spliterator(xxx[] array)

Spliterator.Ofxxx  spliterator(xxx[] array , int sta)
```



```java
<T> List<T>    asList(T... a)
     
xxx[] copyOf(xxx[] original,int newLength); // 拷贝

xxx[] copyRange(xxx[] original, int from , int to);   // q

void fill(xxx[] a , xxx b);   // 赋值

void fill(xxx[] a , int fromIndex ,int toIndex , xxx b); // 区间赋值

void mismatch(xxx[] a , xxx[] b);

void mismatch(xxx[] a , int aFromInde , int aToInde, xxx[] b ,int bFromInde , int bToInde);


```

