# play-with-graph-algorithms

Python implementation of imooc course [玩转图论算法](https://coding.imooc.com/class/370.html), will update as the course going.

Check/fork original repo from course instructor [liuyubobobo](https://github.com/liuyubobobo)!

## Notes
### Chapter 2 图的基本表示

1. 顶点Vertex，边Edge

2. 无向图Undirected Graph，有向图Directed Graph

3. 有权图Weighted Graph，无权图Unweighted Graph

4. 自环边，平行边

5. 没有自环边并且没有平行边的图称为简单图

6. 树是一种无环图

7. 无环图不一定一定是树，比如多个联通分量

8. 联通的无环图是树

9. 生成树就是指包括了原来图中的所有的点并且联通的图，该生成树的边数是V - 1，但是V - 1的边组成的新图并不一定是生成树

10. 只有连通图才有（并且一定有）生成树

11. 一个图一定有生成森林，但是并不一定有生成树（由于是否联通的问题）

12. 对于无向图来说，顶点相邻的边数就是degree

13. 邻接矩阵，邻接表来表示图

14. 邻接矩阵空间复杂度O(V^2)，建图O(E)，查看两点是否相邻O(1)，求一个点的相邻节点O(V)（相当于遍历全部节点）

15. 稀疏图（sparse graph）和稠密图（dense graph）-> 完全图（complete graph）

16. 邻接表的空间复杂度O(V + E)，建图O(E*V)，未优化时查看两点是否相邻O(degree(v))，求一个点的相邻节点O(degree(v))；优化后（使用HashSet）建图O(E)，查看两点是否相邻O(1)

### Chapter 3 图的深度优先遍历

17. 树结构的遍历不用担心重复访问的问题，但是图的遍历会有重复遍历的问题（由于可能存在环），需要记录是否该点被访问过

18. dfs模板：
```python
dfs(0, visited=[False] * V, lst=[])

def dfs(v, visited, lst):
    visited[v] = True
    # 近似于图的先序遍历
    lst.apppend(v)
    for w in adj(v):
        if not visited[w]:
            dfs(v, visited, lst)

# 感觉递归出口写在开头更好一些
def dfs(v, visited, lst):
    if visited[v]:
        return
    visited[v] = True
    # 近似于图的先序遍历
    lst.apppend(v)
    for w in adj(v):
        dfs(v, visited, lst)
```

19. 深度优先时间复杂度是O(V + E)，如果是联通图，可以简化成O(E)

### Chapter 4 图的深度优先遍历的应用

20. 可以利用visited数组（初始化全部为-1表示没有被访问过，其实赋什么都可以。访问时候赋给非负的值），而赋的值就是该节点的group id

21. 图的路径问题：询问两点之间有没有路径，如果属于同一个联通分量，意味着两点之间有路径

22. 深度优先遍历过程中记录当前递归栈的路径即可

23. 或者记录下当前遍历点的前一个点即可

23. 一个点到其他点的路径问题 -> 单源最短路径问题

24. 只需要使用一个pre数组（不需要visited）也可以做DFS遍历，可读性略低一点儿。

25. 原始单源最短路径问题是给出了源点到其他所有点的路径，但有时候我们只是单纯想知道从A点到B点能不能联通，路径是什么，并不关心A到其他点的路径是什么，此时就可以剪枝来加速。

26. 递归的语义尤为重要，要很清楚递归函数的意义和返回值（如果有的话）的意义。

27. 无向图中的环的检测：相当于从起点出发，看有没有路径返回起点（注意当前遍历点的下一个节点不能是上一个节点），也是图的深度优先遍历的思路

28. 二分图：顶点V可以分为不相交的两部分，其中图中每一条边的两个端点都分属于不同的两部分。处理匹配问题时候（比如分类）很有效

29. 思路就是对图深度优先遍历+对个当前访问的节点的下一层递归的节点（即当前边的下一个节点）染色成跟当前节点不一样的颜色；如果发现下一个节点已经染过色并且跟当前节点颜色相同，说明不是二分图了

30. 递归同时记录更多信息，并且利用返回值剪枝

31. 概念：图同构，平面图（无交叉）

### Chapter 5 图的广度优先遍历

32. 图的BFS时间复杂度是O(V+E)，相当于全部的点和边都访问了一遍

33. BFS求解单源最短路径问题跟DFS类似，使用一个pre数组，在往队列里添加节点的时候，设置该被添加的节点的父节点（即当前从队列里pop出来的那个节点）

34. BFS在求单源最短路径的时候的解是全局最优解。而使用DFS则需要全局比较（所有的paths）才能确定最优解

35. 可以在遍历的过程中多记录一个信息distance，记录下某个点target到source点的steps

36. 可以对无权图（即权值一致）直接用BFS求最短路径

37. 遍历过程中可以使用随机队列（随机的线性数据结构），比如迷宫的生成

### Chapter 7 图论搜索和人工智能

38. BFS能够比较快速解决无权图最短路径问题

39. 有时候并不能很快确定这是一个BFS问题，需要多抽象

40. 核心：状态表达。其实如果要最短的状态表达，往往就是BFS。即可以将某一个状态看做是图中的某一个顶点
 
### Chapter 8 桥和割点

41. 如果删除了一条边，整个图的联通分量数量变化，则这条边称为桥（Bridge）

42. 桥就意味着图中最脆弱的关系

43. 图中可以有多个桥；树中多有的边都是桥

44. 如何使得一个桥不再是桥？将其连接的两个分量再加一条边即可

45. 寻找桥的算法使用DFS就可以解决

### Chapter 9 哈密尔顿问题和状态压缩

46. 汉密尔顿回路即遍历完整张图回到原点，而且每个点只访问一次

47. 汉密尔顿路径就是遍历整张图，每个点只访问一次，此时起点的选择就很重要

48. TSP问题：以最小的代价在有权图中遍历完所有的点，而且每个点只能遍历一次