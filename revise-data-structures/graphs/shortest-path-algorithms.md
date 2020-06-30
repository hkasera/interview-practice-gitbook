# Finding Shortest Path in a Graph

The shortest path problem is about finding a path between **2** vertices in a graph such that the total sum of the edges weights is minimum.

## Unweighted Graphs : Shortest Path Algorithms

For an unweighted graph, where the weight of all the edges is 1, this problem could be solved easily using Breadth First Search \(BFS\).

{% hint style="warning" %}
Think - Can we use Depth First Search to find shortest path between two vertices in an unweighted graph? If not, why?
{% endhint %}

BFS always visits vertices at same 'level' of the graph and then goes on to the next level. Hence, it guarantees that vertices are visited in the order of increasing distances from a source vertex.

On the other hand, DFS goes as far as it can from the source vertex and then returns back to unvisited adjacent nodes of visited vertices.Hence, DFS does not guarantee that if a vertex, v1 is visited before another vertex, v2 starting from a source vertex, then v1 is closer to the source than v2.

Hence, by applying BFS we can easily find the shortest path between two vertices in a graph.

## Weighted Graphs : Shortest Path Algorithms

Unlike unweighted graphs, in weighted graphs the weight of edge in these graphs can have any value. Hence, finding shortest path algorithm using BFS won't yield the correct result as we don't differentiate edges based on their weights.

There are three popular algorithms that are discussed below that help us find out the shortest path in weighted graphs.

### Dijkstra's Algorithm

This algorithm helps find the shortest paths from a _**source vertex**_ to all other vertices in the graph with _**non-negative edge weights**_. 

As we need to specify a **source vertex** in order to find the shortest path from it to all other vertices, it is often called as **single-source shortest path** algorithm.

To learn more on how this algorithm works, check out the following resources :

* 📽 [Dijkstra's Algorithm Explained](https://www.youtube.com/watch?v=pVfj6mxhdMw)
* 📝 [Finding The Shortest Path, With A Little Help From Dijkstra](https://medium.com/basecs/finding-the-shortest-path-with-a-little-help-from-dijkstra-613149fbdc8e)

{% hint style="info" %}
* Dijkstra’s algorithm is a **Greedy** algorithm.
* Dijkstra doesn’t work for graphs with negative weight edges.
* Time Complexity of Dijkstra's Algorithm is **O\(V + E\*logV\)** with the use of priority queue, where V is no. of vertices and E is the no. of edges.
{% endhint %}

#### Practice Problems

* [https://leetcode.com/problems/network-delay-time/](https://leetcode.com/problems/network-delay-time/)

### Bellman Ford's Algorithm

This algorithm helps find the shortest paths from a _**source vertex**_ to all other vertices in the weighted graph. It is slower than Dijkstra's algorithm for the same problem, but more versatile, as it is capable of handling graphs in which some of the edge weights are negative numbers. 

### Floyd–Warshall Algorithm

This algorithm helps find the shortest paths between between _**all pairs of vertices**_ in a weighted graph. This algorithm works for negative weighted edges but **not for negative cycles**.

To learn more on how this algorithm works, check out the following resources :

* 📽 [Floyd Warshall All Pairs Shortest Path Algorithm](https://www.youtube.com/watch?v=4NQ3HnhyNfQ)
* 📝 [Floyd-Warshall Algorithm](https://brilliant.org/wiki/floyd-warshall-algorithm/)

{% hint style="info" %}
* The Floyd–Warshall algorithm is an example of **dynamic programming**.
* A single execution of the algorithm will find the shortest paths between all pairs of vertices.
{% endhint %}

## References

* [https://www.hackerearth.com/practice/algorithms/graphs/shortest-path-algorithms/tutorial/](https://www.hackerearth.com/practice/algorithms/graphs/shortest-path-algorithms/tutorial/)
* [https://www.youtube.com/watch?v=pSqmAO-m7Lk](https://www.youtube.com/watch?v=pSqmAO-m7Lk)
* [https://medium.com/free-code-camp/i-dont-understand-graph-theory-1c96572a1401](https://medium.com/free-code-camp/i-dont-understand-graph-theory-1c96572a1401)

