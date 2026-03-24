# 第二章 基础算法

## (一)前缀和

### 1.一维

```c++
int a[11];
int pre[11];

for (int i = 1; i <= 10; i++)
{
	cin >> a[i];
	pre[i] = pre[i - 1] + a[i];
}

//求[l,r]元素和
int l=3,r=8;
cout<<pre[r]-pre[l-1];
```

### 2.二维

```c++
int arr[11][11];
int s[11][11];

for (int i = 1; i <= 10; i++)
{
	for (int j = 1; j <= 10; j++)cin >> arr[i][j];
}
for (int i = 1; i <= 10; i++)
{
	for (int j = 1; j <= 10; j++)
	{
		s[i][j] = arr[i][j] + s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
	}
}

int x1 = 3, y1 = 4, x2 = 8, y2 = 6;
//求(x1,y1)到(x2,y2)矩形区间的元素和
int sum = s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1];
cout << sum;
```

## (二)差分

### 1.一维

```c++
int arr[11];
int diff[11];

for (int i = 1; i <= 10; i++)
{
	cin >> arr[i];
    diff[i] = arr[i] - arr[i - 1];
}

int l = 4, r = 7;
diff[l] -= 2; diff[r] += 2;    //区间元素都-2
for (int i = 1; i <= 10; i++)
{
    arr[i] = arr[i - 1] + diff[i];
}
```

### 2.二维

```c++
int arr[11][11];
int diff[11][11];

for (int i = 1; i <= 10; i++)
{
    for (int j = 1; j <= 10; j++)
    {
        cin >> arr[i][j];
        diff[i][j] = arr[i][j] - arr[i - 1][j] - arr[i][j - 1] + arr[i - 1][j - 1];
    }
}

int x1 = 3, y1 = 4, x2 = 8, y2 = 6;
diff[x1][y1] += 2; diff[x2 + 1][y1] -= 2;
diff[x1][y2 + 1] -= 2; diff[x2 + 1][y2 + 1] += 2;
//(x1,y1)到(x2,y2)矩形区间内元素都+2

for (int i = 1; i <= 10; i++)
{
    for (int j = 1; j <= 10;j++)
    {
        arr[i][j] = diff[i][j] + arr[i][j - 1] + arr[i - 1][j] - arr[i - 1][j - 1];
    }
}
```

## (三)进制转换

### 1.x进制转十进制

将x进制数存入字符串，并对其处理得到十进制数。

```c++
int arr[5000];

int x;cin>>x;         //将要输入x进制数
string ch;cin>>ch;    //x进制数内容

for (int i = 0; i < ch.size(); i++)
{
	if (ch[i] <= '9')arr[i] = ch[i] - '0';
	else//说明ch[i]是字母
	{
		arr[i] = ch[i] - 'A' + 10;
	}
}

ll ans = 0;
for (int i = 0; i < ch.size(); i++)
{
	ans *= x;
	ans += arr[i];
}
cout << ans;
```

### 2.十进制转x进制

储存所有可能的字符，每次处理十进制数，得到x进制数的最后一位。最后反转输出即可。

```c++
string ch = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
//用于储存（36以内进制）可能的字符S

ll a; cin >> a;     //十进制数
int x; cin >> x;    //表示要转换成x进制数
string arr;

while (a)
{
	arr += ch[a % x];
	a /= x;
}
reverse(arr.begin(), arr.end());
cout << arr;
```

### 3.x进制转y进制

先将x进制转成十进制，再将十进制转成y进制即可。

## (四)高精度

这里的所有部分都只支持非负数计算！如果是负数，需要先适当操作变成非负数运算。

### 1.高精度加法

```c++
string a, b;
cin >> a >> b;
int len = max(a.size(), b.size());
while (a.size() < len) a = '0' + a;
while (b.size() < len) b = '0' + b;
//用字符串储存a、b，并补足前导零

string ans;
int jw = 0;
for (int i = len - 1; i >= 0; i--)
{
	int x = a[i] - '0', y = b[i] - '0';
	int now = x + y + jw;
	jw = now / 10;
	now %= 10;
	char c = now + '0';
	ans = c + ans;
}
if (jw)ans = '1' + ans;
cout << ans;
//高精度加法运算
```

### 2.高精度减法

```c++
string a, b;
cin >> a >> b;
bool change = false;   //记录a、b是否互换
if ((a.size() == b.size() && a < b) || (a.size() < b.size()))
{
	swap(a, b);change = true;   //保证a>=b
}
int len = max(a.size(), b.size());
while (a.size() < len) a = '0' + a;
while (b.size() < len) b = '0' + b;
//补足前导零

string ans;
for (int i = len - 1; i >= 0; i--)
{
	int x = a[i] - '0', y = b[i] - '0';
	int now = x - y;
	if (now < 0)now += 10, a[i - 1] -= 1;
	char c = now + '0';
	ans = c + ans;
}
int flag = ans.size() - 1;//无前导零数的首位下标
for (int i = 0; i < ans.size()-1; i++)
{
	if (ans[i] != '0')
	{
		flag = i;
		break;
	}
}
ans = ans.substr(flag);
if (change)ans = '-' + ans;
cout << ans;
//高精度减法运算
```

