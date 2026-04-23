# CF错题集整理

## CF1090 错题解析

*  D. The 67th OEIS Problem

  首先观察题面要找到**gcd(a,b)**的不同值，我们不妨用素数来确定所有**gcd(a,b)**<br>

  则只需要让素数两两相乘即可<br>

  ```cpp
  #include<bits/stdc++.h>
  #define int long long
  using namespace std;
  const int N = 10000005;
  int v[N], prime[N];
  int m, n, t;
  void primes(int n) {
  	memset(v, 0, sizeof(v));
  	m = 0;
  	for (int i = 2; i <= n; i++) {
  		if (!v[i]) {
  			v[i] = i;
  			prime[++m] = i;
  		}
  		for (int j = 1; j <= m; j++) {
  			if (prime[j] > v[i] || prime[j] > n / i) {
  				break;
  			}
  			v[i * prime[j]] = prime[j];
  		}
  	}
  }
  
  void solve() {
  	for (int i = 1; i <= n; i++) {
  		cout << prime[i - 1]*prime[i] << " ";
  	}
  	cout << '\n';
  }
  
  signed main() {
  	ios::sync_with_stdio(0);
  	cin.tie(0);
  	cout.tie(0);
  	cin >> t;
  	primes(10000000);
  	prime[0] = 1;
  	while (t--) {
  		cin >> n;
  		solve();
  	}
  	return 0;
  }
  ```

  

个人新学习该代码模板我感觉更简洁更好看<br>

* E. The 67th XOR Problem<br>

  根据题意我们需要了解以下异或运算的性质

  * 异或交换律     **a^b=b^a**
  * 异或结合律     **a^b^c=a^(b^c)**
  * 自反性             **a^a=0**
  * 零值                 **a^0=a**<br>

  结合以上规律我们不妨对题意解读  每次选一个值进行全数组异或，但我们分析一个规律<br>

  ***{a,b,c}*** 我们选择拿出c异或即<br>

  **{a^c,b^c}**进一步可以推导出**ans = (a^c)^(b^c)  = a^b^c^c = a^b^0 = a^b**<br>

  
  $$
  \large综上所述是不是无论拿什么进行异或最后都是两个数进行异或，所以我们只需要选择两个数异或最大值即可\\
  
  \large我们采用暴力枚举即时间复杂度O(n^2)
  $$
  

  代码如下：

  ```cpp
  #include<bits/stdc++.h>
  using namespace std;
  int main() {
  	int t;
  	cin >> t;
  	while (t--) {
  		int n;
  		cin >> n;
  		vector<int> nums;
  		for (int i = 0; i < n; i++) {
  			int num;
  			cin >> num;
  			nums.push_back(num);
  		}
  		int ans = -1e9;
  		for (int i = 0; i < n; i++) {
  			for (int j = 0; j < n; j++) {
  				ans = max(ans, nums[i] ^ nums[j]);
  			}
  		}
  		cout << ans << endl;
  	}
  	return 0;
  }
  ```
## CF784 错题解析

* D. Colorful Stamp

  由样例分情况讨论发现只要**W前面B和R都存在**，不管BR各自有多少个都可以实现

  又联想到 **异或** 正好符合BR要求，此题得解，不用纠结于数量问题，第一次掉进了奇数偶数的坑，第二次纠结了1的坑，要多想

  以下为代码实现

  ```cpp
  #include<bits/stdc++.h>
  #define int long long
  using namespace std;
  int t;
  int n;
  string s;
  
  void clear() {
  	s = "";
  }
  
  void solve() {
  	int len = s.size();
  	bool flag = false;
  	int flagr = 0;
  	int flagb = 0;
  	for (int i = 0; i < len; i++) {
  		if (s[i] == 'B') {
  			flagb = 1;
  		} else if (s[i] == 'R') {
  			flagr = 1;
  		} else {
  			if ((flagb ^ flagr) == 1) {
  				flag = true;
  			}
  			flagb = 0;
  			flagr = 0;
  		}
  	}
  	if ((flagb ^ flagr) == 1) {
  		flag = true;
  	}
  	if (!flag) cout << "yes\n";
  	else cout << "no\n";
  }
  
  signed main() {
  	ios::sync_with_stdio(0);
  	cin.tie(0);
  	cout.tie(0);
  	cin >> t;
  	while (t--) {
  
  		clear();
  
  		cin >> n;
  		cin.ignore();
  		for (int i = 0; i < n; i++) {
  			char ch;
  			cin >> ch;
  			s += ch;
  		}
  
  		solve();
  	}
  	return 0;
  }
  ```

  

