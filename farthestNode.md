# 가장 먼 노드

출처: [https://programmers.co.kr/learn/courses/30/lessons/49189](https://programmers.co.kr/learn/courses/30/lessons/49189)

### **문제 설명**

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

### 입출력 예

[Untitled](https://www.notion.so/39227e1249bd4fbeb0d51269f853b30f)

### 입출력 예 설명

예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

![https://grepp-programmers.s3.amazonaws.com/files/ybm/fadbae38bb/dec85ab5-0273-47b3-ba73-fc0b5f6be28a.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/fadbae38bb/dec85ab5-0273-47b3-ba73-fc0b5f6be28a.png)

이 문제는..ㅋㅋㅋ어이없다..ㅋㅋㅋㅋ 질문하기에 7, 8, 9번 터진다고 아우성밖에 없음 ㅋㅋㅋ

쉬운 문제인줄 알았는데 나도 터져버렸다.. 울화통 터지기 전에 풀긴했다.

내 알고리즘은, 

딕셔너리 key를 노드 번호, 노드에 붙어있는 다른 노드를 value list로 하는 기본 graph가 있고,

bfs로 그래프 돌면서, 1에 붙어있는 애들의 pathCount 자리는 1로 해주고,

그 다음 노드부터 현재 해당되는 노드에 붙어있는 노드의 pathCount를 현재 해당되는 노드의 pathCount + 1을 해주는 방식이다. 

그리고 pathCount에서 max값을 찾고, max값의 수를 리턴해주면 되겠다.

```python
from collections import deque

def bfs(pathCount, root, vertex):
    visited = []
    queue = deque([root])

    while queue:
        n = queue.popleft()
        if n not in visited:
            visited.append(n)
						# 
            queue.extend(list(set(vertex[n]) - set(visited)))
            for i in queue:
                if pathCount[i] == 0:
                    pathCount[i] = pathCount[n] + 1
				# all count
        if pathCount.count(0) == 2:
            return pathCount.count(max(pathCount))

def solution(n, edge):
    answer = 0
    vertex = {}
    pathCount = [0] * (n + 1)
    # init dic
    for i in range(1, n + 1):
        vertex[i] = []
    # insert dic
    for i in range(len(edge)):
        vertex[edge[i][0]].append(edge[i][1])
        vertex[edge[i][1]].append(edge[i][0])
		# find answer
    answer = bfs(pathCount, 1, vertex)
    return answer
```

첫번째로 푼 코드다.

pathCount는 각각의 노드가 1에서 얼마나 떨어져있는지 기록하는 리스트다.

bfs에서 queue를 돌면서, pathCount값이 0일 경우에(현재 탐색하고 있는 노드에 가지로 달려있는데 아직 count가 안됐을 경우에) 현재 탐색하고 있는 노드 count + 1 해서 저장해준다.

그리고 pathCount에 0이 2개라는 뜻은, 0일 수밖에 없는 0, 1 밖에 안남고 모두 count되었다는 뜻이기 때문에 그때는 바로 max값의 수를 세주고 리턴해버린다.

근데 이거는 7, 8, 9 터져버림ㅋ

[코드 테스트1](https://www.notion.so/43eb483938f04725a87977dea0f582f3)

플머스는 짜증나는게 테케를 안보여준다.. 왜...짜증나 ㅋㅋ

```python
from collections import deque

def bfs(pathCount, root, vertex):
    visited = set()
    queue = deque([root])

    while queue:
        n = queue.popleft()
        if n not in visited:
            visited.add(n)
            queue.extend(list(set(vertex[n]) - visited))
            
            for i in queue:
                if pathCount[i] == 0:
                    pathCount[i] = pathCount[n] + 1
            if pathCount.count(0) == 2:
                return pathCount.count(max(pathCount))

def solution(n, edge):
    answer = 0
    vertex = {}
    pathCount = [0] * (n + 1)
    # init dic
    for i in range(len(edge)):
        if edge[i][0] not in vertex:
            vertex[edge[i][0]] = []
        if edge[i][1] not in vertex:
            vertex[edge[i][1]]= []
        vertex[edge[i][0]].append(edge[i][1])
        vertex[edge[i][1]].append(edge[i][0])
    answer = bfs(pathCount, 1, vertex)
    return answer
```

그래서 최대한 줄이기 위해, init dic 부분을 합쳐버렸다.

그리고, visited를 set으로 바꿔버렸다. set으로 변환하고 차집합하고 리스트로 바꾸는 부분을 최대한 줄이기 위해..

이렇게 바꾸니까 7, 8 까지는 통과했다. 9번은 뭐지? 근데 7, 8도 미친듯한 시간이 걸렸다.

[코드 테스트2](https://www.notion.so/6a45b7bb9f5a4b3bae2a49bec7edb743)

```python
from collections import deque

def bfs(pathCount, root, vertex):
    visited = set()
    queue = deque([root])

    while queue:
        n = queue.popleft()
        if n not in visited:
            visited.add(n)
            for i in vertex[n]:
                if pathCount[i] == 0 and i not in visited:
                    pathCount[i] = pathCount[n] + 1
            if pathCount.count(0) == 2:
                return pathCount.count(max(pathCount))
            queue.extend(list(set(vertex[n]) - visited))

def solution(n, edge):
    answer = 0
    vertex = {}
    pathCount = [0] * (n + 1)
    # init dic
    for i in range(len(edge)):
        if edge[i][0] not in vertex:
            vertex[edge[i][0]] = []
        if edge[i][1] not in vertex:
            vertex[edge[i][1]]= []
        vertex[edge[i][0]].append(edge[i][1])
        vertex[edge[i][1]].append(edge[i][0])
    answer = bfs(pathCount, 1, vertex)
    return answer
```

결국엔 bfs 부분에서, queue를 extend하는 부분과 pathCount를 세주는 부분을 자리 바꿔버렸다.

내가 생각한 알고리즘은, queue를 굳이 구할 필요도 없고, pathCount가 다 차면 바로 끝내버려도 되기 때문에,

자리를 바꿔버리고, queue에서 pathCount를 도는게 아닌 vertex[n]에서 돌게 했다.

그리고 i가 visited에도 없어야 한다는 조건도 추가해야 한다. 왜냐하면 set으로 중복되는 노드를 없애버리지 않았기 때문에..

그랬더니.. 통과했다..

`queue.extend(list(set(vertex[n]) - visited))` 이 부분이 그렇게 오래걸리는지 몰랐다 ..흠..;;

파이썬은 한 줄 안에 여러 개를 때려 넣을 수 있어서 편한데 알고리즘 시간 계산하기가 좀 힘들다;

[코드 테스트3](https://www.notion.so/feea7470924d48238ec2bc557a166daf)
