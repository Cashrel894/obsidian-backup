#CS188
**游戏**(Game)，又可称为**对抗搜索问题**(Adversial Search Problems)，是人工智能中的经典问题之一。在这种问题中，我们的代理有一个或多个**对手**(Adversary) 试图阻碍它达成目标。

## Deterministic Zero-sum Games
**确定零和游戏**(Deterministic Zero-sum Games)，是指行动确定（即状态转移不具有概率性），且己方的得分直接等于对手的损失值，反之亦然。许多常见的游戏都属于这类：
- 跳棋
- 国际象棋
- 围棋
- ……

## Formulation of Games
一个标准游戏的公式化定义包含以下元素：
- **初始状态**： $\displaystyle s_{0}$
- **玩家**：$\displaystyle Players(s)$ 表示状态 $\displaystyle s$ 属于哪个玩家的回合
- **行动**：$\displaystyle Actions(s)$ 表示当前玩家的合法行动
- **转移模型**：$\displaystyle Result(s,a)$
- **终止条件**：$\displaystyle Terminal-test(s)$
- **终止值**：$\displaystyle Utility(s,player)$