### 3.高精度乘法

```c++
int arr[10000];//至少为数字长度的2倍

string a, b;
cin >> a >> b;
int sa = a.size(), sb = b.size();
int len = 2 * max(a.size(), b.size()) + 2;
while (a.size() < len) a = '0' + a;
//将一个数补足数位，便于后续计算

for (int i = a.size() - 1; i >= a.size() - sa; i--)
{
	for (int j = sb - 1; j >= 0; j--)
	{
		arr[i - (sb - 1 - j)] += (a[i] - '0') * (b[j] - '0');
		arr[i - (sb - 1 - j)-1] += arr[i - (sb - 1 - j)] / 10;
		arr[i - (sb - 1 - j)] %= 10;
	}
}
//高精度乘法运算

int flag = a.size() - 1;
for (int i = 0; i < a.size(); i++)
{
	if (arr[i] != 0) { flag = i; break; }
}
for (int i = flag; i < a.size(); i++)cout << arr[i];
//去掉前置零，输出

```

## (五)离散化

将数组值域压缩，用数组元素的大小关系表示原数组的某个数。

需要在排好序的（并且多数时候是去重的）数组中使用。

比如，某个数是“数组中第x小”，由此可以知道数组中比它小/大的数的个数。

```c++
int arr[10000];
vector<int>L;

int n; cin >> n;
for (int i = 1; i <= n; i++)
{
	cin >> arr[i];
	L.push_back(arr[i]);
}
sort(L.begin(), L.end());
L.erase(unique(L.begin(), L.end()), L.end());  //排序去重

int x; cin >> x;    //输入要查找的x（应为arr中有的元素）
int num = lower_bound(L.begin(), L.end(), x) - L.begin();   //记录下标（从0开始）
cout << num;

//样例输入： 5       1 2 3 4 5       3
//样例输出：2

```

## (六)二分

可以在O(log n)时间内完成长度为n区间的检查。

边界处理的细节需要根据题目具体分析。

### 1.整数二分

```c++
int L = 0, R = 1e9;   //L应为 左边界的数-1
int a = 718666;//假设这个数就是我们想找的a
while (L + 1 != R)
{
	int mid = (L + R) / 2;
	if (mid >= a)
	{
		R = mid;
	}
	else
	{
		L = mid;
	}
}
cout << R << '\n';

```

### 2.浮点二分

```c++
double L = 0.0, R = 1e8, e = 1e-5;
//此处的e即为退出条件。L和R的差距小于e时退出，可以使误差不大于e。
double ans = 3.14159;
while (R - L >= e)
{
	double mid = (L + R) / 2;
	if (mid >= ans)
	{
		R = mid;
	}
	else
	{
		L = mid;
	}
}
cout << R << '\n';

```

### 3.二分答案

二分法中最常用的一种,需要选手从题目中发现某个单调的函数，然后对其二分。

常见模型是：二分框架（时间复杂度0(log m)）+具体查找函数（时间复杂度一般为O(n)）。

如：xxx情况下，最多可以拿到多少物品？（假设由题意很容易看出答案在0~10000之间）
`int L=-1,R=10000,mid=(L+R)/2;`
若发现可以拿mid个，则寻找更优解（令L=mid），反之则寻找可能的答案（令R=mid）。

## (七)双指针

### 1.对撞指针

一般会用两个“指针”（left和right）分别指向序列的第一个元素和最后一个元素。
然后l不断递增、r不断递减，直到两个指针相撞或错开（ l >= r ），或满足某些特定条件为止。

### 2.快慢指针

两个“指针”从数组一侧开始遍历，且移动的一快一慢。

我们称“快指针”为r，“慢指针”为l。这样两个“指针”构成一个区间[ l，r ]。

两指针按不同策略移动，直到快指针到达数组末端/区间不合法/达到某些特定条件。

（若从a[1]开始存数据，一般一开始l指向a[1]，r指向a[0]。这样初始的区间[1,0]为空区间。

​    后面的循环中一般要一直保持区间合法。）

## (八)位运算

### 1.常见位运算符号

(1) **& 按位与** —— 两个数的某一位都为1，运算后才为1，否则为0

​    特点：两个数字做与运算，结果不可能变得更大。

(2) **| 按位或** —— 两个数某一位都为0，结果才为0，否则为1

​    特点：两个数字做或运算，结果不可能变得更小。

