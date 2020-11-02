---
layout: post
title: "백준 20046번 Road Reconstruction"
date: 2020-10-30
excerpt: "다익스트라로 최소비용 찾기"
tags: [백준,알고리즘,G4,다익스트라,그래프 이론]
comments: true
---
# 다익스트라로 최소비용 찾기

문제 링크 : [백준 20046번 문제 링크](https://www.acmicpc.net/problem/20046).

## 1. 문제 접근 방법

(1,1)에서 (N,M)까지 도로를 최소비용으로 건설하고 이동해야 하는 문제입니다.
따라서 다익스트라를 이용하여 풀이 하였습니다.
비용이 최소로 드는 방법부터 우선순위 큐에서 뽑아내어 탐색 => (N,M) 가장 도착할 경우 최소 비용이므로 break 하면 최소비용을 구할 수 있습니다.

## 2. 유의사항
(1,1)점이 -1여서 경로를 만들 수 없을수가 있습니다. 따라서 시작점이 -1일경우
예외처리를 해주어야 합니다.

## 3. 전체 코드
```c++
// 백준 20046번 Road Reconstruction
// BFS 탐색문제
// 0,0 -> M-1,N-1탐색
#include <bits/stdc++.h>
using namespace std;
#define MAX 1001
#define INF 987654321
int mp[MAX][MAX];
int dx[4] = {1, 0, -1, 0};
int dy[4] = {0, 1, 0, -1};
// pq로 접근하기때문에 먼저 방문하면 그뒤에 방문할 필요가 없다.
bool vst[MAX][MAX];
int N, M;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	memset(vst,false,sizeof(vst));
	int ans=-1;
	cin >> M >> N;
	for (int i = 0; i < M; i++)
	{
		for (int j = 0; j < N; j++)
		{
			cin >> mp[i][j];
		}
	}
	if(mp[0][0]==-1)
	{
		cout << -1;
		return 0;
	}
	priority_queue<pair<int, pair<int, int>>> pq; // (건설 비용,(Y,X))
	pq.push({-mp[0][0], {0, 0}});
	vst[0][0] = true;
	while (!pq.empty())
	{
		int Y = pq.top().second.first;
		int X = pq.top().second.second;
		int cost = -pq.top().first;
		pq.pop();
		if(Y == M-1 && X == N-1)
		{
			ans = cost;
		}
		for (int i = 0; i < 4; i++)
		{
			int nx = X + dx[i];
			int ny = Y + dy[i];
			if (nx < 0 || nx >= N || ny < 0 || ny >= M || mp[ny][nx] == -1)
			{
				continue;
			}
			if(vst[ny][nx]) continue;
			vst[ny][nx] = true;
			pq.push({-(cost+mp[ny][nx]),{ny,nx}});
		}
	}
	cout << ans;
}
```

