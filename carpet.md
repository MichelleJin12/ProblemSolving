# 카펫
---
출처:https://programmers.co.kr/learn/courses/30/lessons/42842

### **문제 설명**

Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

![https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b1ebb809-f333-4df2-bc81-02682900dc2d/carpet.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b1ebb809-f333-4df2-bc81-02682900dc2d/carpet.png)

Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

### 입출력 예

[Untitled](https://www.notion.so/5f8d70ecd0f742d59e8201792ae866c8)

[출처](http://hsin.hr/coci/archive/2010_2011/contest4_tasks.pdf)

※ 공지 - 2020년 2월 3일 테스트케이스가 추가되었습니다.※ 공지 - 2020년 5월 11일 웹접근성을 고려하여 빨간색을 노란색으로 수정하였습니다.

이 문제의 풀이 방법을 생각해봤다.

먼저 브라운 + 옐로 하면 전체 사각형의 갯수가 나온다.

사각형의 갯수는 가로 * 세로이기 때문에, 전체 사각형 갯수의 공약수를 찾는다.

만약에 브라운 10 옐로 2 여서 12가 되고,

2 * 6 / 3 * 4 라고 나온다 치면, 가로의 길이는 세로보다 길거나 같아야 하니까 가로가 6 or 4가 될 것이다.

여기서 브라운, 옐로의 갯수를 구해보면,

브라운 = 가로의 길이 * 2 + (세로의 길이 - 2{브라운이 먹은것}) *2 일 것이다.

옐로는 전체 사각형 - 브라운이겠지뭐

2 * 6 케이스 : 6 * 2 (x)

3 * 4 케이스: 4 * 2 + (3 - 2) *2 = 10이 될 것이다. 그러므로 답은 4, 3이 되는 것이다.

이 절차대로 그대로 풀면, 2부터 시작해서 나누어 떨어졌을 때, 브라운 갯수를 구하고 맞는 수가 나오면 stop.

```python
def solution(brown, yellow):
    rectangle = brown + yellow
    for length in range(2, rectangle // 2 + 1):
        width = rectangle // length
        if rectangle % length == 0 and width * 2 + (length - 2) * 2 == brown and width >= length:
            return [width, length]
    else:
        return None
```

좀 수학적인 문제 같은데 뇌를 거치지 않고 풀었다.

시간 단축하는 방법은, length를 1씩 증가하지말고, 짝수홀수 판별하고 2, 3부터 +2씩 증가하게 하면 어떨까 생각해봤다.

또 range 범위를 sqrt로 해도 되겠다. 이거는 수학적으로 증명된 거긴한데 내 뇌가 인정하길 거부하기 때문에 나는 그냥 반절로 나누는 편이다.. 더 수련을 해서 뇌가 받아들이도록 해야겠다.

그리고 if문에 조건을 다 때려넣어서 너무 길다. 근데 저 조건이 다 들어가야하는데 우째..

다른 사람들의 풀이를 보니까 신기하넹
