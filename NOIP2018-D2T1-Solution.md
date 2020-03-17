---
title: NOIP2018 Day2T1 题解
date: 2019-11-12 20:33:42
categories: 
  - [OI]
  - [算法]
tags: [OI, 算法, NOIP, 搜索, DFS, 剪枝, 生成树, 图论, 树论]
mathjax: true
---

![](https://infi.wang/ContentStorage/pic/blog/NOIP2018-D2T1-Solution/brutalForceTLE.png)

去年因为这题考虑多了60变20, 1=变2=, 省队变差8分, 草(砸电脑.gif

------------

**本题解几乎全为代码, 请静下心阅读. 我相信我的的代码可读性还是很高的.**

<!-- more -->

# 题目

![NOIP2018.D2T1](https://cdn.infi.wang/pic/blog/NOIP2018-D2T1-Solution/NOIP2018.D2T1.png)

![Datarange](https://cdn.infi.wang/pic/blog/NOIP2018-D2T1-Solution/NOIP2018.D2T1.Datarange.png)

# 题解

回到正题. 首先观察数据. n = 5000, 所以暴力就好了. 当时直接上邻接矩阵都过了树的subtask. 

现在讨论情况. 

## 第一种, n = m - 1 时为一棵树. 

只要保证出边到达点字典序从小到大后进行一次不回溯的DFS即可, 复杂度$\Theta \left ( N \right )$. 以下为本人去年此subtask的去锅代码. 
```cpp
#include <cstdlib>
#include <cstdio>
#include <algorithm>
#include <iostream>

const int maxN = 5000;
int n,m,current;
bool isFinal;
int minimalSet[1 + maxN];
bool isCityVisited[1 + maxN];
bool roadIndex[1 + maxN][1 + maxN];

void travel(int nowCity)
{
	if(isFinal)
	{
		return;
	}
	current++;
	minimalSet[current] = nowCity;
	isCityVisited[nowCity] = true;
	if(current == n)
	{
		minimalSet[0] = nowCity;
		for(int i = 1;i <= n;i++)
		{
			printf("%i ",minimalSet[i]);
		}
		isFinal = true;
		return;
	}
	for(int i = 1;i <= n;i++)
	{
		if(isCityVisited[i] == false
		&&(roadIndex[nowCity][i] || roadIndex[i][nowCity]))
		{
			travel(i);
		}
	}
}

int main()
{
	std::cin >> n >> m;
	for(int i = 1;i <= m;i++)
	{
		int u,v;
		scanf("%i %i",&u,&v);
		roadIndex[u][v] = true;
		roadIndex[v][u] = true;
	}
	
	travel(1);
	
	return 0;
}
```

    
## 第二种, n = m 时为一张有且仅有一条环的图

去年我看到这就放弃了, 毕竟当时连存图都是凭印象瞎搞的. 当然, 现在看来无非两种方式: 找边, 找环. 这里从简(其实还是不会), 只讨论暴力断边的方案. 
    这样一张图(基环树/图)有这样的性质: 断环上的任意一条边就变成一棵树. 那么就有以下代码. 
```cpp
#include <cstdlib>
#include <cmath>
#include <algorithm>
#include <cstring>
#include <vector>
#include <queue>
#include <map>
#include <cstdio>
#include <iostream>

struct Edge
{
	int u, v;
	bool w;

	Edge(int U = 0, int V = 0, bool W = true)
	{
		(*this).u = U, (*this).v = V, (*this).w = W;
	}

	void reverse()
	{
		std::swap((*this).u, (*this).v);
		return;
	}

	Edge getReversed()
	{
		return Edge((*this).v, (*this).u, (*this).w);
	}

	Edge operator = (const Edge& inEdge)
	{
		(*this).u = inEdge.u, (*this).v = inEdge.v, (*this).w = inEdge.w;
		return *this;
	} 
}
emptyEdge;

inline bool cmp(Edge E, Edge e)
{
	return E.v < e.v;
}

inline void copy(int n, int* ori, int* des)
{
	for(int i = 1; i <= n; i++) des[i] = ori[i];
}

inline void set(int n, bool stat, bool* des)
{
	for(int i = 1; i <= n; i++) des[i] = stat;
}

inline bool isMinDic(int n, int* dicOri, int* dicCmp)
{
	for(int i = 1; i <= n; i++)
	{
		if(dicCmp[i] != dicOri[i])
		{
			if(dicCmp[i] > dicOri[i]) return false;
			if(dicCmp[i] < dicOri[i]) return true;
		}
	}
	return false;
} 

inline void setEdge(int u, int v, bool w, std::vector<Edge>* graph)
{
	for(std::vector<Edge>::iterator it = graph[u].begin(); it != graph[u].end(); it++)
	{
		if((*it).v == v)
		{
			(*it).w = w;
			return;
		}
	}
}

void DFS(int now, int& depth, bool* isVisited, std::vector<Edge>* graph, int* dic)
{
	isVisited[now] = true;
	dic[depth++] = now;
	for(std::vector<Edge>::iterator it = graph[now].begin(); it != graph[now].end(); it++)
	{
		if(!isVisited[(*it).v] && (*it).w)
			DFS((*it).v, depth, isVisited, graph, dic);
	}
}

inline void subtask(int n, std::vector<Edge>* graph, bool* isVisited, int* dic)
{
	for(int k = 1;k <= n;k++)
	{
		for(std::vector<Edge>::iterator it = graph[k].begin(); it != graph[k].end(); it++)
		{
			(*it).w = false/*, setEdge((*it).v, (*it).u, false, graph)*/;

			int tDic[1 + n]; tDic[0] = 0; int depth = 1;
			for(int i = 1; i <= n; i++){tDic[i] = 5000 + 1; isVisited[i] = false;}

			DFS(1, depth, isVisited, graph, tDic);
			if(isMinDic(n, dic, tDic)) copy(n, tDic, dic);

			(*it).w = true/*, setEdge((*it).v, (*it).u, true, graph)*/;
		}
	}
}

int main()
{
	int n, m;
	std::cin >> n >> m;
	std::vector<Edge> graph[1 + n];
	for(int i = 1; i <= m; i++)
	{
		int u, v;
		scanf("%i %i", &u, &v);
		graph[u].push_back(Edge(u, v));
		graph[v].push_back(Edge(v, u));
	}
	for(int i = 1; i <= n; i++)
	{
		std::sort(graph[i].begin(), graph[i].end(), cmp);
	}

	int dic[1 + n]; for(int i = 1;i <= n;i++){dic[i] = 5000 + 1;} dic[0] = 0;
	bool isVisited[1 + n]; memset(isVisited, false, sizeof(isVisited));
	int depth = 1;
	n != m ? DFS(1, depth, isVisited, graph, dic)
		   : subtask(n, graph, isVisited, dic);

	for(int i = 1; i <= n; i++)
	{
		printf("%i ", dic[i]);
	}
	
	return 0;
}
```
![TLE](https://cdn.infi.wang/pic/blog/NOIP2018-D2T1-Solution/brutalForceTLE.png)

我交了, 吸氧了, 多50ms T了, 那咋办嘛 QAQ

很明显, 这个方法的复杂度为$\Theta \left ( N^{2} \right )$, 所以炸T也不奇怪.

## 这时就需要剪枝了

这里只讲最优化剪枝. 当当前搜索所得序列劣于已得最佳序列时就可选择剪枝. 于是最终得到以下代码, 复杂度$\Omega \left ( n \right )$, $O \left ( N^{2} \right )$(偏向前者). 

```cpp
#include <cstdlib>
#include <cmath>
#include <algorithm>
#include <cstring>
#include <vector>
#include <queue>
#include <map>
#include <cstdio>
#include <iostream>

struct Edge
{
	int u, v;
	bool w;

	Edge(int U = 0, int V = 0, bool W = true)
	{
		(*this).u = U, (*this).v = V, (*this).w = W;
	}

	void reverse()
	{
		std::swap((*this).u, (*this).v);
		return;
	}

	Edge getReversed()
	{
		return Edge((*this).v, (*this).u, (*this).w);
	}

	Edge operator = (const Edge& inEdge)
	{
		(*this).u = inEdge.u, (*this).v = inEdge.v, (*this).w = inEdge.w;
		return *this;
	} 
}
emptyEdge;

inline bool cmp(Edge E, Edge e)
{
	return E.v < e.v;
}

inline void copy(int n, int* ori, int* des)
{
	for(int i = 1; i <= n; i++) des[i] = ori[i];
}

inline void set(int n, bool stat, bool* des)
{
	for(int i = 1; i <= n; i++) des[i] = stat;
}

inline bool isMinDic(int n, int* dicOri, int* dicCmp)
{
	for(int i = 1; i <= n; i++)
	{
		if(dicCmp[i] != dicOri[i])
		{
			if(dicCmp[i] > dicOri[i]) return false;
			if(dicCmp[i] < dicOri[i]) return true;
		}
	}
	return false;
} 

inline void setEdge(int u, int v, bool w, std::vector<Edge>* graph)
{
	for(std::vector<Edge>::iterator it = graph[u].begin(); it != graph[u].end(); it++)
	{
		if((*it).v == v)
		{
			(*it).w = w;
			return;
		}
	}
}

void DFS(int now, int& depth, bool* isVisited, std::vector<Edge>* graph, int* dic, int* cmpDic, bool& wasEqual, bool& isBestSolution, bool& notBestSolution)
{
	if(notBestSolution) return;
	isVisited[now] = true;
	dic[depth] = now;
	if(wasEqual && !isBestSolution)
	{
		dic[depth] != cmpDic[depth] ? (dic[depth] < cmpDic[depth] ? isBestSolution = true : false),
									  (dic[depth] > cmpDic[depth] ? notBestSolution = true : false),
									  wasEqual = false
									: wasEqual = true;
	}
	depth++;
	for(std::vector<Edge>::iterator it = graph[now].begin(); it != graph[now].end(); it++)
	{
		if(!isVisited[(*it).v] && (*it).w)
			DFS((*it).v, depth, isVisited, graph, dic, cmpDic, wasEqual, isBestSolution, notBestSolution);
	}
}

inline void subtask(int n, int& depth, bool* isVisited, std::vector<Edge>* graph, int* dic, int* resultDic, bool& wasEqual, bool& isBestSolution, bool& notBestSolution)
{
	for(int k = 1;k <= n;k++)
	{
		for(std::vector<Edge>::iterator it = graph[k].begin(); it != graph[k].end(); it++)
		{
			(*it).w = false/*, setEdge((*it).v, (*it).u, false, graph)*/;

			int tDic[1 + n];
			tDic[0] = 0, depth = 1, wasEqual = true, isBestSolution = false, notBestSolution = false;
			for(int i = 1; i <= n; i++) {tDic[i] = 5000 + 1; isVisited[i] = false;}

			DFS(1, depth, isVisited, graph, tDic, resultDic, wasEqual, isBestSolution, notBestSolution);
			if(isBestSolution) copy(n, tDic, resultDic);

			(*it).w = true/*, setEdge((*it).v, (*it).u, true, graph)*/;
		}
	}
}

int main()
{
	//freopen("in","r",stdin);
	int n, m;
	std::cin >> n >> m;
	std::vector<Edge> graph[1 + n];
	for(int i = 1; i <= m; i++)
	{
		int u, v;
		scanf("%i %i", &u, &v);
		graph[u].push_back(Edge(u, v));
		graph[v].push_back(Edge(v, u));
	}
	for(int i = 1; i <= n; i++)
	{
		std::sort(graph[i].begin(), graph[i].end(), cmp);
	}

	int dic[1 + n]; for(int i = 1;i <= n;i++){dic[i] = 5000 + 1;} dic[0] = 0;
	bool isVisited[1 + n]; memset(isVisited, false, sizeof(isVisited));
	int depth = 1; bool wasEqual = true, isBestSolution = false, notBestSolution = false;
	n != m ? DFS(1, depth, isVisited, graph, dic, dic, wasEqual, isBestSolution, notBestSolution)
		   : subtask(n, depth, isVisited, graph, dic, dic, wasEqual, isBestSolution, notBestSolution);

	for(int i = 1; i <= n; i++)
	{
		printf("%i ", dic[i]);
	}
	
	return 0;
}
```
