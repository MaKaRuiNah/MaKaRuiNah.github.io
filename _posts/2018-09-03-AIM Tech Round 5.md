---
title: AIM Tech Round 5 (rated, Div. 1 + Div. 2)
date: 2018-09-03 23:00:00
updated: 2018-09-03 23:00:00
description: 1028 A,B,C,D,E题解报告
categories: acm
photo: 
tags: 
- codeforces
music-id: 449818741
password:
math:
---
[http://codeforces.com/contest/1028](http://codeforces.com/contest/1028)
### A. Find Square

Consider a table of size n*m, initially fully white. Rows are numbered 11 through nn from top to bottom, columns 1 through m from left to right. Some square inside the table with odd side length was painted black. Find the center of this square.

**Input**

The first line contains two integers n and m (1<=n,m<=115) — the number of rows and the number of columns in the table.
The i-th of the next nn lines contains a string of m characters si1si2…sim (sijsij is 'W' for white cells and 'B' for black cells), describing the i-th row of the table.

**Output**

Output two integers rr and cc (1≤r≤n,1≤c≤m) separated by a space — the row and column numbers of the center of the black square.

### 问题分析：

> 问题的大意是找到黑色区域的中心点。这题直接找到黑色区域左上角和右下角的坐标即可。

### AC代码：

```c++

/*
author:Manson
date:9.2.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 120;

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n,m;
	cin>>n>>m;
	char c;
	int flag = 0;
	int sx,sy,ex,ey;
	for(int i = 0;i < n;i++){
		for(int j = 0;j < m;j++){
			cin>>c;
			if(c == 'B' && !flag){
				flag = 1;
				sx= ex = i+1;
				sy = ey = j+1;
			}else if(c == 'B' && flag){
				ex = i+1;
				ey = j+1;
			}
		}
	}
	cout<<(sx+ex)/2<<" "<<(sy+ey)/2<<endl; 
	return 0;
}

```

### B. Unnatural Conditions

Let s(x) be sum of digits in decimal representation of positive integer x. Given two integers nn and mm, find some positive integers a and b such that
s(a)≥n
s(b)≥n
s(a+b)≤m.

**Input**

The only line of input contain two integers nn and mm (1≤n,m≤11291≤n,m≤1129).

**Output**

Print two lines, one for decimal representation of aa and one for decimal representation of bb. Both numbers must not contain leading zeros and must have length no more than 22302230.

### 问题分析：
> 本题是一个构造题，问题的大意是给出两个数n,m；输出两个数a,b，使得a的各位上数字之和>=n，b的各位数字之和>=n，但是a+b的各位数上数字之和<=m.
本题有个技巧就是a和b分别为一个大整数，但是a+b首位为1，其余为0即可。

### AC代码：

```
/*
author:Manson
date:9.2.2018
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
	int n,m;
	cin>>n>>m;
	for(int i = 0;i < 1000;i++){
		cout<<5;
	}
	cout<<" ";
	for(int i = 0;i < 999;i++){
		cout<<4;
	}
	cout<<5<<endl;
	return 0;
}

```

### C. Rectangles

You are given nn rectangles on a plane with coordinates of their bottom left and upper right points. Some (n−1) of the given n rectangles have some common point. A point belongs to a rectangle if this point is strictly inside the rectangle or belongs to its boundary.
Find any point with integer coordinates that belongs to at least (n−1) given rectangles.

**Input**

The first line contains a single integer n (2≤n≤132674) — the number of given rectangles.
Each the next n lines contains four integers x1, y1, x2 and y2 (−109≤x1<x2≤109, −109≤y1<y2≤109) — the coordinates of the bottom left and upper right corners of a rectangle.

**Output**

Print two integers x and y — the coordinates of any point that belongs to at least (n−1) given rectangles.

### 问题分析：

> 本题的大意是给出n个矩形的左下坐标和右上坐标。问至少有n-1个矩形有一个公共点的坐标。本题是一道模拟题。当两个矩形相交时，会产生一个相交的小矩形。所以取左下较大的横纵坐标，取右上较小的横纵坐标。因为题目要求是n-1个矩形有公共交点就行，因此我们可以求出前缀交和后缀交，然后再取左下较大的横纵坐标和右上较小的横纵坐标。满足x1<=x2&&y1<=y2即可。

### AC代码：

```
/*
author:Manson
date:7.7.2018
theme:模拟 
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 132680;
const int INF = 1e9+7;
//第二维表示坐标点x1,y1,x2,y2; 
int a[N][5],b[N][5],c[N][5];
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	
	int n;
	cin>>n;
	for(int i = 0;i < n;i++){
		for(int j = 0;j < 4;j++){
			cin>>a[i][j];
		}
	}
	b[0][0] = b[0][1] = c[n][0] = c[n][1] = -INF;
	b[0][2] = b[0][3] = c[n][2] = c[n][3] = INF;
	//前缀交 
	for(int i = 0;i < n;i++){
		//左下交点 
		b[i+1][0] = max(b[i][0],a[i][0]);
		b[i+1][1] = max(b[i][1],a[i][1]);
		//右上交点 
		b[i+1][2] = min(b[i][2],a[i][2]);
		b[i+1][3] = min(b[i][3],a[i][3]);
	}
	//后缀交 
	for(int i = n-1;i >=0;i--){
		c[i][0] = max(c[i+1][0],a[i][0]);
		c[i][1] = max(c[i+1][1],a[i][1]);
		c[i][2] = min(c[i+1][2],a[i][2]);
		c[i][3] = min(c[i+1][3],a[i][3]);
	}
	for(int i = 0;i < n;i++){
		int x1,y1,x2,y2;
		x1 = max(b[i][0],c[i+1][0]);
		y1 = max(b[i][1],c[i+1][1]);
		x2 = min(b[i][2],c[i+1][2]);
		y2 = min(b[i][3],c[i+1][3]);
		if(x1 <= x2 && y1<=y2){
			cout<<x1<<" "<<y1<<endl;
			return 0;
		}
	}
	return 0;
}

```


### D. Order book

Let's consider a simplified version of order book of some stock. The order book is a list of orders (offers) from people that want to buy or sell one unit of the stock, each order is described by direction (BUY or SELL) and price.
At every moment of time, every SELL offer has higher price than every BUY offer.In this problem no two ever existed orders will have the same price.
The lowest-price SELL order and the highest-price BUY order are called the best offers, marked with black frames on the picture below.

![http://codeforces.com/predownloaded/78/81/788181fcca39d7ec1a9bf9271c61d2965b57b82b.png](http://codeforces.com/predownloaded/78/81/788181fcca39d7ec1a9bf9271c61d2965b57b82b.png)

There are two possible actions in this orderbook:
   1.Somebody adds a new order of some direction with some price.
   2.Somebody accepts the best possible SELL or BUY offer (makes a deal). It's impossible to accept not the best SELL or BUY offer (to make a deal at worse price). After someone accepts the offer, it is removed from the orderbook forever.
It is allowed to add new BUY order only with prices less than the best SELL offer (if you want to buy stock for higher price, then instead of adding an order you should accept the best SELL offer). Similarly, one couldn't add a new SELL order with price less or equal to the best BUY offer. For example, you can't add a new offer "SELL 20" if there is already an offer "BUY 20" or "BUY  25" — in this case you just accept the best BUY offer.
You have a damaged order book log (in the beginning the are no orders in book). Every action has one of the two types:
1."ADD p" denotes adding a new order with price p and unknown direction. The order must not contradict with orders still not removed from the order book.
2."ACCEPT p" denotes accepting an existing best offer with price p and unknown direction.
The directions of all actions are lost. Information from the log isn't always enough to determine these directions. Count the number of ways to correctly restore all ADD action directions so that all the described conditions are satisfied at any moment. Since the answer could be large, output it modulo 109+7. If it is impossible to correctly restore directions, then output 0.

**Input**

The first line contains an integer n (1≤n≤363304) — the number of actions in the log.
Each of the next nn lines contains a string "ACCEPT" or "ADD" and an integer p (1≤p≤308983066), describing an action type and price.
All ADD actions have different prices. For ACCEPT action it is guaranteed that the order with the same price has already been added but has not been accepted yet.

**Output**

Output the number of ways to restore directions of ADD actions modulo 109+7.

### 问题分析：

> 本题大意是有n个操作。ADD p 表示在序列中加入p值（可能是buy也可能是sell）.ACCEPT p表示接受p价格（可能是buy or sell,p在当前序列中）。要求满足每一次Accept都是buy的最大值和sell中的最小值.输出不同ADD操作（buy or sell）的方案总数。本题也是一道模拟题。每次ADD操作后，如果p值比buy中的最大值小，那么放入buy中。p值比sell中的最小值大，那么放入sell中，否则就放入mid中。每次ACCEPT操作后，判断p值与buy中最大值A和sell中最小值B的关系。如果<A或者>B直接为0.如果=A或者=B,那么改变buy和sell,重新确定A和B,如果在（A，B）之间,那么结果ans*2,因为左右边都有可能。最后输出结果ans*(mid.size()+1).

### AC代码：

```
/*
author:Manson
date:9.3.2018
theme:模拟 
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 1005;
const int mod = 1e9+7;

set<int>sell,buy,mid;
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	
	int n;
	cin>>n;
	ll ans = 1;
	for(int i = 0;i < n;i++){
		string s;
		int p;
		cin>>s>>p;
		//add
		if(s.size() == 3){
			//buy的最大值 > p
			if(!buy.empty() && *buy.rbegin() >= p){
				buy.insert(p);
			}
			//sell的最小值 > p 
			else if(!sell.empty() && *sell.begin() <= p){
				sell.insert(p);
			}
			//中间区域 
			else{
				mid.insert(p);
			}
		}
		else{
			if(!buy.empty() && *buy.rbegin() > p){
				cout<<0<<endl;
				return 0;
			}
			if(!sell.empty() && *sell.begin() < p){
				cout<<0<<endl;
				return 0;
			}
			if(sell.count(p)){
				sell.erase(p);
				for(int x:mid){
					buy.insert(x);
				}
				mid.clear();
			}
			else if(buy.count(p)){
				buy.erase(p);
				for(int x:mid){
					sell.insert(x);
				}
				mid.clear();
			}
			else{
				mid.erase(p);
				ans = ans*2%mod;
				for(int x:mid){
					if(x < p){
						buy.insert(x);
					}
					else{
						sell.insert(x);
					}
				}
				mid.clear();
			}
		}
	}
	cout<<ans*(mid.size()+1)%mod<<endl;
	return 0;
}

```


### E. Restore Array

While discussing a proper problem A for a Codeforces Round, Kostya created a cyclic array of positive integers a1,a2,…,an. Since the talk was long and not promising, Kostya created a new cyclic array b1,b2,…,bn so that bi=(ai mod ai+1), where we take an+1=a1. Here mod is the modulo operation. When the talk became interesting, Kostya completely forgot how array aa had looked like. Suddenly, he thought that restoring arrayaa from array b would be an interesting problem (unfortunately, not A).

**Input**

The first line contains a single integer n (2≤n≤140582) — the length of the array a.
The second line contains n integers b1,b2,…,bn (0≤bi≤187126).

**Output**

If it is possible to restore some array a of length n so that bi=ai mod a(i mod n)+1 holds for all i=1,2,…,n, print «YES» in the first line and the integers a1,a2,…,an in the second line. All ai should satisfy1≤ai≤1018. We can show that if an answer exists, then an answer with such constraint exists as well.
It it impossible to restore any valid a, print «NO» in one line.
You can print each letter in any case (upper or lower).

### 问题分析：

> 问题的大意是给出一个数组a,其中a[i] = b[i] % b[(i+1)%n];要求构造一个满足条件的b。首先当a中所有元素相等时（0除外），明显不可能，直接输出0。当全部为0时单独判断一下。然后找出所有元素中的最大值a[k]其中a[k] > a[(k-1)%n],不然不满足有些样例。在给a[k]*一个超大的系数后，向前加上余数a[(k-1)%n]就 等于 b[(k-1)%n]。

### AC代码：

```
/*
author:Manson
date:9.3.2018
theme:构造 
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int N = 150000;
const int INF = 1e9+7;
ll a[N],b[N];
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n;
	cin>>n;
	int flag = 1,k = -1;
	ll id = INF;
	for(int i = 0;i < n;i++){
		cin>>a[i];
		if(a[i] > 0){
			flag = 0;
		}
	}
	//全为 0 
	if(flag){
		cout<<"YES"<<endl;
		for(int i = 0;i < n;i++){
			cout<<1<<" ";
		}
		cout<<endl;
		return 0;
	}
	for(int i = 0;i < n;i++){
		if(a[i] > a[(i-1+n)%n]){
			k = i;
		}
		id = min(id,a[i]);
	}
	
	if(k == -1 || a[k] ==  id){
		cout<<"NO"<<endl;
		return 0;
	}
	
	b[k] = a[k];
	k = (k-1+n)%n; 
	b[k] = 10000000*b[(k+1)%n]+a[k];
	
	for(int i = 1;i < n-1;i++){
		int j = (k-i+n)%n;
		b[j] = a[j]+b[(j+1)%n];
	}
	/*处理余数是0时产生了超界问题 
	for(int i = 0;i < n-1;i++){
		if(a[(k-i+n-1)%n] == 0){
			b[(k-i+n-1)%n] = b[(k-i+n)%n]*2;
		}
		else{
			b[(k-i+n-1)%n] = b[(k-i+n)%n]+a[(k-i+n-1)%n];
		}
		//cout<<b[(k-i+n-1)%n]<<endl;
	}
	*/
	cout<<"YES"<<endl;
	for(int i = 0;i < n;i++){
		
		cout<<b[i]<<" ";
	}
	cout<<endl;
	return 0;
}

```

