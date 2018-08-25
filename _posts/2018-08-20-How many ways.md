---
title: How many ways
date: 2018-08-20 14:45:25
updated: 2018-08-20 14:45:25
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

这是一个简单的生存游戏，你控制一个机器人从一个棋盘的起始点(1,1)走到棋盘的终点(n,m)。游戏的规则描述如下：
1.机器人一开始在棋盘的起始点并有起始点所标有的能量。
2.机器人只能向右或者向下走，并且每走一步消耗一单位能量。
3.机器人不能在原地停留。
4.当机器人选择了一条可行路径后，当他走到这条路径的终点时，他将只有终点所标记的能量。
 
 ![http://acm.hdu.edu.cn/data/images/C113-1003-1.gif](http://acm.hdu.edu.cn/data/images/C113-1003-1.gif)


如上图，机器人一开始在(1,1)点，并拥有4单位能量，蓝色方块表示他所能到达的点，如果他在这次路径选择中选择的终点是(2,4)点，当他到达(2,4)点时将拥有1单位的能量，并开始下一次路径选择，直到到达(6,6)点。
我们的问题是机器人有多少种方式从起点走到终点。这可能是一个很大的数，输出的结果对10000取模。

**Input**

第一行输入一个整数T,表示数据的组数。
对于每一组数据第一行输入两个整数n,m(1 <= n,m <= 100)。表示棋盘的大小。接下来输入n行,每行m个整数e(0 <= e < 20)。
 
**Output**

对于每一组数据输出方式总数对10000取模的结果.

### 问题分析：

> 本题的解题思路就是记忆化搜索。我们可以初始化dp[n][m] = 1表示终点到达终点的种数只有1。然后从起点(1,1)开始搜。dp[x][y]表示从（x,y）到达终点（n,m）的种数。状态方程为**dp[x][y] = dp[x][y] + dfs(x+i,y+j);**其中i,j表示从x,y能够到达的点。

### AC代码：

```c++

/*
author:Manson
date:8.20.2018
theme:记忆化搜索 
*/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
#define MOD 10000
const int N = 105;

int n,m;
int mp[N][N];
int dp[N][N];

int dfs(int x,int y){
	if(dp[x][y] >= 0){
		return dp[x][y];
	}
	dp[x][y] = 0;
	//遍历（x,y）能够到达的点 
	for(int i = 0;i <= mp[x][y];i++){
		for(int j = 0; i+ j <= mp[x][y];j++){
			if(x + i <= n && y +j <= m){
				dp[x][y] = (dp[x][y]+dfs(x+i,y+j))%MOD;
			}
		}
	}
	return dp[x][y];
}

int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int t;
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i = 1;i <= n;i++){
			for(int j = 1;j <= m;j++){
				cin>>mp[i][j];
			}
		}
		memset(dp,-1,sizeof(dp));
		dp[n][m] = 1;
		cout<<dfs(1,1)<<endl;
	}
	return 0;
}


```
