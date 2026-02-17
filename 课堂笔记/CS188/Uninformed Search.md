#CS188 
解决搜索问题的通用方案是：维护搜索树的一个**边境**(Frontier) 结点集合，我们通过删除一个结点并加入结点的所有子节点，来**拓展**(Expand) 边境，直到到达目标结点为止。

其中，对于删除结点的选取方式被称为**策略**(Strategy)，不同的选取策略也就产生了不同的搜索算法。

伪代码：
```
function TREE-SEARCH(problem, frontier) return a solution or failure
    frontier ← INSERT(MAKE-NODE(INITIAL-STATE[problem]), frontier)
    while not IS-EMPTY(frontier) do
        node ← POP(frontier)
        if problem.IS-GOAL(node.STATE) then return node
        for each child-node in EXPAND(problem, node) do
            add child-node to frontier
    return failure
```

```
function EXPAND(problem, node) yields nodes
    s ← node.STATE
    for each action in problem.ACTIONS(s) do
        s' ← problem.RESULT(s, action)
        yield NODE(STATE=s', PARENT=node, ACTION=action)
```

## Uninfromed Search
当我们不清楚目标结点在搜索树中的位置时，我们只能进行**无信息搜索**(Uninformed Search)。

无信息搜索主要有以下三种策略：
- **深度优先搜索**(Deep-First Search, DFS)。
- **广度优先搜索**(Breadth-First Search, BFS)。
- **一致代价搜索**(Uniform Cost Search, UCS)。

不同的策略具有不同的基本特性，包括：
- **完整性**：当搜索问题有解时，假设有无限计算资源的情况下是否保证一定能找到解？
- **最优性**：是否保证找到的路径一定是最优的？
- **分支因子 $\displaystyle b$**：边境结点每次的增加量是 $\displaystyle O(b)$ 的，深度为 $\displaystyle k$ 的搜索树的节点数为 $\displaystyle O(b^k)$。
- **最大深度 $\displaystyle m$**
- **最浅方案的深度 $\displaystyle s$**

以下三种算法，除了边境拓展策略不同以外，基本原理是完全相同的：
### Depth-First Search
**策略**：总是拓展距离初始结点**最深**的边境结点。
**边境的表示**：通常用**栈**实现。
**完整性**：不完整，有可能在有环状态空间图上无限循环。
**最优性**：总是找到最“左”的解，而不关心路径代价，因此不是最优的。
**时间复杂度**：$\displaystyle O(m)$
**空间复杂度**：$\displaystyle O(bm)$

### Breadth-First Search
**策略**：总是拓展距离初始结点**最浅**的边境结点。
**边境的表示**：通常用**队列**实现。
**完整性**：完整的。
**最优性**：同样不关心路径代价，非最优。
**时间复杂度**：$\displaystyle O(b^s)$
**空间复杂度**：$\displaystyle O(b^s)$

### Uniform Cost Search
**策略**：总是拓展距离初始结点**代价最小**的边境结点。
**边境的表示**：通常用**优先队列**实现，优先级由*后向代价*决定。
**完整性**：完整的。
**最优性**：当所有边的代价非负时最优。
**时间复杂度**：假设最优路径长度为 $\displaystyle C*$，相邻结点的最小代价为 $\displaystyle \epsilon$，则时间复杂度为 $\displaystyle O\left( b^ {\frac{C*}{\epsilon}} \right)$
**空间复杂度**：$\displaystyle O\left( b^ {\frac{C*}{\epsilon}} \right)$


