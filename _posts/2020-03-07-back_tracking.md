---
layout: post
title: Back Tracking 기법
tags:  [algorithm-and-data-structure]
---
# 백 트래킹(backtracking)이란?

**백 트래킹 문제 (backtracking)** 또는 **퇴각 문제(backtrack)** 이라고 부른다. **제약 조건 만족 문제(Constraint Satisfaction Problem)** 에서 해를 찾기 위한 전략이다.

즉, 해를 찾기 위해 후보군에 제약 조건을 점진적으로 체크하다가 해당 후보군이 **제약 조건을 만족할 수 없다고 판단** 되는 즉시 **backtrack** 하고, 바로 다른 후보군으로 넘어가서 **결국 최적의 해를 찾는 방법** 이다.

실제 구현 시, 고려할 수 있는 모든 **경우의 수(후보군)** 을 **상태공간트리(State Space Tree)** 를 통해 표현한다. 그리고 각 후보군을 **DFS** 를 통해 확인한다.

상태 공간 트리를 탐색하면서 제약에 맞지 않으면 해의 후보가 될만한 곳으로 바로 넘어가서 탐색한다.

* **Promising :** 해당 루트가 조건에 맞는 지를 검사.

* **Pruning :** 조건에 맞지 않으면 포기하고 다른 루트로 돌아가서 **탐색의 시간을 절약하는 방법.**

 > **즉 백트래킹 방법은 트리 구조를 기반으로 DFS로 깊이 탐색을 진행하면서 각 루트에 대해 조건에 부합하는 지 체크 (Promising) 만약 해당 트리(나무)에서 조건에 맞지 않는 노드는 더 이상 DFS로 깊이 탐색하지 않고, 가치를 쳐버린다. (Pruning)**

&nbsp;
&nbsp;

### 상태공간트리
문제 해결 과정의 중간 상태를 각각의 노드로 나타낸 트리이다.

각 노드는 하나의 경우의 수를 의미한다.

![Alt text](/public/post/2020_03_06_backtrack/pic1.PNG)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# N Queen 문제 이해
대표적인 backtrack 문제이다. **NxN 크기의 체스판에 N개의 퀀을 서로 공격할 수 없게 배치하는 문제이다.**

퀀은 다음과 같이 이동할 수 있고 각 퀸을 서로 이동할 수 없는 공간에 각 행과 열마다 배치해야 한다.

![Alt text](/public/post/2020_03_06_backtrack/pic2.PNG)

&nbsp;
&nbsp;

### N Queen문제의 Pruning(가지치기)

**한 행에는 하나의 퀸 밖에 오지 못한다.** (퀸은 수평 이동이 가능하다.)

**맨 위 행부터 퀀을 하나씩 배치하면서 다음 행에는 퀸이 이동할 수 없는 위치를 찾아서 배치해야 한다.**

**만약 앞선 행에 배치한 퀸 때문에 다음 행에 퀸을 배치할 공간이 없다면 더 이상 퀸을 배치하지 않고 이전 행의 퀸의 배치를 바꾼다. (Pruning)**

> **즉, 맨 위의 행부터 전체 행까지 퀸의 배치가 가능한 경우의 수를 상태 공간 트리 형태로 만든 후, 각 경우를 맨 위의 행부터 DFS 방식으로 접근, 해당 경우가 진행이 어려울 경우, 더 이상 진행하지 않고, 다른 경우를 체크하는 방식이다.**

![Alt text](/public/post/2020_03_06_backtrack/pic3.PNG)

&nbsp;
&nbsp;

### N Queen문제의 Promising

**해당 루트가 조건에 맞는 지 검사해야 한다.**

**수직 체크와 수평 체크를 한다.**

![Alt text](/public/post/2020_03_06_backtrack/pic4.PNG)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 파이썬 코드

~~~python
def is_available(candidate, current_col):
    current_row = len(candidate)
    for queen_row in range(current_row):
        if candidate[queen_row] == current_col or abs(candidate[queen_row] - current_col) == current_row - queen_row:
            return False
    return True


def DFS(N, current_row, current_candidate, final_result):
    if current_row == N:
        final_result.append(current_candidate[:])
        return

    for candidate_col in range(N):
        if is_available(current_candidate, candidate_col):
            current_candidate.append(candidate_col)
            DFS(N, current_row + 1, current_candidate, final_result)
            current_candidate.pop()

def solve_n_queens(N):

    final_result = []
    DFS(N, 0, [], final_result)

    return final_result


solve_n_queens(4)
~~~

~~~
[[1, 3, 0, 2], [2, 0, 3, 1]]
~~~
