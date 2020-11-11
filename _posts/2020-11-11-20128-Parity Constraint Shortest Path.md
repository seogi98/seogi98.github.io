---
layout: post
title: "백준 20218번 Parity Constraint Shortest Path"
date: 2020-11-11
excerpt: "다익스트라 응용문제"
tags: [백준,알고리즘,G3,다익스트라,그래프 이론]
comments: true
---
# 2206번 확장 문제

문제 링크 : [백준 20218번 문제 링크](https://www.acmicpc.net/problem/20218).

## 1. 문제 풀이 방법

다익스트라 문제입니다. 대신 홀수 경로의 최소값과 짝수 경로의 최솟값을 따로 구해야 하므로
dst[노드][짝수 또는 홀수]로 최소거리를 저장해야합니다.

## 2. 유의사항

다익스트라 탐색할때 탐색하는 노드의 거리가 이미 작으면 continue처리를 해줘야지 시간 초과를 받지 않습니다.

```c++
if(curD > dst[curNode][evenOdd])
{
	continue;
}
```

## 3. 코드
{% raw %}
```c++
#include <bits/stdc++.h>
using namespace std;
#define MAX 100001
#define INF 999999999999999

long long dst[MAX][2];
vector<pair<long long,long long>> E[MAX];
bool cmp(pair<long long,long long> p1,pair<long long,long long> p2)
{
	return p1.second < p2.second;
}
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	long long N,M;
	cin >> N >> M;
	for(long long i=1;i<=N;i++)
	{
		dst[i][0] = INF;
		dst[i][1] = INF;
	}
	for(long long i=1;i<=M;i++)
	{
		long long node1 ,node2 ,weight;
		cin >> node1 >> node2 >> weight;
		E[node1].push_back({node2, weight});
		E[node2].push_back({node1, weight});
	}
	for(long long i=1;i<=N;i++)
	{
		sort(E[i].begin(),E[i].end(),cmp);
	}
	//(-weight,node)
	priority_queue<pair<long long,long long>> pq;
	dst[1][0] = 0;
	pq.push({0,1});
	while(!pq.empty())
	{
		long long curD = -pq.top().first;
		long long curNode = pq.top().second;
		pq.pop();
		int evenOdd = curD%2;
		if(curD > dst[curNode][evenOdd])
		{
			continue;
		}
		for(long long i=0;i<E[curNode].size();i++)
		{
			long long nxNode = E[curNode][i].first;
			long long nxD = E[curNode][i].second + curD;
			// nxD 짝수 홀수 판단
			long long evenOdd = nxD%2;
			if(dst[nxNode][evenOdd] > nxD)
			{
				dst[nxNode][evenOdd] = nxD;
				pq.push({-nxD,nxNode});
			}
		}
	}
	for(long long i=1;i<=N;i++)
	{
		if(dst[i][0] >= INF)
		{
			dst[i][0] = -1;
		}
		if(dst[i][1] >= INF)
		{
			dst[i][1] = -1;
		}
		cout << dst[i][1] << " " << dst[i][0] << "\n";
	}
}

```
{% endraw %}







