---
title: "C++ 레퍼런스 적용 전과 후의 성능 차이"
date: "2025-01-05"
tags:
    - "C++"
thumbnail: "/assets/img/Code/C++/레퍼런스%20(2).png"
---

# 문제점
---
백준에서 DFS와 BFS 문제를 풀던 중 시간이 이상하게 길게 나오는 것을 발견했다.

전체 코드:
```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

void DFS(const vector<vector<int>> tree, vector<bool>& isTraveled, int root) {
	cout << root << " ";
	isTraveled[root] = true;

	for (int i : tree[root]) {
		if (!isTraveled[i]) {
			DFS(tree, isTraveled, i);
		}
	}
}

void BFS(const vector<vector<int>> tree, vector<bool>& isTraveled, int root) {
	queue<int> Q;
	isTraveled[root] = true;
	Q.push(root);

	while (!Q.empty()) {
		int current = Q.front(); Q.pop();
		cout << current << " ";
		for (int i : tree[current]) {
			if (!isTraveled[i]) {
				isTraveled[i] = true;
				Q.push(i);
			}
		}
	}
}

int main(void) {
	int node, edge, root;
	cin >> node >> edge >> root;

	vector<vector<int>> tree(node + 1);

	for (int i = 0; i < edge; i++) {
		int t1, t2;
		cin >> t1 >> t2;
		tree[t1].push_back(t2);
		tree[t2].push_back(t1);
	}

	for (vector<int>& v : tree) {
		sort(v.begin(), v.end());
	}

	{
		vector<bool> isTraveled(node + 1, false);
		DFS(tree, isTraveled, root);
	}
	cout << endl;
	{
		vector<bool> isTraveled(node + 1, false);
		BFS(tree, isTraveled, root);
	}
}
```

```C++
void DFS(const vector<vector<int>> tree, vector<bool>& isTraveled, int root) {
	cout << root << " ";
	isTraveled[root] = true;

	for (int i : tree[root]) {
		if (!isTraveled[i]) {
			DFS(tree, isTraveled, i);
		}
	}
}
```
위 코드에서 함수 정의 부분을 살펴보면 현재 tree 벡터를 복사로 받고 있는데, 함수 호출시 벡터 복사에 걸리는 시간이 원인인듯 하여 레퍼런스로 받도록 바꿔보았다.

# 개선
---
```C++
void DFS(const vector<vector<int>>& tree, vector<bool>& isTraveled, int root)
```
tree 벡터를 레퍼런스로 받도록 하여 불필요한 복사가 일어나지 않도록 수정했다.

# 결과
---
![](/assets/img/Code/C++/레퍼런스%20(2).png)

아래 두개 제출이 수정하기 전, 맨 위 제출이 수정한 후이다.
기존 172ms에서 8ms로 극적인 단축이 있었고, 메모리 사용량 또한 격감한 것을 확인할 수 있다.

단순히 함수 호출에서 값으로 가져오던 것을 참조로 가져옴으로서 이러한 성능 향상을 얻을 수 있었다. 이렇듯 C++에 레퍼런스가 최초로 도입될 때 여러 STL 컨테이너들의 성능이 향상되었던 데에는 이러한 배경이 있었던 것이다...