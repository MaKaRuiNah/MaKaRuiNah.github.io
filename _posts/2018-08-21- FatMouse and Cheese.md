---
title: FatMouse and Cheese
date: 2018-08-21 15:45:25
updated: 2018-08-21 15:45:25
description: 记忆化搜索
categories: acm
photo: 
tags: 
- hdu
- dfs
music-id:
password:
math:
---

### 问题描述：

FatMouse has stored some cheese in a city. The city can be considered as a square grid of dimension n: each grid location is labelled (p,q) where 0 <= p < n and 0 <= q < n. At each grid location Fatmouse has hid between 0 and 100 blocks of cheese in a hole. Now he's going to enjoy his favorite food.
FatMouse begins by standing at location (0,0). He eats up the cheese where he stands and then runs either horizontally or vertically to another location. The problem is that there is a super Cat named Top Killer sitting near his hole, so each time he can run at most k locations to get into the hole before being caught by Top Killer. What is worse -- after eating up the cheese at one location, FatMouse gets fatter. So in order to gain enough energy for his next run, he has to run to a location which have more blocks of cheese than those that were at the current hole.

Given n, k, and the number of blocks of cheese at each grid location, compute the maximum amount of cheese FatMouse can eat before being unable to move. 
 
**Input**

There are several test cases. Each test case consists of  a line containing two integers between 1 and 100: n and k. n lines, each with n numbers: the first line contains the number of blocks of cheese at locations (0,0) (0,1) ... (0,n-1); the next line contains the number of blocks of cheese at locations (1,0), (1,1), ... (1,n-1), and so on. 
The input ends with a pair of -1's. 
 
**Output**

For each test case output in a line the single integer giving the number of blocks of cheese collected. 

### 问题分析：

> 问题大意是有一只老鼠在起点（0，0）处，它只能横向或竖向走，每次跑的跑到一个点的时候他都要吃奶酪，但是每次奶酪都要比上一个要大，不然就会维持不了能量。要求输出它能吃到的最多的奶酪。

> 这题的解题思路是记忆化搜索。dfs(x,y)表示从点（x,y）遍历，用dp[x][y]保存点（x,y）处到结束为止时吃到的奶酪的最大值。在遍历的过程中用ans暂时表示(x,y)下一个能走到的位置（xx,yy）处到结束为止可以吃到的奶酪的最大值。
> 那么dp[x][y]  = ans+mp[x][y];

### AC代码：

```c++

/*
author:Manson
date:8.21.2018
theme:
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1005;
int n,k;
int mp[N][N];
int dir[4][2] = {-1,0,0,-1,1,0,0,1};
int dp[N][N];

int dfs(int x,int y){
	int ans = 0;
	if(!dp[x][y]){
		for(int i = 0;i < 4;i++){
			for(int j = 1;j <= k;j++){
				//走的步数遍历 
				int xx = x + dir[i][0]*j;
				int yy = y + dir[i][1]*j;
				if(xx >= 0 && xx < n &&yy >= 0 && yy < n){
					if(mp[xx][yy] > mp[x][y])
					//ans = max(ans,dp[xx][yy]) 
					ans = max(ans,dfs(xx,yy));
				}
			}
		}
		dp[x][y] = ans + mp[x][y];
	}
	return dp[x][y];
}

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	while(cin>>n>>k){
		if(n == -1 && k == -1){
			break;
		}
		for(int i = 0;i < n;i++){
			for(int j = 0;j < n;j++){
				cin>>mp[i][j];
			}
		}
		memset(dp,0,sizeof(dp));
		cout<<dfs(0,0)<<endl;
	}
	return 0;
}



```
