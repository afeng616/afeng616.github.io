# 操作系统学习

<!--more-->

## 死锁问题
死锁是指，多个进程在执行过程中，因争夺资源而造成的一种互相等待的现象。  
死锁的必要条件有：
- 互斥条件：资源不能被多个进程共享。
- 占有且等待条件：一个进程至少持有一个资源，并等待其他资源。
- 不剥夺条件：已获得的资源在未使用完之前，不能被其他进程抢占。
- 环路等待条件：存在一个进程的集合，使得每个进程都在等待其他进程持有的资源。

### 死锁检测
死锁检测是指，系统定期检查进程的状态，判断是否存在死锁。
死锁检测算法通常使用资源分配图（Resource Allocation Graph，RAG）来表示进程和资源之间的关系。通过检测图中是否存在环路来判断是否发生死锁。  
环路判断可通过DFS或拓扑排序等算法实现。  
1. DFS，从任意一个节点开始，优先向下一个节点遍历，直到无法继续遍历为止。若在遍历过程中发现已经访问过的节点，则说明存在环路。
```cpp
// DFS判断环路（递归）
bool hasCycle(int u, vector<vector<int>>& graph, vector<int>& visited) {
    visited[u] = 1;  // 标记为正在访问
    for (int v : graph[u]) {
        if (visited[v] == 1) {
            return true;  // 回到正在访问中的点，发现环
        }
        if (visited[v] == 0 && hasCycle(v, graph, visited)) {
            return true;
        }
    }
    visited[u] = 2;  // 标记访问完成
    return false;
}
bool detectDeadlock(int n, vector<vector<int>>& graph) {
    vector<int> visited(n, 0);
    for (int i = 0; i < n; ++i) {
        if (visited[i] == 0) {
            if (hasCycle(i, graph, visited)) return true;
        }
    }
    return false; // 没有环，没死锁
}
```
2. 拓扑排序，从入度为0的节点开始，依次删除该节点及其出边，直到所有节点都被删除。若最终发现还有节点未被删除，则说明存在环路。
```cpp
// 拓扑排序判断环路
vector<int> detectDeadlock(int n, vector<vector<int>>& graph) {
    vector<int> indegree(n, 0);        // 入度
    for (int u = 0; u < n; ++u) {
        for (int v : graph[u]) {
            indegree[v]++;
        }
    }

    queue<int> q;
    for (int i = 0; i < n; ++i) {
        if (indegree[i] == 0) q.push(i);  // 初始入度为0的点
    }

    int resultSize = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        resultSize++;
        for (int v : graph[u]) {
            if (--indegree[v] == 0) {
                q.push(v);
            }
        }
    }

    // 检查是否有环
    return resultSize != n; // 有环返回true
}
```
### 死锁避免
死锁避免是指，系统在资源分配时，采取一些措施来避免死锁的发生。常用的方法如Dijkstra提出的银行家算法（Banker’s Algorithm）。通过预判资源分配后的系统状态是否安全，来决定是否允许某进程申请资源。  
银行家算法的实现主要有两点：
1. 自愿请求处理：
    1. 判断请求是否超出该进程的最大需求。
    2. 判断请求是否超出系统的可用资源。
    3. 资源预分配，并系统状态是否安全。
       - 安全，分配成功。
       - 不安全，回滚做出的资源预分配。
2. 安全性检查：判断当前系统状态是否安全，即所有进程都能顺利完成。
   1. 循环判断每个进程是否能完成，若能完成，则将其释放的资源加入可用资源中。
   2. 若所有进程都能完成，则系统状态安全。




