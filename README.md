# Dijkstra's Algorithm

- https://www.youtube.com/watch?v=xQ3vjWwFRuI
- Dijkstra's algorithm finds the shortest path from a source node to all other nodes in a graph with **non-negative edge weights**.
- It uses a **min-heap priority queue** and a **greedy approach**.

```
    Input : Adjacency list
    u -> {v,w}         u - source, v - destination, w - weight
    2 -> {0,6}, {1,3}
    1 -> {2,3}, {0,1}
    0 -> {1,1}, {2,6}
    
    source = 2
```
```
    Output : [4,3,0]
    Shortest distance between 2 to 0 : 4
    Shortest distance between 2 to 1 : 3
    Shortest distance between 2 to 2 : 0
```
```
    Approach:
    
    1. Maintain a `result` vector to store the shortest distance of each node from the source, initialized to INT_MAX. Set `result[source] = 0`.
    
    2. Create a min-heap (priority queue) and push `{0, source}` — since the shortest distance from the source to itself is 0.
    
    3. The priority queue always returns the node with the current shortest known distance at the top.
    
    4. For each neighbor of the current node, check if a shorter path is found via this node. If yes, update the distance and push `{newDistance, neighbor}` into the heap.
    
    5. Update the `result` vector whenever a shorter path is found to any node.
```

```c++
# Implement dijkstra using min-heap priority queue
    vector<int> dijkstraAlgorithm(vector<vector<pair<int,int>>> adjacencyList, int source) {
        vector<int> result(adjacencyList.size(), INT_MAX);
        result[source] = 0;
    
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
        pq.push({0, source});
    
        while (!pq.empty()) {
            auto [dis, node] = pq.top();
            pq.pop();
    
            for (auto& [neiNode, neiWeight] : adjacencyList[node]) {
                int distanceTillNow = dis + neiWeight;
    
                if (result[neiNode] > distanceTillNow) {
                    result[neiNode] = distanceTillNow;
                    pq.push({distanceTillNow, neiNode});
                }
            }
        }
        return result;
    }
```
