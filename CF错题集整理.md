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

  

个人新学习该代码模板我感觉更简洁更好看

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

  