* F. Eating Candies

  经典的一道双指针问题，多画图考虑双指针范围，可以直接通过双指针确认吃了多少个糖果的！

  代码如下

  ```cpp
  #include<bits/stdc++.h>
  #define int long long
  using namespace std;
  int t;
  int n;
  vector<int> nums;
  
  void clear() {
  	nums.clear();
  }
  
  void solve() {
  	int n = (int)nums.size();
  	int l = 0, r = n - 1;
  	long long suml = 0, sumr = 0;
  	int ans = 0;
  
  	while (l <= r) {
  		if (suml == sumr) {
  			ans = max(ans, l + (n - 1 - r));
  		}
  		if (suml <= sumr) {
  			suml += nums[l++];
  		} else {
  			sumr += nums[r--];
  		}
  	}
  	if (suml == sumr) {
  		ans = max(ans, l + (n - 1 - r));
  	}
  	cout << ans << '\n';
  }
  
  signed main() {
  	ios::sync_with_stdio(0);
  	cin.tie(0);
  	cout.tie(0);
  	cin >> t;
  	while (t--) {
  		clear();
  
  		cin >> n;
  		for (int i = 0; i < n; i++) {
  			int num;
  			cin >> num;
  			nums.push_back(num);
  		}
  
  		solve();
  	}
  	return 0;
  }
  ```

  
  
  
  
  
  

## ***CF790 错题解析***

#### ***D. X-Sum***

本题偏简单，本题要求求出所在格子斜线的最长距离，但数据给的范围支持暴力，以下是暴力解法，但本人在vp时候并没有a出来，因为有一个字母看错了，希望下次仔细检查<br>

以下是暴力代码:

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

int t, n, m;
int tu[205][205];

void clear() {
	memset(tu, 0, sizeof(tu));
}

void solve() {
	int ans = 0;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			int sum = tu[i][j];
			for (int k = 1; i - k >= 0 && j - k >= 0; k++) {
				sum = sum + tu[i - k][j - k];
			}
			for (int k = 1; (i - k) >= 0 && (j + k) < m; k++) {
				sum += tu[i - k][j + k];
			}
			for (int k = 1; (i + k) < n && (j - k >= 0); k++) {
				sum += tu[i + k][j - k];
			}
			for (int k = 1; (i + k < n) && (j + k) < m; k++) {
				sum += tu[i + k][j + k];
			}
			ans = max(ans, sum);
		}
	}
	cout << ans << '\n';
}


signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	cin.ignore();
	while (t--) {
		clear();

		cin >> n >> m;
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				cin >> tu[i][j];
			}
		}

		solve();
	}
	return 0;
}
```



* 但当数据范围大时必定超时，所以通过题解发现分为两种对角线，两种对角线都有规律<br>

  参考**题解**如下：

  [题解][CF1676D X-sum - 洛谷专栏](https://www.luogu.com.cn/article/lznb4tio)

  自行实现代码如下

  #### ***Code***

  ```cpp
  #include<bits/stdc++.h>
  #define int long long
  using namespace std;
  
  int t, n, m;
  int tu [205][205];
  int l[550];
  int r[550];
  
  void clear() {
  	memset(tu,0,sizeof(tu));
  	memset(l,0,sizeof(l));
  	memset(r,0,sizeof(r));
  }
  
  void solve() {
  	int ans = 0;
  	for(int i=0;i<n;i++){
  		for(int j=0;j<m;j++){
  			if(ans<l[i+j]+r[i-j+m]-tu[i][j]){
  				ans = l[i+j]+r[i-j+m]-tu[i][j];
  			}
  		}
  	}
  	cout<<ans<<'\n';
  }
  
  
  signed main() {
  	ios::sync_with_stdio(0);
  	cin.tie(0);
  	cout.tie(0);
  	cin >> t;
  	cin.ignore();
  	while (t--) {
  		clear();
  
  		cin>>n>>m;
  		for(int i=0;i<n;i++){
  			for(int j=0;j<m;j++){
  				cin>>tu[i][j];
  				l[i+j]+=tu[i][j];
  				r[i-j+m]+=tu[i][j];
  			}
  		}
  		solve();
  	}
  	return 0;
  }
  ```

  ###   ***E. Eating Queries***

  本题题意就是**给定一个k看看能不能在数组里选尽可能少的数加起来>=k**<br>

  本人认为自己错在了构建代码的能力上，思路并没有问题，但是想的是纯暴力运用贪心从大到小排序一点点来，会超时<br>

  ***Code***

  ```cpp
  #include<bits/stdc++.h>
  #define int long long
  using namespace std;
  
  int t, n, q;
  vector<int> nums;
  bool cmp(const int &a, const int &b) {
  	return a > b;
  }
  
  void clear() {
  	nums.clear();
  }
  
  void solve() {
  	sort(nums.begin(), nums.end(), cmp);
  	while (q--) {
  		int ans = 0;
  		int num;
  		cin >> num;
  		int len = nums.size();
  		int sum = 0;
  		bool flag = false;
  		for (int i = 0; i < len; i++) {
  			sum += nums[i];
  			ans++;
  			if (sum >= num) {
  				flag = true;
  				break;
  			}
  		}
  		if (!flag) cout << -1 << '\n';
  		else cout << ans << '\n';
  	}
  }
  
  
  signed main() {
  	ios::sync_with_stdio(0);
  	cin.tie(0);
  	cout.tie(0);
  	cin >> t;
  	cin.ignore();
  	while (t--) {
  		clear();
  
  		cin >> n >> q;
  		for (int i = 0; i < n; i++) {
  			int num;
  			cin >> num;
  			nums.push_back(num);
  		}
  
  		solve();
  	}
  	return 0;
  }
  ```

  

  

题解优化思路，因为序列是单调递增的，且答案具有单调性，如果和过大一定成功，如果和不够一定失败，可以采用二分优化代码

***Code***

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

const int N = 1e5+5e4+5;

int t, n, q;
vector<int> nums;
int pre[N];
bool cmp(const int &a, const int &b) {
	return a > b;
}

void clear() {
	nums.clear();
	memset(pre, 0, sizeof(pre));
}


void solve() {
	sort(nums.begin(), nums.end(), cmp);
	pre[1] = nums[0];
	for (int i = 2; i <= n; i++) {
		pre[i] = pre[i - 1] + nums[i - 1];
	}
	while (q--) {
		int num;
		cin >> num;
		auto ans = lower_bound(pre + 1, pre + n + 1, num) - pre;
		if (ans == n + 1) cout << -1 << '\n';
		else cout << ans << '\n';

	}
}


signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	cin.ignore();
	while (t--) {
		clear();

		cin >> n >> q;
		for (int i = 0; i < n; i++) {
			int num;
			cin >> num;
			nums.push_back(num);
		}

		solve();
	}
	return 0;
}
```



### ***F. Longest Strike***

这道题题意大概可以理解为找最长出现次数均大于K次的连续子序列<br>

**<font color="red"> 而且我发现如果没确定的把握最好是使用map因为他很稳定</font>**

***Code***

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

const int N = 1e5+5e4+5;

int t, n, k;
vector<int> nums;
map<int, int> mp;
int max_emerge = 0;

void clear() {
	nums.clear();
	mp.clear();
	max_emerge = 0;
}


void solve() {
	if (max_emerge < k) {
		cout << -1 << '\n';
		return;
	}
	vector<int> satisfy;
	for (auto &it : mp) {
		if (it.second >= k) {
			satisfy.push_back(it.first);
		}
	}
	sort(satisfy.begin(), satisfy.end());
	int len = satisfy.size() ;
	int l = satisfy[0];
	int r  = satisfy[0];
	int ansl = l, ansr = r;
	int maxn = 0;
	if (len >= 2) {
		for (int i = 1; i < len; i++) {
			if (satisfy[i] == satisfy[i - 1] + 1) {
				r = satisfy[i];
			} else {
				if (maxn < r - l) {
					maxn = r - l;
					ansl = l;
					ansr = r;
				}
				l = satisfy[i];
				r = satisfy[i];
			}
		}
	}
	if (r - l > maxn) {
		ansl = l;
		ansr = r;
	}
	cout << ansl << " " << ansr << '\n';
}


signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	cin.ignore();
	while (t--) {
		clear();

		cin >> n >> k;
		for (int i = 0; i < n; i++) {
			int num;
			cin >> num;
			nums.push_back(num);
			mp[num]++;
			max_emerge = max(mp[num], max_emerge);
		}

		solve();
	}
	return 0;
}
```

***Cursor 给了优化答案***

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#define int long long
using namespace std;

int t, n, k;
vector<int> nums;

void solve() {
	vector<int> satisfy;
	sort(nums.begin(), nums.end());

	for (int i = 0; i < n; ) {
		int j = i;
		while (j < n && nums[j] == nums[i]) j++;
		if (j - i >= k) satisfy.push_back(nums[i]);
		i = j;
	}
	if (satisfy.empty()) {
		cout << -1 << '\n';
		return;
	}

	int l = satisfy[0], r = satisfy[0];
	int ansl = l, ansr = r;
	int maxn = 0;
	int len = (int)satisfy.size();

	for (int i = 1; i < len; i++) {
		if (satisfy[i] == satisfy[i - 1] + 1) {
			r = satisfy[i];
		} else {
			if (r - l > maxn) {
				maxn = r - l;
				ansl = l;
				ansr = r;
			}
			l = satisfy[i];
			r = satisfy[i];
		}
	}

	// 别漏掉最后一段连续区间
	if (r - l > maxn) {
		ansl = l;
		ansr = r;
	}

	cout << ansl << " " << ansr << '\n';
}


signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	while (t--) {
		nums.clear();
		cin >> n >> k;
		for (int i = 0; i < n; i++) {
			int num;
			cin >> num;
			nums.push_back(num);
		}
		solve();
	}
	return 0;
}
```

