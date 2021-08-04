# 타겟 넘버

출처: [https://programmers.co.kr/learn/courses/30/lessons/43165](https://programmers.co.kr/learn/courses/30/lessons/43165)

### **문제 설명**

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

- `1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3`

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

### 입출력 예

[Untitled](https://www.notion.so/a068cca606e44da39c54ca80a0a5d1c1)

### 입출력 예 설명

문제에 나온 예와 같습니다.

이 문제는 numbers의 수를 한번은 더하고, 한번은 빼고, 가지쳐서 마지막까지 더하고 빼서 답을 찾는 문제이다.

여러가지 방법으로 풀 수 있겠지만 dfs 항목에 있어서 걍 dfs로 풀었다.. 굳이 따지면 dfs가 아닌가 싶은데 걍 함수이름이 dfs다.

```python
def dfs(numbers, target, idx, num, answer):
    if idx >= len(numbers):
        if num == target:
            return 1
        return 0
    answer = dfs(numbers, target, idx + 1, num + numbers[idx], answer)
    answer += dfs(numbers, target, idx + 1, num - numbers[idx], answer)
    return answer

def solution(numbers, target):
    answer = 0
    answer = dfs(numbers, target, 0, 0, 0)
    return answer
```

dfs 메소드는 

numbers 리스트, 

다 연산했을 때 나올 target 넘버, 

numbers 몇까지 더했는지 알아야할 idx, 

더하고 빼고 나온 결과의 num, 

return할 answer를 파라미터로 받는다.

일단 쁠러스로 쭈우우욱 갔다가 막판에 target과 같으면 1을 리턴받고, 마이너스로 갔을 때도 1이 나오면 + 해주는 식이다.

파라미터로 다 때려넣어서 살짝 애매하기 때문에 더 좋은 방법을 생각해봐야겠다.
