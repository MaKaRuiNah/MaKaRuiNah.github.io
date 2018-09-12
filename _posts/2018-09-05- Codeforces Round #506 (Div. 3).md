---
title: Codeforces Round#506(Div.3)
date: 2018-09-05 20:47:12
updated: 2018-09-05 23:00:00
description: 1029 A,B,C,D,E,F题解报告
categories: acm
photo: 
tags: 
- codeforces
music-id: 480580003
password:
math:
---
[http://codeforces.com/contest/1029](http://codeforces.com/contest/1029)
### A. Many Equal Substrings

You are given a string t consisting of n lowercase Latin letters and an integer number kk.
Let's define a substring of some string s with indices from l to r as s[l…r].
Your task is to construct such string ss of minimum possible length that there are exactly k positions i such that s[i…i+n−1]=t. In other words, your task is to construct such string s of minimum possible length that there are exactly k substrings of s equal to t.
It is guaranteed that the answer is always unique.

**Input**

The first line of the input contains two integers n and k (1 ≤ n,k ≤50) — the length of the string t and the number of substrings.
The second line of the input contains the string t consisting of exactly n lowercase Latin letters.

**Output**
`
Print such string s of minimum possible length that there are exactly k substrings of s equal to t.
It is guaranteed that the answer is always unique

### 问题分析：

> 问题的大意是输出最短的字符串，要求满足其中有k个子字符串和所给字符串相等。本题主要是找到所给字符串最长的前缀字符串和后缀字符串，要求两者相等。然后输出(k-1)次前缀字符串后面的子字符串

### AC代码：

```c++

/*
author:Manson
date:9.4.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 1005;

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n,k;
	cin>>n>>k;
	string s,s1;
	cin>>s;
	for(int i = n-1;i >= 1;i--){
		//枚举长度 
		if(s.substr(0,i) == s.substr(n-i)){
			s1 = s.substr(i);
			cout<<s;
			k--;
			while(k--){
				cout<<s1;
			}
			return 0;
		}
	}
	while(k--){
		cout<<s;	
	}
	return 0;
}

```

### B. Creating the Contest

You are given a problemset consisting of n problems. The difficulty of the i-th problem is ai. It is guaranteed that all difficulties are distinct and are given in the increasing order.
You have to assemble the contest which consists of some problems of the given problemset. In other words, the contest you have to assemble should be a subset of problems (not necessary consecutive) of the given problemset. There is only one condition that should be satisfied: for each problem but the hardest one (the problem with the maximum difficulty) there should be a problem with the difficulty greater than the difficulty of this problem but not greater than twice the difficulty of this problem. In other words, let ai1,ai2,…,aip be the difficulties of the selected problems in increasing order. Then for each j from 1 to p−1 ;aij+1≤aij*2 should hold. It means that the contest consisting of only one problem is always valid.
Among all contests satisfying the condition above you have to assemble one with the maximum number of problems. Your task is to find this number of problems.

**Input**

The first line of the input contains one integer nn (1≤n≤2⋅105) — the number of problems in the problemset.
The second line of the input contains n integers a1,a2,…,an (1≤ai≤109) — difficulties of the problems. It is guaranteed that difficulties of the problems are distinct and are given in the increasing order.

**Output**

Print a single integer — maximum number of problems in the contest satisfying the condition in the problem statement.

### 问题分析：

> 问题大意是有n个数组成的序列，要求找出最长的子序列满足ai+1 <= 2ai。本题就是一道简单的模拟题，一遍循环即可。

### AC代码：

```
/*
author:Manson
date:9.4.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 2e5+5;
int a[N];
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n;
	cin>>n;
	int cnt = 0,ans = 0;
	for(int i = 0;i < n;i++){
		cin>>a[i];
		if(i == 0 || a[i] > 2*a[i-1]){
			cnt = 1;
		}else{
			cnt++;
		}
		ans = max(ans,cnt);
	}
	cout<<ans<<endl;
	return 0;
}


```

### C. Maximal Intersection

You are given n segments on a number line; each endpoint of every segment has integer coordinates. Some segments can degenerate to points. Segments can intersect with each other, be nested in each other or even coincide.
The intersection of a sequence of segments is such a maximal set of points (not necesserily having integer coordinates) that each point lies within every segment from the sequence. If the resulting set isn't empty, then it always forms some continuous segment. The length of the intersection is the length of the resulting segment or 0 in case the intersection is an empty set.
For example, the intersection of segments [1;5] and [3;10] is [3;5] (length 2), the intersection of segments [1;5] and [5;7] is [5;5](length 0) and the intersection of segments [1;5] and [6;6] is an empty set (length 0).
Your task is to remove exactly one segment from the given sequence in such a way that the intersection of the remaining (n−1)segments has the maximal possible length.

**Input**

The first line contains a single integer n (2≤n≤3*105) — the number of segments in the sequence.
Each of the next n lines contains two integers li and ri (0≤li≤ri≤109) — the description of the i-th segment.

**Output**

Print a single integer — the maximal possible length of the intersection of (n−1) remaining segments after you remove exactly one segment from the sequence.

### 问题分析：

> 问题大意是给出n个区间，求移除一个区间后，其他n-1个区间相交后的区间的最大长度。本题就是一道模拟题。首先从0-n-1遍历一遍前缀交，然后从n-1到0遍历一遍后缀交，然后比较前缀交和后缀交，表示取出其中一个i区间所得到的区间。得到最大的区间长度。

### AC代码：

```
/*
author:Manson
date:9.4.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 3e5+5;
const int INF = 1e9+7;
//a[i][0]表示第i个区间左端点,a[i][1]表示第i个区间右端点 
int a[N][3],b[N][3],c[N][3];
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n;
	cin>>n;
	for(int i = 0;i < n;i++){
		cin>>a[i][0]>>a[i][1];
	}
	b[0][0] = c[n][0] = -INF;
	b[0][1] = c[n][1] = INF;
	//前缀交,从1到n 
	for(int i = 0;i < n;i++){
		b[i+1][0] = max(b[i][0],a[i][0]);
		b[i+1][1] = min(b[i][1],a[i][1]);
	}
	//后缀交,从n-1到0
	for(int i = n-1;i >= 0;i--){
		c[i][0] = max(c[i+1][0],a[i][0]);
		c[i][1] = min(c[i+1][1],a[i][1]);
	}
	int ans  = 0;
	for(int i = 0;i < n;i++){
		int x1 = max(b[i][0],c[i+1][0]);
		int y1 = min(b[i][1],c[i+1][1]);
		ans = max(ans,y1-x1);
	}
	cout<<ans<<endl;
	return 0;
}

```


### D. Concatenated Multiples

You are given an array a, consisting of n positive integers.
Let's call a concatenation of numbers x and y the number that is obtained by writing down numbers x and y one right after another without changing the order. For example, a concatenation of numbers 12 and 3456 is a number 123456.
Count the number of ordered pairs of positions (i,j) (i≠j) in array a such that the concatenation of ai and aj is divisible by k.

**Input**

The first line contains two integers n and k (1≤n≤2*105, 2≤k≤109).
The second line contains n integers a1,a2,…,an (1≤ai≤109).

**Output**

Print a single integer — the number of ordered pairs of positions (i,j) (i≠j) in array aa such that the concatenation of ai and aj is divisible by k.

### 问题分析：

> 问题大意给出n个数，把其中两个数拼接，要求拼接后的数能被k整除，求有多少种拼接方式。本题的主要思路：收先用数学知识化简公式。x*10^(1gy+1)%k+y%k = x*10%k*10%k*... + y%k;
> 然后用b[N]数组保存各个数的位数，mp[N][N]存储余数个数。mp[i][x]即表示位数为i，余数为x的个数。
> 最后一层循环1-n个数，然后循环一下y的位数，采用余数互补的个数加入到最终结果中去。

### AC代码：

```
/*
author:Manson
date:9.5.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 2e5+5;
//mp[i][k]表示i位数的余数为k的个数 
map<int,int >mp[15];
//b存储第i个数的位数 
int a[N],b[N];
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n,k;
	cin>>n>>k;
	for(int i = 1;i <= n;i++){
		cin>>a[i];
		int x = a[i];
		while(x){
			x /= 10;
			b[i]++;
		}
		mp[b[i]][a[i]%k]++;
	}
	ll ans = 0;
	for(int i = 1;i <= n;i++){
		ll x = a[i];
		//除去本身的个数 
		mp[b[i]][x%k]--;
		//枚举后面加了j个0，余数互补 
		for(int j = 1;j <= 10;j++){
			x = x*10%k;
			if(mp[j].count(k-x)){
				ans += mp[j][k-x];
			}
			if(x == 0 && mp[j].count(0)){
				ans += mp[j][0];
			}
			
		}
		mp[b[i]][a[i]%k]++;
	}
	cout<<ans<<endl;
	return 0;
}


```


### E. Tree with Small Distances

You are given an undirected tree consisting of n vertices. An undirected tree is a connected undirected graph with n-1 edges.
Your task is to add the minimum number of edges in such a way that the length of the shortest path from the vertex 1 to any other vertex is at most 2. Note that you are not allowed to add loops and multiple edges.

**Input**

The first line contains one integer n (2≤n≤2*105) — the number of vertices in the tree.
The following n-1 lines contain edges: edge i is given as a pair of vertices ui,vi (1≤ui,vi≤n). It is guaranteed that the given edges form a tree. It is guaranteed that there are no loops and multiple edges in the given edges.

**Output**

Print a single integer — the minimum number of edges you have to add in order to make the shortest distance from the vertex 1 to any other vertex at most 2. Note that you are not allowed to add loops and multiple edges.

### 问题分析：

> 问题的大意是给出一棵树，要求1到任意一个节点的距离不能大于2；问至少要添加多少个连线才能满足这个条件。本题就是一个dfs问题，我用的是前式链向星保存的树的结构（看了一下数据量，也可以用vector保存）。在dfs中判断到1的距离是否>2;当大于2是就连线，并且连线过后子节点的父节点要更新为2。

### AC代码：

```
/*
author:Manson
date:9.4.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 2e5+5;
int n,num,res;
//pos距离子节点是否连接到 1，dis记录子节点到1的距离 
int head[N],pos[N],dis[N];
struct node{
	int next;
	int v;
}edge[N*2];

void addedge(int u,int v){
	edge[num].v = v;
	edge[num].next = head[u];
	head[u] = num++;
}

void dfs(int x,int fa){
	int ans = 0;
	for(int i = head[x];i!=-1;i = edge[i].next){
		int v = edge[i].v;
		if(v == fa){
			continue;
		}
		dis[v] = dis[x]+1;
		dfs(v,x);
		ans = max(ans,dis[v]);
		//如果dis>2的子节点连接到 1，那么父节点更新为2 
		if(pos[v] && dis[x] > 2){
			dis[x] = 2;
		}
	}
	if(ans > 2){
		pos[x] = 1;
		dis[x] = 1;
		res++;
	}
}

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	cin>>n;
	num = 0;
	res = 0;
	memset(head,-1,sizeof(head));
	for(int i = 1;i < n;i++){
		int u,v;
		cin>>u>>v;
		addedge(u,v);
		addedge(v,u);
	}
	dfs(1,-1);
	cout<<res<<endl;
	return 0;
}


```

### F. Multicolored Markers

There is an infinite board of square tiles. Initially all tiles are white.
Vova has a red marker and a blue marker. Red marker can color a tiles. Blue marker can color b tiles. If some tile isn't white then you can't use marker of any color on it. Each marker must be drained completely, so at the end there should be exactly a red tiles and exactly b blue tiles across the board.
Vova wants to color such a set of tiles that:
（1）they would form a rectangle, consisting of exactly a+b colored tiles;
（2）all tiles of at least one color would also form a rectangle.
Here are some examples of correct colorings:

![http://codeforces.com/predownloaded/04/d3/04d397883e96b6340f36e2733b99fcc6bbe58270.png](http://codeforces.com/predownloaded/04/d3/04d397883e96b6340f36e2733b99fcc6bbe58270.png)

Here are some examples of incorrect colorings:

![http://codeforces.com/predownloaded/dc/25/dc2548463a79d92f33f4fc83041926f2f377817e.png](http://codeforces.com/predownloaded/dc/25/dc2548463a79d92f33f4fc83041926f2f377817e.png)

Among all correct colorings Vova wants to choose the one with the minimal perimeter. What is the minimal perimeter Vova can obtain?
It is guaranteed that there exists at least one correct coloring.

**Input**

A single line contains two integers a and b (1≤a,b≤1014) — the number of tiles red marker should color and the number of tiles blue marker should color, respectively.

**Output**

Print a single integer — the minimal perimeter of a colored rectangle Vova can obtain by coloring exactly a tiles red and exactly b tiles blue.
It is guaranteed that there exists at least one correct coloring.

### 问题分析：

> 本题的大意是给出a,b,a+b = c;就是求面积为c矩形最大的周长。但是要满足a or b可以单个构成一个矩形。这题我是直接枚举的一边长。没想到就过了。。。

### AC代码：

```
/*
author:Manson
date:9.4.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 1005;
const ll INF = 4e14+5;
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	ll a,b;
	cin>>a>>b;
	ll c  = a+b;
	ll ans = INF;
	ll l = a;
	for(int i = 1;i <= c/i;i++){
		if(a%i == 0){
			l = min(l,a/i);
		}
		if(b%i == 0){
			l = min(l,b/i);
		}
		if(c % i == 0 && c/i>= l){
			ans = min(ans,((c/i+i)*2));
		}
	}
	cout<<ans<<endl;
	return 0;
}

```
