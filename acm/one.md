## 目录
  - [常用语句](#一常用语句)
  - [string字符串](#二string字符串)
  - [常用库函数](#三常用库函数)
  - [STL库](#四STL库)

## （一）常用语句

### 1.交互题刷新缓冲区

使用scanf/printf时：`fflush(stdout);`

使用cin/cout时：`cout<<flush;`

在c++中输出endl会自动刷新缓冲区。

交互题中c++不能取消同步流，否则会读取不到输出内容。

### 2.取消同步流

`ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);`

### 3.声明参数类型

以“将long long声明成ll”为例，声明后二者可同时使用：

`typedef long long ll;`

### 4.输出特定格式的数

c++中：

`cout << fixed << setprecision(5) << a;`

保留五位小数

c语言中：

`printf("%5d", 10);`（输出“   10”）

设置最小输出长度。若实际数据小于五位，向右对齐，左侧补空格

`printf("%-5d", 10);`（输出“10   ”）

设置最小输出长度。若实际数据小于五位，向左对齐，右侧补空格

`printf("%05d", 10);`（输出“00010”）

指定最小数字位数。若实际位数不够，前面补0

`printf("%.2f", 3.14159);`（输出“3.14”）

`printf("%.2f", 3.1);`（输出“3.10”）

保留特定位数的小数（默认是6位）

`printf("%+d", 10);`（输出“+10”）

强制输出正负号，例如5输出+5，-5输出-5

### 5.读取整行内容

```c++
getchar();//先缓冲掉换行符
string s;
getline(cin, s);
```

## （二）string字符串

### 1.初始化

```c++
string s1;             //空字符串
string s2 = "hello";   //直接初始化
char ch[100] = "world";
string s3(ch);         //构造函数初始化
string s4(5, 'a');     //生成5个'a'的字符串"aaaaa"
string s5(s2);         //拷贝构造，也可以string s5 = s2;
```

### 2.获取长度

```c++
string s = "hello";
int len1 = s.size();
int len2 = s.length();  //两种写法完全等价，len1=len2=5
```

### 3.字符串拼接

```c++
string s1 = "hello", s2 = "world";
string s3 = s1 + s2;          //"helloworld"
string s4 = s1 + " " + s2;    //"hello world"
s1 += s2;                     //s1变为"helloworld"
s1.append(3, '!');            //在s1末尾添加3个'!'
```

### 4.字符串插入

```c++
string s = "hello";
s.insert(2, "xxx");    // 在位置2插入："hexxxllo"
s.insert(2, 3, 'x');   // 在位置2插入3个'x'，作用同上
```

### 5.字符串删除

```c++
string s = "hello world";
s.erase(5);          // 删除从位置5开始到末尾："hello"
s.erase(5, 3);       // 从位置5开始删除3个字符："hello world" → "hellorld"
s.erase(s.begin() + 2); // 删除s[2]
s.erase(s.begin(), s.begin() + 3); // 删除前3个字符（区间左闭右开的部分）
s.clear();           // 清空字符串
```

### 6.字符串替换

```c++
string s = "hello world";
s.replace(6, 5, "xxx");  // 从s[6]开始，替换5个字符为"xxx"："hello xxx"
s.replace(6, 5, "abcde", 3); // 替换为"abcde"的前3个字符："hello abc"
```

### 7.字符串查找

```c++
string s = "hello world hello";
size_t pos1 = s.find("hello");     // 返回第一次出现的位置：0
size_t pos2 = s.find("hello", 1);  // 从位置1开始找：12
size_t pos3 = s.find("abc");       // 找不到返回string::npos
if(pos3 != string::npos)    //这样输出比较保险
{
        cout<<pos3;
}
```

### 8.提取子串

```c++
string s = "hello world";
string sub1 = s.substr(6);      // "world"（从位置6到末尾）
string sub2 = s.substr(6, 3);   // "wor"（从位置6开始，取3个字符，注意别越界）
```

### 9.比较大小

string可以直接用大于/小于/等于号进行处理，依据字典序比较。

## （三）常用库函数

### 1.排序

默认为升序排序。时间复杂度为O( n log n )。

```c++
int arr[100];
int n; cin >> n;
for (int i = 1; i <= n; i++)cin >> arr[i];
sort(arr + 1, arr + 1 + n);
```

对数值排序时按升序排序，对bool类型排序时false(0)在前true(1)在后，

对char类型排序时按ASCII码升序，对string字符串排序时按字典序升序，

对pair类型时先按first升序，若相同再按second升序。



也可自定义排序函数，应设为bool类型，如：

```c++
bool big(int x, int y)
{
	return x > y;//符合要求则返回1，不符合则返回0
}
sort(arr, arr + n, big);
//对int类型数组，按降序排序


bool cmp(const pair<int, int>& a, const pair<int, int>& b)
{
    //这样写不用复制临时变量，性能更好
    //用(pair<int,int>a,pair<int,int>b)也不是不行
    if (a.second != b.second)return a.second > b.second;
    return a.first < b.first;
}
sort(arr.begin(),arr.end(),cmp);
//对pair类型数组，优先按second降序，其次按first升序
```

### 2.交换元素

`swap(a,b);   swap(arr[0], arr[4]);`

时间复杂度O( 1 )。

适用于两个相同类型元素，如int、pair、string、vector等。

不能用于内置数组如int a[3]的整个交换。

### 3.最值查找

**(1)max、min**

`int a=max(3,5);`时间复杂度O(1)

`int b=min({3,7,9,2,5});`时间复杂度O(n)。

**(2)max_element、min_element**

```c++
vector<int> v = { 3, 1, 4, 1, 5, 9 };
auto max_it = max_element(v.begin(), v.end());
int mx = *max_it;      // mx = 9
auto min_it = min_element(v.begin(), v.end());
int mn = *min_it;      // mn = 1
```

传入两个迭代器[l,r)，返回一个迭代器

时间复杂度O(n)。

**(3)nth_element**

```c++
vector<int> v = {5, 10, 6, 4, 3, 2, 6, 7, 9, 3};
int k = 3;                 // 找第k+1小的元素（0-based）
nth_element(v.begin(), v.begin() + k, v.end());
cout << v[k] << endl;      //v[k] = 4 （因为下标从0开始算）
```

排序后其左侧一定比它小，右侧一定比它大，但不保证完全排好序

会修改原序列

时间复杂度平均O(n)，最坏O(n^2^)。

### 4.大小写字母检查和转换

```c++
isalpha(c);  // 是否是字母
islower(c);  // 是否是小写字母
isupper(c);  // 是否是大写字母
//传入char类型，返回bool类型

tolower(c);  // 转小写（如果原本不是大写字母，保持不变）
toupper(c);  // 转大写（如果原本不是小写字母，保持不变）
//传入char类型，返回int类型（ASCII码，可转成char）
```

时间复杂度均为O(1)。

### 5.全排列

```c++
int arr[] = { 3, 1, 2 };
int n = sizeof(arr) / sizeof(arr[0]);

while (next_permutation(arr, arr + n))
//若存在下一个排列，则将其变为下一个并返回true，否则返回false
{
    //处理数组
}

vector<int> v = {3, 2, 1};
while (prev_permutation(v.begin(), v.end()))
//若存在上一个排列，则将其变为上一个并返回true，否则返回false
{
    //处理数组
}
```

支持用在vector、内置数组、string、deque上。会修改原数组。

常用于生成所有排列。方法是先sort一下数组，再do{…}while(该函数)

单次时间复杂度O(n)。

### 6.初始化内存

`memset(arr, 0, sizeof(arr));`

初始化为0最安全，如果是整数类型也可以初始化为-1。其它数可能导致错误

时间复杂度O(n)。

### 7.反转区间元素

```c++
vector<int> v = {1, 2, 3, 4, 5};
reverse(v.begin(), v.end());
reverse(v.begin() + 1, v.begin() + 4);  //反转下标区间[1,3]，这样写的时候注意别越界
```

支持用在vector、内置数组、string、deque、list上。

时间复杂度O(n)。

### 8.去重

```c++
vector<int>vec = { 1,2,3,4,5,1,2,3,4,5 };
sort(vec.begin(), vec.end());  //用前必须先排序

// 去重，返回新的结束迭代器
auto last = unique(vec.begin(), vec.end());

int n = last - vec.begin();    //剩余元素个数（也可删除后用size()得到）
vec.erase(last, vec.end());
```

支持vector、内置数组、string、deque、list等容器。

时间复杂度O(n)。（但排序O( n log n )，因此去重的时间复杂度通常可忽略）

### 9.二分查找

```c++
vector<int> v = {1, 2, 4, 4, 5, 6};
sort(v.begin(), v.end());       //必须在有序数组中使用
int target = 4;

auto lb = lower_bound(v.begin(), v.end(), target);
cout << lb - v.begin() << endl; // 输出: 2

auto ub = upper_bound(v.begin(), v.end(), target);
cout << ub - v.begin() << endl; // 输出: 4
//若不存在，则返回v.end()
```

常用于统计某元素出现次数（ub-lb）、离散化（lb-begin()）、查找某元素（保证存在）最后一次出现的位置（ub-1）、查找某元素插入位置（配合insert使用）等。

支持用于vector、内置数组、string、deque中。

时间复杂度O( log n )。

## （四）STL库

### 1.pair

用于储存一对值的集合，有成员变量first和second。

```c++
pair<int, int> p1;                     // 默认初始化，两个int均为0
pair<int, string> p2(1, "hello");      // 直接初始化
pair<int, double> p3 = {2, 3.14};      // 列表初始化

cout << p2.first << " " << p2.second << endl;  // 输出: 1 hello
```

pair可以嵌套。

`pair<int,pair<int,double>>p;`

可存入vector，排序时默认“优先按first升序，若相同则按second升序”。也可自定义比较函数。

`vector<pair<int, string>> vec;`

### 2.vector

一个动态大小的数组容器，可储存一系列相同类型的元素。

```c++
vector<int> v1;                // 空vector
vector<int> v2(10);            // 大小为10，默认值0
vector<int> v3(5, 100);        // 大小为5，每个元素都是100
vector<int> v4 = {1, 2, 3};    // 列表初始化
vector<vector<int>> v5;        // 二维vector
```

支持操作：

|       操作        |          作用          | 时间复杂度 |
| :---------------: | :--------------------: | :--------: |
|       v[i]        |        下标访问        |    O(1)    |
|   push_back(x)    |     插入元素到末尾     |  平均O(1)  |
|    pop_back()     |      删除末尾元素      |    O(1)    |
|     insert()      |      任意位置插入      |    O(n)    |
|      erase()      |      任意位置删除      |    O(n)    |
|      size()       |   返回数组大小(int)    |    O(1)    |
|      empty()      | 检查是否为空(返回bool) |    O(1)    |
|      clear()      |        清空数组        |    O(n)    |
|  begin() / end()  |     返回正向迭代器     |    O(1)    |
| rbegin() / rend() |     返回反向迭代器     |    O(1)    |
| front() / back()  |     访问首/尾元素      |    O(1)    |
|     resize()      |        调整大小        |    O(n)    |

```c++
vector<int>vec = { 1,2,3 };
for (auto i = vec.begin(); i != vec.end(); i++)
{
    cout << *i << ' ';    //输出1 2 3
}
for (auto i = vec.rbegin(); i != vec.rend(); i++)
{
    cout << *i << ' ';    //输出3 2 1
}

vec.insert(vec.begin() + 2, 99);  //变成1 2 99 3
vec.erase(vec.begin() + 1);       //变成1 99 3
//还可传入两个地址l、r，删[l,r)

cout << vec.front();    //1（访问空数组时会出错）
vec.front()++;          //会改变原数组（变为2 99 3）
cout << vec.front();    //2

vec.resize(2);      //缩小到大小为2，变为“2 99”
vec.resize(4);      //扩大，默认填充0，变为“2 99 0 0”
vec.resize(6,8);    //扩大，填充8，变为“2 99 0 0 8 8”
```

### 3.stack

栈，一种“先进后出”的容器，只能操作栈顶元素。

`stack<int>s;`

支持操作：

|  操作   |          作用          | 时间复杂度 |
| :-----: | :--------------------: | :--------: |
| push(x) |      在栈顶插入x       |    O(1)    |
|  pop()  | 弹出栈顶元素（不返回） |    O(1)    |
|  top()  |      返回栈顶元素      |    O(1)    |
| empty() |      判断是否为空      |    O(1)    |
| size()  |      返回元素个数      |    O(1)    |

不能直接clear()，想清空只能一个个弹出。也没有迭代器。也不支持随机访问。

### 4.queue

队列，一种“先进先出”的数据结构。

`queue<int>q;`

支持操作：

|  操作   |          作用          | 时间复杂度 |
| :-----: | :--------------------: | :--------: |
| push(x) |      在队尾插入x       |    O(1)    |
|  pop()  | 弹出队首元素（不返回） |    O(1)    |
| front() |      返回队首元素      |    O(1)    |
| back()  |      返回队尾元素      |    O(1)    |
| empty() |      判断是否为空      |    O(1)    |
| size()  |      返回元素个数      |    O(1)    |

也没有clear()，没有迭代器，不支持随机访问。

### 5.priority_queue

优先队列，一种基于堆结构实现的容器，内部元素有序。

默认提供最大堆（大根堆），最大元素在队首，即降序：

`priority_queue<int>arr1;`

如果需要最小堆（小根堆），即升序容器，则需要：

`priority_queue<int, vector<int>, greater<int>>arr2;`

支持操作：

|  操作   |          作用          | 时间复杂度 |
| :-----: | :--------------------: | :--------: |
| push(x) |    插入x到优先队列     |  O(log n)  |
|  pop()  | 弹出队首元素（不返回） |  O(log n)  |
|  top()  |      返回队首元素      |    O(1)    |
| empty() |      判断是否为空      |    O(1)    |
| size()  |      返回元素个数      |    O(1)    |

也没有clear()，没有迭代器，不支持随机访问。

自定义比较函数时，返回的是“优先级”，返回true说明第二个参数优先级更高（更靠近队首），反之亦然。

return a<b   →   若true，b更大，优先级更高   →   b更靠近队首   →   最大堆（降序）

return a>b   →   若true，b更小，优先级更高   →   b更靠近队首   →   最小堆（升序）

### 6.deque

双端队列，支持在队列两端进行插入、删除等操作。

```c++
deque<int> dq1;                       // 空deque
deque<int> dq2(5);                    // 大小为5，默认值0
deque<int> dq3(5, 100);               // 大小为5，每个元素都是100
deque<int> dq4 = { 1, 2, 3, 4, 5 };   // 列表初始化
```

支持操作：

|       操作       |      作用       | 时间复杂度 |
| :--------------: | :-------------: | :--------: |
|  push_front(x)   | 在队头插入元素  |    O(1)    |
|   push_back(x)   | 在队尾插入元素  |    O(1)    |
|   pop_front()    |  删除队头元素   |    O(1)    |
|    pop_back()    |  删除队尾元素   |    O(1)    |
| front() / back() | 访问队首/尾元素 |    O(1)    |
|     insert()     |  任意位置插入   |    O(n)    |
|     erase()      |  任意位置删除   |    O(n)    |
|      dq[i]       |    下标访问     |    O(1)    |
|      size()      |  返回元素个数   |    O(1)    |
|     empty()      |  检查是否为空   |    O(1)    |
|     resize()     |    调整大小     |    O(n)    |
|     clear()      |    清空容器     |    O(n)    |

```c++
deque<int> dq = { 1,2,3,4,5 };
cout << dq[2] << '\n';         // 输出: 3
cout << dq.front() << ' ' << dq.back() << '\n';    // 输出: 1 5
dq.push_front(0);              // 变成 { 0,1,2,3,4,5 }
dq.pop_front();                // 变成 { 1,2,3,4,5 }
dq.push_back(6);               // 变成 { 1,2,3,4,5,6 }
dq.pop_back();                 // 变成 { 1,2,3,4,5 }
for (auto it = dq.begin(); it != dq.end(); ++it)
{
    cout << *it << " ";  //正向遍历（反向也支持）
} 
```

支持clear()、使用正向/反向迭代器遍历、下标随机访问。

很多操作语法类似vector，如insert、erase、resize等。

### 7.set

一种关联容器，用于储存一组不重复的元素，并按照特定顺序排列（默认是升序）。

插入重复元素时，set会自动忽略该元素。

```c++
set<int>arr1;                  //升序排序
set<int, greater<int>>arr2;    //降序排序
```

支持操作：



|      操作       |                             作用                             | 时间复杂度 |
| :-------------: | :----------------------------------------------------------: | :--------: |
|    insert(x)    |                          插入元素x                           |  O(log n)  |
|    erase(x)     |                   删除元素x(若没有则忽略)                    |  O(log n)  |
|     find(x)     |     查找元素x。<br />找到了返回对应迭代器，否则返回end()     |  O(log n)  |
|    count(x)     |                   返回x出现的次数（0或1）                    |  O(log n)  |
|     clear()     |                           清空容器                           | O(n log n) |
|     empty()     |                         检查是否为空                         |    O(1)    |
|     size()      |                         返回元素个数                         |    O(1)    |
| prev() / next() | 传入迭代器，返回其上一个/下一个迭代器。<br />使用时注意边界。可以先检查下。 |    O(1)    |
| lower_bound(x)  |     返回第一个>=x元素处的迭代器。<br />若无则返回end()。     |  O(log n)  |
| upper_bound(x)  |     返回第一个>x元素处的迭代器。<br />若无则返回end()。      |  O(log n)  |

也支持begin()/end()/rbegin()/rend()迭代器遍历，或auto遍历，操作方法与其他元素相同。
插入的元素不能再修改，只能删除再重新添加。

```c++
set<int> s = { 1,3,5,7,9 };
auto it = s.find(5);

auto next_it = next(it);
if (next_it != s.end())
{
    cout << *next_it << endl; // 输出7
}
if (it != s.begin()) 
{ 
    auto prev_it = prev(it);
    cout << *prev_it << endl; // 输出3
}
```

```c++
set<int> s = {1, 3, 5, 7};

auto ub1 = s.upper_bound(6); // 指向7（第一个>6的元素）
auto ub2 = s.upper_bound(7); // 返回end()（所有元素都<=7）
auto ub3 = s.upper_bound(0); // 指向1（第一个>0的元素）
```

### 8.multiset

与set类似，但允许重复元素的存在。容器内仍然保持有序。

各种操作与set类似。不同点有：

1.count返回的整数不再是“非0即1”，时间复杂度变成O(log n + k)。其中k是查找元素重复次数

   重复多次时，时间复杂度可能接近O(n)。

2.erase()删除时会删掉所有相同元素，时间复杂度变为O(log n + k)。

   若只想删掉一个元素，需要：

   `arr.erase(arr.find(x));`

3.find()返回找到的第一个迭代器。若没有仍返回end()。

### 9.unordered_set

与set类似，储存唯一元素，但内部元素无序。

插入、删除、查找、计数等操作时间复杂度平均O(1)，但最坏情况O(n)。

仍然能正向遍历，但遍历顺序不确定。不再支持反向遍历。

### 10.map

一种关联容器，用于储存“键值对”，其中每个“键”都是唯一的。

按照“键”的特定准则进行排序，默认是升序排序。

`map<int, string>m1;` 按“键”升序

`map<int, string, greater<int>>m2;` 按“键”降序

（自定义比较规则时，传参的数据类型是“键”的类型。）

支持操作：



|      操作      |                             作用                             | 时间复杂度 |
| :------------: | :----------------------------------------------------------: | :--------: |
| insert({a,b})  |                        插入元素{a,b}                         |  O(log n)  |
|  emplace(a,b)  |                        插入元素{a,b}                         |  O(log n)  |
|    erase(a)    |           删除以a为键的键值对。<br />若没有则不变            |  O(log n)  |
|    find(a)     |       查找以a为键的键值对。<br />返回对应迭代器或end()       |  O(log n)  |
|     mp[a]      | 传入a(“键”)，返回“值”。<br />若没有，则会创建值为默认(0或空)的元素 |  O(log n)  |
|    count(a)    |                返回以a为“键”的元素个数(0或1)                 |  O(log n)  |
|    empty()     |                         检查是否为空                         |    O(1)    |
|     size()     |                         返回元素个数                         |    O(1)    |
| lower_bound(a) |         返回首个“键”>=a的迭代器<br />没有则返回end()         |  O(log n)  |
| upper_bound(a) |         返回首个“键”>a的迭代器<br />没有则返回end()          |  O(log n)  |

也支持正向、反向迭代器的遍历。

### 11.multimap

multimap与map的区别，类似multiset和set的区别。multimap储存的”键值对“中”键“可以重复。

不支持用类似mp[i]的形式访问。

count()、erase()的时间复杂度变成O(log n + k)。

删除时会删除对应键的所有元素。若想只删除一个，需要：

`mp.erase(mp.find(x));`

若想查找某个键的所有元素，可以使用equal_range(a)。

```c++
auto range = mp.equal_range(2);
for (auto i = range.first; i != range.second; i++)
{
    cout << (*i).first << ' ' << (*i).second << '\n';
}
```

### 12.unordered_map

unordered_map和map的区别，类似unordered_set和set的区别。

插入等操作的时间复杂度为平均O(1)，最坏O(n)。

支持正向迭代器，不支持反向。

### 13.总结

支持lower_bound/upper_bound的容器：

vector、deque、arr（内置数组，排序后使用）、string（排序后使用）、set、map

其中set、map不支持 - begin() 获取区间大小的操作，其他的支持。

