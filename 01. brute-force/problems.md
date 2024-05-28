## 1018: 체스판 다시 칠하기

> https://www.acmicpc.net/problem/1018

이 문제를 완전 탐색으로 풀 때, 최악의 반복 횟수를 계산해보자.

보드의 최대 크기는 50 X 50이 될 것이다.

이 보드에서 잘라낼 수 있는 가능한 모든 8 x 8 체스칸은 대략 O(50 x 50) = O(2500)개가 있다.

잘라낸 각 체스칸을 검사하기 위해서는 O(8 x 8)의 반복문이 필요하다.

결국 모든 경우를 완전탐색으로 다 찾아보아도 O(2500 x 8 x 8) = O(160,000)으로 1초도 걸리지 않는다.

이 문제를 브루트 포스가 아닌 다른 방식으로 푼다고 생각해 보면 매우 복잡해 질 것이다.

```cpp
#include <cstdio>
#include <algorithm>
using namespace std;

int N, M;
char board[50][51];

int check(int y, int x) {
    int count = 0;
    for(int i = 0; i < 8; i++)
        for(int j = 0; j < 8; j++) {
            if((i + j) % 2 == 0 && board[y + i][x + j] == 'B')
                count++;
            else if((i + j) % 2 == 1 && board[y + i][x + j] == 'W')
                count++;
        }
    return count;
}

int main() {
    scanf("%d %d", &N, &M);
    int ret = 987654321;
    for(int i = 0; i < N; i++)
        scanf("%s", board[i]);
    for(int y = 0; y + 8 <= N; y++)
        for(int x = 0; x + 8 <= M; x++) {
            int wb = check(y, x);
            int bw = 64 - wb;
            ret = min(ret, min(wb, bw));
        }
    printf("%d", ret);
}
```