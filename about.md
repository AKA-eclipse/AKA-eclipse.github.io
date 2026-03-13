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
