# 네트워크

출처: [https://programmers.co.kr/learn/courses/30/lessons/43162](https://programmers.co.kr/learn/courses/30/lessons/43162)

### **문제 설명**

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

### 제한사항

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

### 입출력 예

[Untitled](https://www.notion.so/03e4febe3bbb43468943bebb49cc25ca)

### 입출력 예 설명

예제 #1

아래와 같이 2개의 네트워크가 있습니다.

![https://grepp-programmers.s3.amazonaws.com/files/ybm/5b61d6ca97/cc1e7816-b6d7-4649-98e0-e95ea2007fd7.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/5b61d6ca97/cc1e7816-b6d7-4649-98e0-e95ea2007fd7.png)

예제 #2

아래와 같이 1개의 네트워크가 있습니다.

![https://grepp-programmers.s3.amazonaws.com/files/ybm/7554746da2/edb61632-59f4-4799-9154-de9ca98c9e55.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/7554746da2/edb61632-59f4-4799-9154-de9ca98c9e55.png)

이 문제는 dfs로 모든 노드 시작노드로 호출해서 돌면서, 각 노드가 visited에 없으면, 새로운 넷웤으로 보고 1을 리턴해서 answer에 +1해주고, 있으면 0 리턴해서 아무것도 더해주지 않는다. 그리고 visited는 계속 되야 하므로 같이 리턴받고, dfs함수에 파라미터로 계속 넣어준다.

```python
def dfs(graph, root, visited):
    stack = [root]
    if root not in visited:
        while stack:
            node = stack.pop()
            if node not in visited:
                visited.append(node)
                stack.extend(graph[node])
        return 1, visited
    else:
        return 0, visited
        
def solution(n, computers):
    answer = 0
    graph = {}
    # init graph
    for i in range(len(computers)): # 0, 1, 2, 3, 4, 5
        for j in range(len(computers[i])): # 0, 1, 2, 3, 4, 5
            if i != j: # 0, 1, 2, 3, 4, 5
                if i + 1 not in graph:
                    graph[i + 1] = []
                if computers[i][j] == 1:
                    graph[i + 1].append(j + 1)
    # graph {1: [2, 3, 4, 5, 6], 2: [1, 3, 4, 5, 6], 3: [1, 2, 4, 5, 6], 4: [1, 2, 3, 5, 6], 5: [1, 2, 3, 4, 6], 6: [1, 2, 3, 4, 5]}

    visited = []
    for i in graph.keys():
        ans, visited = dfs(graph, i, visited)
        if ans == 1:
            answer += 1
    return answer
```

이건 처음에 푼 문제다.

다 통과했는데 테케 5번만 틀려서 슬펐다.. 질문하기에 찾아보니까 테케 5번은 node가 1개일 때였다. 이런 예외사항을 고려하지 못하다니 예외처리에 민감한 42카뎃이라고 할수가 읎네

```python
def dfs(graph, root, visited):
    stack = [root]
    if root not in visited:
        while stack:
            node = stack.pop()
            if node not in visited:
                visited.append(node)
                stack.extend(graph[node])
        return 1, visited
    else:
        return 0, visited
        
def solution(n, computers):
    answer = 0
    graph = {}
    # only one node
    if n == 1:
        return 1
    # init graph
    for i in range(len(computers)): # 0, 1, 2, 3, 4, 5
        for j in range(len(computers[i])): # 0, 1, 2, 3, 4, 5
            if i != j: # 0, 1, 2, 3, 4, 5
                if i + 1 not in graph:
                    graph[i + 1] = []
                if computers[i][j] == 1:
                    graph[i + 1].append(j + 1)
    # graph {1: [2, 3, 4, 5, 6], 2: [1, 3, 4, 5, 6], 3: [1, 2, 4, 5, 6], 4: [1, 2, 3, 5, 6], 5: [1, 2, 3, 4, 6], 6: [1, 2, 3, 4, 5]}

    visited = []
    for i in graph.keys():
        ans, visited = dfs(graph, i, visited)
        if ans == 1:
            answer += 1
    return answer
```

그래서 solution 함수 처음에 n == 1이면 return 1 해버렸다 ㅋㅋㅋ

그래서 통과!!

level3이라서 좀 걸릴줄 알았는데 금방 풀어서 뿌듯했다. 경험치가 쌓이는 느낌 ㅎㅎ
