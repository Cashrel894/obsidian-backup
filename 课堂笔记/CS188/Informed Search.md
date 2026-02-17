#CS188 
尽管 UCS 既具有完整性、又具有最优性，但它相对较慢，因为它会试图向**任何方向**拓展。如果我们知道需要搜索的大致方向，就可以显著提高算法性能，更快的找到目标。这种策略就被称为**有信息搜索**(Informed Search)。

## Heuristics
**启发法**(Heuristics) 允许搜索时估计当前状态到目标状态的距离，搜索时将优先选取估计距离较小的结点进行拓展。

**启发函数**就是根据当前状态输出预估距离的函数，并且通常来讲启发函数为实际距离的一个下界。

例如对于吃豆人相关的问题，一个合适的启发函数是**曼哈顿距离**。

启发法非常强大，有两种算法就运用了启发函数的思想：**贪婪搜索**和 **A\***。

## Greedy Search
**策略**：总是拓展**启发值最小**的边境结点，即预估距离目标结点最近的边境结点。
**边境的表示**：通常用**优先队列**实现，优先级由**预计前向代价**决定。
**完整性** & **最优性**：均无法保证，特别是当启发函数选的不好的时候。

## A\* Search
**策略**：总是拓展**预计总代价最小**的边境结点，即 后向代价 + 预计前向代价，即从初始结点出发到目标结点的预计总代价最小的结点。
**边境的表示**：通常用**优先队列**实现，优先级由**后向代价 + 预计前向代价**决定。
**完整性** & **最优性**：都可以保证。

## Admissibility
在启发式算法中，启发函数的选取非常重要。

定义：
![[Pasted image 20260217165030.png]]

在 A\* 算法中，为了达成**最优性**，启发函数所需满足的条件被称为**可采性**(Admissibility)。可采性要求启发函数**非负**且**非高估**，即，假设 $\displaystyle h^*(n)$ 为结点 $\displaystyle n$ 到目标状态的最优前向代价，我们有：
$$
∀n,0≤h(n)≤h^∗(n)
$$
此外，所有目标状态的启发值都必须为 0。

简单来讲，这样可以保证最优目标状态的所有祖先结点都比次优目标状态更早被拓展，从而保证保证最优目标状态是最早被拓展的目标状态。

## Graph Search
以上搜索策略都可能重复搜索同一个状态多次，但这造成了多余的计算量，甚至在某些搜索策略下造成无限循环。一个自然的解决方案是，维护一个已访问结点的集合，每次拓展时先确认结点是否不在已访问的集合中。

有了这种附加优化的树搜索，又被称为**图搜索**(Graph Search)。然而在图搜索中，A\*算法无法保证最优性，例如：![[Pasted image 20260217173827.png]]
其中最短路径是 SACG，但算法会优先搜索 SBC，认为 C 的预计总代价为 `3+1` 并将 C 加入已访问集合，但实际上为 `2+1`，此时解非最优。

因此，为了保证 A\*算法在图搜索中的最优性，我们需要允许当存在更短路径时重复访问结点：
```
function A*-GRAPH-SEARCH(problem, frontier) return a solution or failure
    reached ← an empty dict mapping nodes to the cost to each one
    frontier ← INSERT((MAKE-NODE(INITIAL-STATE[problem]),0), frontier)
    while not IS-EMPTY(frontier) do
        node, node.CostToNode ← POP(frontier)
        if problem.IS-GOAL(node.STATE) then return node
        if node.STATE is not in reached or reached[node.STATE] > node.CostToNode then
            reached[node.STATE] = node.CostToNode
            for each child-node in EXPAND(problem, node) do
                frontier ← INSERT((child-node, child-node.COST + CostToNode), frontier)
    return failure
```

## Dominance
**优势度**(Dominace) 是衡量可采启发函数好坏的一个标准。启发函数 a **不劣于**启发函数 b，当且仅当
$$
∀n:h_a(n)≥h_b(n)
$$
直觉来讲，对于可采的启发函数，其值越高，就意味着与真实值越接近，于是搜索的效率就越高。

而**无用启发**(Trivial Heuristic) 被定义为 $\displaystyle h(n)=0$，此时 A\*就退化为了 UCS，是优势度最低的启发函数。

同时，将多个可采的启发函数取最大值函数 $\displaystyle max$，得到的函数依然是可采的，同时总是不劣于组成它的启发函数。因此我们通常会用 $\displaystyle max$ 组合多个启发函数作为最终使用的启发函数。