(3) **^ 按位异或** —— 两个数的某一位相同则结果为0，相异则为1

​    特点：结果可能变大/变小/不变。

(4) **~ 按位取反** —— 将每一位0变1,1变0。通常作用于无符号整形。

​    特点：数字长度不同，结果也不同（因为可能有若干个前置0）。

(5) **<< 按位左移** —— 将二进制数整体左移指定位数，低位补0。通常作用于无符号整形。

​    特点：左移n位，相当于给这个数乘2的n次方。（假设不溢出）

(6) **>> 按位右移** —— 将二进制数整体右移指定位数，高位一般补0。通常作用于无符号整数/非负数。

​    特点：右移n位，相当于给这个数除以2的n次方，向下取整。

### 2.异或的性质

交换律： x^y = y^x
结合律：x ^ (y ^ z) = (x ^ y) ^ z
自反性： x^x = 0
零元素： x^0 = x
逆运算： x^y = z , 则z^y = x（相当于等式两边同时异或y）

### 3.位运算技巧

(1)**判断数字奇偶性**

x&1，若结果为1则为奇数，为0则为偶数。

(2)**获取二进制数的某一位**

(x>>i) & 1 ，结果必然为0或1，表示x从右向左数第i+1位的数字。

(3)**修改二进制的某一位为1**

x | (1<<i) ，将x的从右向左第i+1位变为1，其他位保持不变。

(4)**判断一个数是否为2的幂次方**

x & (x-1)，若结果为0，说明x是2的（非负整数）幂次方，否则不是。

(5)**获取一个数二进制位中最低位的1**

lowbit(x)，返回二进制中最低位的1及其后面的0组成的数。

```c++
int lowbit(int n)
{
    return n & (-n);
}

int n = 172;   //二进制：10101100
int x = lowbit(n);   //x=4 , 二进制：100

```

## (九)排序

以下各种排序中，均以数组中共n个元素、储存在arr[1]~arr[n]范围内的情况为例。

### 1.冒泡排序

时间复杂度O(n^2^)。

```c++
for (int i = n; i >= 1; i--)
{
	for (int j = 1; j <= i - 1; j++)
	{
		if (arr[j] > arr[j + 1])swap(arr[j], arr[j + 1]);
	}
}

```

### 2.选择排序

时间复杂度O(n^2^)。

```c++
for (int i = n; i >= 1; i--)
{
	int max = 1;   //扫描过程中最大数的下标
	for (int j = 1; j <= i; j++)
	{
		if (arr[j] > arr[max])max = j;
	}
	swap(arr[i], arr[max]);
}

```

### 3.插入排序

时间复杂度O(n^2^)。

```c++
for (int i = 2; i <= n; i++)
{
	int now = arr[i];
	int j;
	for (j = i; j > 1 && now < arr[j - 1]; j--)
	{
		arr[j] = arr[j - 1];
	}
	arr[j] = now;
}

```

### 4.快速排序

平均时间复杂度O(n log n)，最坏情况O(n^2^)。

```c++
int partition(int arr[],int l,int r)  //将某个数放在正确位置，并返回其下标
{
	int point = arr[r];
	int i = l, j = r;
	while (i < j)
	{
		while (i < j && arr[i] <= point)i++;
		while (i < j && arr[j] >= point)j--;
		if (i < j)
		{
			swap(arr[i], arr[j]);
		}
		else
		{
			swap(arr[i], arr[r]);
		}
	}
	return i;
}
void quicksort(int arr[], int l, int r)  //将[l,r]区间通过分治实现排序
{
	if (l < r)
	{
		int mid = partition(arr, l, r);
		quicksort(arr, l, mid - 1);
		quicksort(arr, mid + 1, r);
	}
}

int main()
{
    ……  //处理输入
    quicksort(arr, 1, n);
}    

```

### 5.归并排序

时间复杂度O(n log n)。

```c++
int b[10009];    //需要用到一个辅助数组，大小和arr相同

void mergesort(int arr[], int l, int r)
{
	if (l >= r)
	{
		return;
	}
	int mid = (l + r) / 2;
	mergesort(arr, l, mid);
	mergesort(arr, mid + 1, r);

	int pl = l, pr = mid + 1, pb = l;
	while (pl <= mid || pr <= r)
	{
		if (pl > mid)
		{
			b[pb] = arr[pr];
			pb++;pr++;
		}
		else if (pr > r)
		{
			b[pb] = arr[pl];
			pb++;pl++;
		}
		else
		{
			if (arr[pl] < arr[pr])
			{
				b[pb++] = arr[pl++];
			}
			else
			{
				b[pb++] = arr[pr++];
			}
		}
	}
	for (int i = l; i <= r; i++)arr[i] = b[i];
}

int main()
{
    ……  //处理输入
    mergesort(arr, 1, n);
}    

```

