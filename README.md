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


# Binary Search

### Pattern Identification for Binary Search
- **Minimum / Maximum keywords** are present in the problem  
  (e.g., "minimum time", "maximum capacity", "smallest index").
- The array (or search space) is **sorted** or can be made sorted.
- The answer space can be **monotonic**  
  (true for some range, false for the rest — or vice versa).
---

### Lower Bound (`std::lower_bound`)
- **Definition:** Returns the first position where the element is **not less than** the target (`>= target`).
- **If target is found:** Returns the **first occurrence** index.
- **If target is not found:** Returns the index of the **first element greater than** target.

```c++
    int idx = lower_bound(v.begin(), v.end(), target) - v.begin();
    [2, 4, 5, 6, 6, 6, 9]
    Target = 6          Target = 7
    Answer = 3          Answer = 6
```
  


### Upper Bound  (`std::lower_bound`)
- **Definition:** Returns the first index in a sorted array where the element is **greater than** the target.
- **If target is found:** Returns the index just **after the last occurrence** of the target.
- **If target is not found:** Returns the index of the **first element greater than** the target.
- **If all elements are ≤ target:** Returns `end()`.

```c++
    int idx = upper_bound(v.begin(), v.end(), target) - v.begin();
    [2, 4, 5, 6, 6, 6, 9]
    Target = 6          Target = 7
    Answer = 6          Answer = 6
```

---
```c++
# Implement binary search on answer template
    vector<int> binarySearchOnAnswer(vector<int> nums) {
        int l = 0;
        int h = 1000000000;
        int ans = 0;
        while(i<=j) {
            int mid = l + (h-l)/2;
            if(isValid(nums,mid)) {
                ans = mid;
                h = mid - 1; // search on left window for better answer
            }
            else {
                l = mid + 1; // search on right window if answer not found
            }
        }
        return ans;
    }
```
