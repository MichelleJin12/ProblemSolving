# 단어 변환

출처: [https://programmers.co.kr/learn/courses/30/lessons/43163](https://programmers.co.kr/learn/courses/30/lessons/43163)

### **문제 설명**

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

`1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.`

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

### 입출력 예

[Untitled](https://www.notion.so/04962e70f92d4c948862279a261cc0a6)

### 입출력 예 설명

예제 #1문제에 나온 예와 같습니다.

예제 #2target인 "cog"는 words 안에 없기 때문에 변환할 수 없습니다.

ㅎ.. 이 문제 겁나 이상하다.

일단 공개된 테스트케이스랑 채점테스트케이스 기준이 다르다. 이것 때문에도 겁나 삽질했다.

공개된 테스트케이스는 단어가 변환한 횟수, 채점 테스트케이스는 단어의 수라고 생각하면 된다. 이거 잘못된지 엄청 오래된 거 같은데 플머스는 고치려는 생각이 전혀 없나보다 ㅋㅋㅋㅋㅎ

여튼..

나의 알고리즘은,

먼저 begin, words에 있는 단어에 대한 딕셔너리를 만들고, value에 그 단어가 변화할 수 있는 모든 단어의 리스트를 넣어준다.

그리고 bfs로 돌면서, queue를 업데이트 할때마다 queue를 리스트로 변환해서 count에 이중리스트로 넣는다. (depth를 계산하기 위함)

모든 노드를 다 돌면 count를 돌면서 target이 있는지 찾아보고, 있으면, 그 depth에서 +1 해서 리턴한다.

없으면 0

```python
from collections import deque

def checkWord(begin, target):
    count = 0
    for i in range(len(begin)):
        if begin[i] != target[i]:
            count += 1
        if count > 1:
            return False
    return True

def bfs(graph, root, target):
    visited = []
    count = []
    queue = deque([root])
    
    while queue:
        n = queue.popleft()
        if n not in visited:
            visited.append(n)
            # if n is None, Runtime error
            if n in graph:
                queue.extend(list(set(graph[n]) - set(visited)))
            # count depth
            count.append(list(queue))
    for i in range(len(count)):
        if target in count[i]:
            return i + 1
    return 0

def solution(begin, target, words):
    answer = 0
    if target not in words:
        return 0
    graph = {}
    graph[begin] = []
    #init graph
    for i in words:
        if checkWord(begin, i):
            graph[begin].append(i)
        for j in words:
            if i != j and checkWord(i, j):
                if i not in graph.keys():
                    graph[i] = []
                if j not in graph.keys():
                    graph[j] = []
                graph[i].append(j)
    answer = bfs(graph, begin, target)
    return answer
```

queue extend하는 부분에서 계속 runtime error가 나서 엄청 헤맸다. graph[n]만 접근하면 에러가 나는겨.. 오늘 너무너무 피곤해서 바로 못찾은거같다;; 출근하고 밥먹고 회의하고 알고 하나 풀고 자려고 푼건데 문제도 맛이 갔고 나도 맛이 가버렸다;

이게 왜 에러 났나면, 이 words들이 서로 돌고 돌아서 나중에 차집합하고나면 빈 list가 extend되어서 n 자체가 빈 queue가 되게 된다. 

그러면 pop 해봤자 None이고 그 None을 key로 딕셔너리에 접근하려고 하니까 안되지.. 아....

이거때문에 테스트케이스 4번 계에에에속 런타임 에러가 났다...

오래 삽질했지만 요즘 알고에 재미붙여서 피곤해도 재밌다. 영어 공부도 재미 좀 붙으면 좋겠다.
