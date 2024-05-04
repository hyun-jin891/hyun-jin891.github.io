---
layout: post
title: Dijkstra Algorithm
description: >
  Algorithm
tags: [dijkstra algorithm]
use_math: true
categories:
  - study
  - computer science
---
### Dijkstra Algorithm
* 다익스트라 or 데이크스트라 algorithm
* In suggested graph, we can use it for calculating the shortest distance from the same start node to other nodes
* BFS vs Dijkstra
  * BFS : mainly used for calculating the shortest distance in the graph which has no weight
  * Dijkstra : mainly used for calculating the shortest distance in the graph which has specific weight on every edges

### Procedure
Example with suggested graph <br>
![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/153.png?raw=true){: width="600" height="600"}<br>

* node 0 ~ 5 with the weight (drawn on the edges)
* Start with node 0 (visited.append(0))
* distance from node 0 to node 0 is 0

<br>

![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/154.png?raw=true){: width="600" height="600"}<br>

* Current node : node 0
* check the nodes which are directly connected with node 0
  * They are node 1 (distance = 2), node 2 (distance = 4), node 3 (distance = 1)
  * node 4 and node 5 are INF (It is not connected with start node)

<br>

![그림3](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/155.png?raw=true){: width="600" height="600"}<br>

* Next node we want to visit : Except visited node, next node we want to visit has the shortest distance from start node → node 3 (visited.append(3))
* Current node : node 3
* check the nodes which are directly connected with node 3 except visited nodes
  * They are node 4 (distance = dist[3] + distance between node 3 and node 4 = 1 + 3 = 4)
  * We update dist[4] from INF to 4 (INF > 4)

<br>

![그림4](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/156.png?raw=true){: width="600" height="600"}<br>

* Next node we want to visit : Except visited node, next node we want to visit has the shortest distance from start node → node 1 (visited.append(1))
* Current node : node 1
* check the nodes which are directly connected with node 1 except visited nodes
  * They are node 2 (distance = dist[1] + distance between node 1 and node 2 = 2 + 1 = 3)
  * We update dist[2] from 4 to 3 (4 > 3)

<br>

![그림5](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/157.png?raw=true){: width="600" height="600"}<br>

* Next node we want to visit : Except visited node, next node we want to visit has the shortest distance from start node → node 2 (visited.append(2))
* Current node : node 2
* check the nodes which are directly connected with node 2 except visited nodes
  * They are node 5 (distance = dist[2] + distance between node 2 and node 5 = 3 + 1 = 4)
  * We update dist[2] from INF to 4 (INF > 4)

<br>

![그림6](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/158.png?raw=true){: width="600" height="600"}<br>

* Next node we want to visit : Except visited node, next node we want to visit has the shortest distance from start node → node 4 (visited.append(4))
* Current node : node 4
* check the nodes which are directly connected with node 4 except visited nodes
  * They are node 5 (distance = dist[4] + distance between node 4 and node 5 = 4 + 2 = 6)
  * No change (6 > 4)

<br>

![그림7](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/159.png?raw=true){: width="600" height="600"}<br>

* Next node we want to visit : Except visited node, next node we want to visit has the shortest distance from start node → node 5 (visited.append(5))
* Current node : node 5
* check the nodes which are directly connected with node 5 except visited nodes
  * No node
  * No change

### Implementation
* If we use linear Searching for the step that finds the next node we want to visit, Dijkstra Algorithm will be O(N<sup>2</sup>)
* If we use priority queue (heap), the time will be saved to O(NlogN)
* Make the graph
  * Graph will be adjacency list
  * Index is nodes' name
  * **Adjacency list value's form : [(connected node1, weight), (cn2, weight), (cn3, weight), ...]**
* **Priority Queue's value will be tuple(possible_dist, target node)**
  * possible_dist == calculated possible distance from start node to target node
  * The value which has short dist will have priority
* Code

<br>

~~~python
import heapq

N = int(input())          # Number of nodes
graph = []                # followed by code for making the graph

INF = 10 ** 10
distance = [INF] * N      

def dijkstra(start):
  distance[start] = 0
  pq = []
  heapq.heappush(pq, (0, start))

  while pq:
    possible_dist, current_node = heapq.heappop(pq) # The value that has the shortest p_dist in heap will be out

    if possible_dist > distance[current_node]:
      continue     # p_dist > distance[cn] means the distance which is closer to the shortest distance than possible_dist has been already calculated / That is, we can pass this (p_dist, c_node) pair

    for path in graph[current_node]: # Bring all path connected with current_node
      next_node = path[0]
      dist_for_next_node = path[1]

      if possible_dist + dist_for_next_node < distance[next_node]: # If the distance from new path is shorter than previously calculated distance
        distance[next_node] = possible_dist + dist_for_next_node
        heapq.heappush(pq, (distance[next_node], next_node))

dijkstra(0)  

print(distance)

~~~

<br>

### Example Problem
This is the private repository where I can only review my notes<br>
[Repository](https://github.com/hyun-jin891/hidden-post-hyunjin891-github-blog/blob/master/_posts/study/computer%20science/2024-02-17-Dijkstra-Algorithm.md)
