---
title: "Graph Traversal Algorithm"
tags:
  - BFS
  - DFS
  - Algorithm
  - Graph Traversal
---

💡 This post introduces how to solve the Graph Traversal Problem on leetcode Q.1971: Find if Path Exists in Graph.
{: .notice--warning}
![q](https://user-images.githubusercontent.com/40441643/206105330-1ee75fbe-fd98-48d7-abef-c71724380631.PNG)
![q1](https://user-images.githubusercontent.com/40441643/206105577-fd3408f2-0c7f-4f7b-9dc2-c673360d8622.PNG)
![q2](https://user-images.githubusercontent.com/40441643/206105584-08a86d21-64c8-441f-8c25-ffd4ac19cdec.PNG)

### Problem Overview
- Given a bi-directional graph in this problem, the task is: for the two nodes `source` and `destination` in this graph, we need to check if there exists a path from `source` to `destination`.
- Start traversal from one node and check if we can reach the other one.

### Solutions
Two methods to solve this problem:
1. Breadth-first search(BFS)
2. Depth-first search(DFS)
3. If there exists a `path` between two given nodes, two nodes must be **connected** thus, this problem can also be solved using **Disjoint Set Union(DSU)**.

### Approach 1: Breadth First Search(BFS)

- Algorithm
1. Initialise an empty `queue` to **store the nodes** to be visited.
2. Use one `boolean array` seen to **mark all visited nodes** and `hasp map(graph`) to **store all edges**.
3. **Add** the starting node `0` to `queue` and **mark** it as `visited`
4. If `queue` has `nodes`, **get** the `first node(curr_node)` from queue. Return true if curr_node is destination, otherwise go to step 5.
5. **Add** `all unvisited neighbour nodes` of curr_node to `queue` and mark them as `visited`, then repeat step 4.
6. If we `emptied` queue without finding destination, return `false`.


```
class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        //Store all edges in 'graph'.
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] edge : edges) {
            int a = edge[0], b = edge[1];
            graph.computeIfAbsent(a, val -> new ArrayList<Integer>()).add(b);
            graph.computeIfAbsent(b, val -> new ArrayList<Integer>()).add(a);
        }
        
        // Store all the nodes to be visited in 'queue'.
        boolean[] seen = new boolean[n];
        seen[source] = true;
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(source);
        
        while (!queue.isEmpty()) {
            int currNode = queue.poll();
            if (currNode == destination) {
                return true; 
            }

            // For all the neighbours of the current node, if we haven't visit it before,            
            // add it to 'queue' and mark it as visited.
            for (int nextNode : graph.get(currNode)) {
                if (!seen[nextNode]) {
                    seen[nextNode] = true;
                    queue.offer(nextNode);
                }
            }
        }
        
        return false;
    }
}
```
- Complexity Analysis
<br>
Let `n` be the number of `nodes` and `m` be the number of `edges`.
1. Time Complexity: O(n+m)
<br>
In a `typical BFS search`, the time complexity is **O(V+E)** where V is the number of vertices and E is the number of edges.
<br>
We build `adjacent list` of all m `edges` in graph which takes O(m).
<br>
Each node is added to the `queue` and poppend from queue once, it takes O(n) to handle all nodes.
2. Space Complexity: O(n+m)
<br>
We used a `hash map neighbours` to **store all edges**, which requires O(m) space for m edges.
<br>
We use seen, either a has set or an `array` to **record the visited nodes**, which takes O(n) space.

### Approach 2: Depth First Search(DFS), Recursive
- Intuition
<br>
In DFS, we explore nodes as far as possible along each branch. Upon reaching the end of the current branch, we **backtrack** to the next possible branch and continue exploring.
![q3](https://user-images.githubusercontent.com/40441643/206110485-30e40b50-9b3d-443b-8b5c-ec0748e78c95.PNG)

Once we encounter an `unvisited node`, we take `one of its neighbour nodes`(if it exists) as the `next node` on this branch.
<br>
**Recursively call** the function to take next node as `starting node` and solve the subproblem.
<br>
If we reach the end of this branch, we **backtrack** to the `previous node` and visit the next neighbour node(if it exists), and repeat the process. 
<br>
Similarly, we also use a `boolean array` seen to **record every visited node**, so they won’t be visited by other nodes anymore.
- Algorithm
1. Use one `boolean array` seen to **mark all visited nodes**, set `seen[source] = true`.
2. Use a `hash map graph` to **store all edges**.
3. Start the search from node source. If the current node we are visiting(curr_node) equals destination, return true. Otherwise, find all its neighbour nodes that have not visited before.
If there exists such a `neighbour node`, make it as `visited`, move on to this node and repeat step 3.
If this `neighbour node` can **reach** `destination`, then return `true`, otherwise, try the `next neighbour`.
4. Return `false` if we finished the search without finding destination.

```
 class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        boolean[] seen = new boolean[n];

        for (int[] edge : edges) {
            int a = edge[0], b = edge[1];
            graph.computeIfAbsent(a, val -> new ArrayList<Integer>()).add(b);
            graph.computeIfAbsent(b, val -> new ArrayList<Integer>()).add(a);
        }
        
        return dfs(graph, seen, source, destination);
    }
    
    private boolean dfs(Map<Integer, List<Integer>> graph, boolean[] seen, int currNode, int destination) {
        if (currNode == destination) {
            return true;
        }
        if (!seen[currNode]) {
            seen[currNode] = true;
            for (int nextNode : graph.get(currNode)) {
                if (dfs(graph, seen, nextNode, destination)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
- Complexity Analysis
<br>
Let `n` be the number of `nodes` and `m` be the number of `edges`.
1. Time Complexity: O(n+m)
<br>
In `typical DFS search`, the time complexity is O(V+E) where V, E are the number of vertices and edges.
<br>
We build `adjacent list` of all `m edges` in graph, which takes O(m).
<br>
Each node is only visited once, it takes O(n) to traverse all nodes.
2. Space Complexity: O(m+n)
<br>
Use a `hash map` to **store m edges**, which takes O(m) space.
<br>
Use a `boolean array` seen to keep track of `visited nodes`,which requires O(n) space.
<br>
The `recursive function` take O(n) space.

### Approach 3: Depth First Search(DFS), Iterative
- Intuition
<br>
We can also implement DFS **iteratively** using a `stack` to replicate **recursive self calls**.
Since the operations on a stack are performed in **First In, Last Out(FILO)** order.
Therefore, the `top node` on the `stack` always `leads` to the `next branch`:
Whenever we `reach` the end of the current branch, we can get the `node` on the `top` of the `stack` and move along the branch that starts from it.

Similarly, we can use a `boolean array` seen to **record the status of each node**,
we mark the visited nodes as `visited` so we don’t need to take them into account.
Once we **add** an `unvisited node` to `stack`, we immediately **mark** it `visited` to prevent if from being revisited later.
- Algorithm
1. Initialise an empty `stack` to **store the nodes** to be `visited`.
2. Use one `boolean array` seen to **mark all visited nodes** and `hash map graph` to **store all edges**.
3. **Add** the `starting node`(source) to `stack` and **mark** it `visited`.
4. While `stack` has `nodes`, get the `top node`(curr_node) from `stack`. 
If curr_node equals destination, return `true`.
Otherwise, **add** `unvisited neighbour nodes` of curr_node to `stack` and mark them as `visited` and repeat step 4.
5. If we finish the iteration without finding destination, return `false`.

```
class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        // Store all edges according to nodes in 'graph'.
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] edge : edges) {
            int a = edge[0], b = edge[1];
            graph.computeIfAbsent(a, val -> new ArrayList<Integer>()).add(b);
            graph.computeIfAbsent(b, val -> new ArrayList<Integer>()).add(a);
        }
        
        // Start from source node, add it to stack.
        boolean[] seen = new boolean[n];
        seen[source] = true;
        Stack<Integer> stack = new Stack<>();
        stack.push(source);
        
        while (!stack.isEmpty()) {
            int currNode = stack.pop();
            if (currNode == destination) {
                return true;
            }
            // Add all unvisited neighbours of the current node to stack'
            // and mark it as visited.
            for (int nextNode : graph.get(currNode)) {
                if (!seen[nextNode]) {
                    seen[nextNode] = true;
                    stack.push(nextNode);
                }
            }
        }
        
        return false; 
    }
}
```
- Complexity Analysis
<br>
1. Time Complexity: O(n+m)
<br>
Build `adjacent list` of all `m edges` in graph which takes O(m).
<br>
Each node is added to the `stack` and popped from stack once, it takes O(n) to handle all nodes.
2. Space Complexity: O(n+m)
<br>
Use a `hash map` to **store m edges**, it takes O(m) space.
<br>
Use one `boolean array `seen to **record visited nodes**, which also takes O(n) space.
<br>
Use a `stack` to **store all does to be visited**, in the worst-case scenario, there may be O(n) nodes in `stack`.

### Approach 4: Disjoint Set Union
- Intuition
<br>
In short, `DSU data structure` stores `disjoint subsets `and provides operations for adding new sets and merging sets.
It also allows us to find out **if any two given elements are in the same group or not**. 
Back to this problem: if there exists a `path` connecting `source` and `destination`, **these two nodes must be in the same group**. 
<br>
<br>
Let’s first assume that there is no edge in the graph and that all these nodes are isolated.
Then we connect them using edges from edges, for each edge `edge = [node_a, node_b]`, we connect `node_a` with `node_b`, represents that these `two nodes` belong to the **same group**. 
- Algorithm
1. Initialise the DSU data structure `UnionFind` containing all nodes. Each node i has:
A distinct `root`, which means each point is individual and a group size of 1, which means each set only contains one node.
The **DSU** also supports:
`find(x)`: find the `root` of the `node x`.
`union(x,y)`: if two given nodes x and y are not in the **same group**, we **modify** `one of the roots` as the other `root`, which means that the **two group** containing x and y are `merged` into **one group**. Note that we use the **union-by-rank method** to optimise the `time complexity`, basically, we **modify** the `root` of the **smaller group** as the `root` of the **larger group**.
2. **Iterate** over `all edges`. For each `edge = [node_a, node_b]`, use the DSU data structure we initialised to connect `node_a` and `node_b`.
3. Check if node `source` and node `destination` are in the **same group**.


```
class UnionFind {
    private int[] root;
    private int[] rank;
    public UnionFind(int n) {
        this.root = new int[n];
        this.rank = new int[n];
        for (int i = 0; i < n; ++i) {
            this.root[i] = i;
        }
    }   
    public int find(int x) {
        if (this.root[x] != x) {
            this.root[x] = find(this.root[x]);
        }
        return this.root[x];
    }
    public void union(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX != rootY) {
            if (this.rank[rootX] > this.rank[rootY]) {
                int tmp = rootX;
                rootX = rootY;
                rootY = tmp;
            }
            // Modify the root of the smaller group as the root of the
            // larger group, also increment the size of the larger group.
            this.root[rootX] = rootY;
            this.rank[rootY] += this.rank[rootX];
        }
    }
}

class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        UnionFind uf = new UnionFind(n);

        for (int[] edge : edges) {
            uf.union(edge[0], edge[1]);
        }

        return uf.find(source) == uf.find(destination);
    }
}
```
- Complexity Analysis
<br>
1. Time Complexity: O(m⋅α(n))
<br>
The amortised complexity for performing *m* union find operations is O(m⋅α(n)) time where α is the **Inverse Ackermann Function**.
2. Space Complexity: O(n+m)
<br>
We used **two arrays** `root` and `rank` to save the root and rank of each node in the DSU data structure, each of them takes O(n) space.