### ***H2. Maximum Crossings (Hard Version)***

由题意得本题就是求逆序对，本人刚想的是归并排序求逆序对，在bilibili学到了新知识叫树状数组

***Code***

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

int a[200005];
int test, n, ans;
int f[200005];

void clear() {
	memset(a, 0, sizeof(a));
	ans = 0;
	memset(f, 0, sizeof(f));
}

void add(int x, int y) {
	for (; x <= n; x += x & (-x)) f[x] += y;
}

int ask(int x) {
	int res = 0;
	for (; x; x -= x & (-x)) {
		res += f[x];
	}
	return res;
}
void solve() {
	for (int i = n; i; i--) {
		ans += ask(a[i]);
		add(a[i], 1);
	}
	cout << ans << '\n';

}
signed main() {
	cin >> test;
	while (test--) {
		clear();

		cin >> n;
		for (int i = 1; i <= n; i++) {
			cin >> a[i];
		}

		solve();

	}
}
```

##### **<font color="red"> 建议大家多考虑前缀和因为我想起来这玩意用处蛮大的</font>**



## [Educational Codeforces Round 189 (Rated for Div. 2)](https://codeforces.com/contest/2225) 错题解析



### ***C. Red-Black Pairs***

典型的dp板子题，但是本人好久不做忘了很多，需要补充一些dp板子知识，当前面情况已经模拟好后，不难发现要么竖着统计要么两行横着统计

***Code***

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

const int N = 2e5+5;

int t;
string s1, s2;
int n;
int dp[N];

void clear() {
	s1 = "";
	s2 = "";
}


void solve() {
	memset(dp, 0x3f3f3f3f, sizeof(dp));
	dp[n] = 0;
	for (int i = n - 1; i >= 0; i--) {
		int v = 0;
		if (s1[i] != s2[i]) v = 1;
		dp[i] = min(dp[i], v + dp[i + 1]);
		if (i < n - 1) {
			int h = 0;
			if (s1[i] != s1[i + 1]) h += 1;
			if (s2[i] != s2[i + 1]) h += 1;
			dp[i] = min(dp[i], h + dp[i + 2]);
		}
	}
	cout << dp[0] << '\n';
}


signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	cin.ignore();
	while (t--) {
		clear();
		cin >> n;
		for (int i = 0; i < n; i++) {
			char ch;
			cin >> ch;
			s1 += ch;
		}
		cin.ignore();
		for (int i = 0; i < n; i++) {
			char ch;
			cin >> ch;
			s2 += ch;
		}

		solve();
	}
	return 0;
}
```

### ***D. Exceptional Segments***

数学知识题，本题涉及到异或的多种性质，详情见链接

[异或性质][题解：CF2225D Exceptional Segments - 洛谷专栏](https://www.luogu.com.cn/article/i87rgz8p)<br>

下为本人复述异或性质的推理

**连续整数异或和的周期性**见本人另一个知识点.md

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

const int N = 2e5+5;
const int MOD =  998244353;

int t;

void clear() {

}

int count_rem(int limit, int rem) {
	if (limit < rem) return 0;
	return (limit - rem) / 4 + 1;
}

void solve() {
	int n, x;
	cin >> n >> x;

	int a0 = count_rem(x - 1, 3) + 1;
	int r0 = count_rem(n, 3) - count_rem(x - 1, 3);

	int a1 = count_rem(x - 1, 1);
	int r1 = count_rem(n, 1) - count_rem(x - 1, 1);

	int ans = 0;
	ans = (ans + (a0 % MOD) * (r0 % MOD)) % MOD;
	ans = (ans + (a1 % MOD) * (r1 % MOD)) % MOD;

	cout << ans << "\n";
}

signed main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	cin >> t;
	while (t--) {
		clear();
		solve();
	}
	return 0;
}
```

