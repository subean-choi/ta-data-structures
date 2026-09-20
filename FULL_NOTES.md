# TA session

- Sort 알고리즘
- MaxHeap TA Session
- 최단 경로 찾기
- 미로 만들기 과제

---

# Sort 알고리즘

---
## 목차
---
## Sort 종류
### 1. Selection Sort (선택 정렬)
[image omitted: personal or temporary Notion asset]
```c
void selection_sort(int list[], int n){
  int i, j, least, temp;

  // 마지막 숫자는 자동으로 정렬되기 때문에 (숫자 개수-1) 만큼 반복한다.
  for(i=0; i<n-1; i++){
    least = i;

    // 최솟값을 탐색한다.
    for(j=i+1; j<n; j++){
      if(list[j]<list[least])
        least = j;
    }

    // 최솟값이 자기 자신이면 자료 이동을 하지 않는다.
    if(i != least){
        SWAP(list[i], list[least], temp);
    }
  }
}
```
1 2 3 4 5
### 2. Insertion Sort (삽입 정렬)
[image omitted: personal or temporary Notion asset]
```c
void insertion_sort(int list[], int n){
  int i, j, key;

  for(i=1; i<n; i++){
    key = list[i]; // 현재 삽입될 숫자인 i번째 정수를 key 변수로 복사

    // 현재 정렬된 배열은 i-1까지이므로 i-1번째부터 역순으로 조사한다.
    // j 값은 음수가 아니어야 되고
    // key 값보다 정렬된 배열에 있는 값이 크면 j번째를 j+1번째로 이동
    for(j=i-1; j>=0 && list[j]>key; j--){
      list[j+1] = list[j]; // 레코드의 오른쪽으로 이동
    }

    list[j+1] = key;
  }
}
```
5 4 3 2 1
### 3. Bubble Sort
[image omitted: personal or temporary Notion asset]
```c
void bubble_sort(int list[], int n){
  int i, j, temp;

  for(i=n-1; i>0; i--){
    // 0 ~ (i-1)까지 반복
    for(j=0; j<i; j++){
      // j번째와 j+1번째의 요소가 크기 순이 아니면 교환
      if(list[j]<list[j+1]){
        temp = list[j];
        list[j] = list[j+1];
        list[j+1] = temp;
      }
    }
  }
}
```
1 2 3 4 5
### Sort 시간 복잡도 :
<table header-row="true" header-column="true">

<tr>
<td>알고리즘</td>
<td>Best Case</td>
<td>Average Case</td>
<td>Worst Case</td>
</tr>
<tr>
<td>Insertion Sort</td>
<td>O(n)</td>
<td>O(n²)</td>
<td>O(n²)</td>
</tr>
<tr>
<td>Selection Sort</td>
<td>O(n²)</td>
<td>O(n²)</td>
<td>O(n²)</td>
</tr>
<tr>
<td>Bubble Sort</td>
<td>O(n²)</td>
<td>O(n²)</td>
<td>O(n²)</td>
</tr>
</table>
## 과제 Hint 
### Function :
1. **Sort 함수**
- selection 
- insertion
- bubble
1. **데이터 생성 함수**
- 정렬 상태에 따른 여러개의 배열 생성하는 함수
1. **수행 시간 측정 함수**
**❓ 왜 index가 100만개인 배열은 전역변수에 지정해야 하는가?**
[image omitted: personal or temporary Notion asset]

---

# MaxHeap TA Session

---
## 목차
---
## Heap
= 완전 이진 트리 (Complete Binary Tree)
= 즉, 마지막 레벨을 제외한 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 노드들은 왼쪽부터 순서대로
### Heap의 구현 : Array
🌟 루트 노드의 인덱스 = 1
- 부모 노드의 인덱스가 i일 때,
- 왼쪽 자식 노드의 인덱스 : 2 \* i
- 오른쪽 자식 노드의 인덱스 : 2 \* i + 1
- 자식 노드의 인덱스가 i일 때,
- 부모 노드의 인덱스 : i / 2
```c
     (A)
     루트
    /    \
  (B)    (C)
 /  \    /  \
(D)(E) (F)  (G)
```
위 트리를 배열로 :
<table header-column="true">

<tr>
<td>인덱스</td>
<td>1</td>
<td>2</td>
<td>3</td>
<td>4</td>
<td>5</td>
<td>6</td>
<td>7</td>
</tr>
<tr>
<td>값</td>
<td>A</td>
<td>B</td>
<td>C</td>
<td>D</td>
<td>E</td>
<td>F</td>
<td>G</td>
</tr>
</table>
- 루트 노드 : 1번 (A)
- A의 자식 노드 : 2번 (B), 3번 (C)
- B의 자식 노드 : 4번 (D), 5번 (E)
- C의 자식 노드 : 6번 (F), 7번 (G)
### Heap 구현에 필요한 함수 :
1. **삽입 연산**
- 완전 이진 트리 모양을 유지하기 위해 input을 맨 끝에 위치
- 크기에 따라 이동
- 삽입 후에 새로운 노드를 부모 노드들과 교환하며 힙의 성질 만족
- 즉, 새로 생긴 값은 자기 부모와 비교
2. **삭제 연산**
- 루트를 삭제
- 모양을 유지하기 위해서 맨 끝값을 루트로 올림
- 값에 따라서 위치 변경
### Heap 종류 :
1. **MaxHeap**
= 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
🌟 부모 노드보다 무조건 자식 노드가 작아야 함 (= root 값이 가장 커야 함)
↔ bst와 다른 점 : bst는 root의 왼쪽이 작은 수, 오른쪽이 더 큰 수
2. **MinHeap**
= 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리
---
## MaxHeap


## code

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define HEAP_SZ 100

int heap[HEAP_SZ + 1];
int idx = 0;

void addToMaxHeap(int _v)
{
if (idx == HEAP_SZ) {
printf("heap full\n");
return;
}

idx = idx + 1;
heap[idx] = _v;

//upheap 과정
int _cur = idx;
while (_cur > 1) {
int _p = _cur / 2; //부모 인덱스
if (heap[_p] >= heap[_cur]) {
return; //부모가 더 크기에 할 일이 없음
}
else {
int temp = heap[_p];
heap[_p] = heap[_cur];
heap[_cur] = temp;
_cur = _p; //내가 부모의 위치로 upheap 가장 중요
}
}
}


//max heap이 비어있으면 -999 반환
int delFromMaxHeap() {
if (idx == 0) {
return -999;
}

int res = heap[1]; //heap에서 꺼내는 것은 항상 인덱스 1번
//root가 나감으로써, 붕괴된 maxheap을 재건하는 과정
//downheap과정

//맨 마지막 것을 root위치로 옮기고 
// ids를 감소

heap[1] = heap[idx];
idx = idx - 1;

int _cur = 1;
while (1) {
int child_idx = 2 * _cur;//나의 왼쪽 자식 인덱스
if (child_idx > idx) {
break;
}

if ((child_idx <= idx - 1) && heap[child_idx] < heap[child_idx + 1]) {
child_idx = child_idx + 1;
}

if (heap[_cur] >= heap[child_idx]) {
break;
}
else {//자식 자리로 내려가야 한다
int temp = heap[_cur];
heap[_cur] = heap[child_idx];
heap[child_idx] = temp;
_cur = child_idx;
}
}

return res;
}


int main()
{

addToMaxHeap(20);
addToMaxHeap(30);
addToMaxHeap(5);
addToMaxHeap(40);
addToMaxHeap(15);

//addtomaxheap이 제대로 구현되었는지 확인 위해서
for (int i = 1; i <= idx; i++) {
printf("%d %d\n", i, heap[i]);
}

while (1) {
int res = delFromMaxHeap();
if (res == -999) {
break;
}
printf("%d\n", res);
}

return 0;
}
```

[image omitted: personal or temporary Notion asset]
### MaxHeap의 삽입 & 삭제
**\< 삽입 \>**
---
1. Heap의 마지막에 새로운 노드 추가
2. 추가한 노드와 부모 노드 비교
- 추가한 노드가 부모보다 크면 swap하고 부모로 이동
- 그렇지 않다면 종료
3. 루트까지 반복 or 부모가 더 클 때까지 반복
---
[image omitted: personal or temporary Notion asset]
**\< 삭제 \>**
---
1. root 노드 삭제
2. Heap의 마지막 노드 → root 노드 위치로 옮김
3. 새 루트 노드와 자식 노드들을 비교
- 자식 노드 중 가장 큰 값과 교환
- 새 루트가 더 크거나 자식이 없으면 종료
4. Heap 성질을 만족할 때 까지 반복
---
[image omitted: personal or temporary Notion asset]
---
## Project : MaxHeap 
### Non-use MaxHeap


## code

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

#define MAX_WORD_LEN 100
#define MAX_UNIQUE   10000
#define K            5

typedef struct {
    char word[MAX_WORD_LEN];
    int  count;
} WordCount;

WordCount wc[MAX_UNIQUE];
int unique = 0;

// 문자열 비교: a < b → -1, a == b → 0, a > b → 1
int my_strcmp(const char* a, const char* b) {
    int i = 0;
    while (a[i] && b[i]) {
        if (a[i] != b[i])
            return (a[i] < b[i]) ? -1 : 1;
        i++;
    }
    if (!a[i] && !b[i]) return 0;
    return (!a[i]) ? -1 : 1;
}

// 문자열 복사
void my_strcpy(char* dest, const char* src) {
    int i = 0;
    while (src[i]) {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

int main(void) {
    int t;
    printf("t값 입력 : ");

    FILE* file = fopen("word_data.txt", "r");
    if (!file) {
        perror("Error opening file");
        return 1;
    }

    char buf[MAX_WORD_LEN];
    int timestamp;

    unique = 0;
    while (fscanf(file, "%s %d", buf, &timestamp) == 2) {
        if (timestamp > t) continue;
        int i;
        for (i = 0; i < unique; ++i) {
            if (my_strcmp(wc[i].word, buf) == 0) {
                wc[i].count++;
                break;
            }
        }
        if (i == unique && unique < MAX_UNIQUE) {
            my_strcpy(wc[unique].word, buf);
            wc[unique].count = 1;
            unique++;
        }
    }
    fclose(file);

    if (unique == 0) {
        printf("No words appeared by time %d.\n", t);
        return 0;
    }

    // 상위 K개 선택
    int printed = 0;
    for (int i = 0; i < unique && printed < K; ++i) {
        // i 위치에 올 최댓값 인덱스 찾기
        int maxIdx = i;
        for (int j = i + 1; j < unique; ++j) {
            if (wc[j].count > wc[maxIdx].count ||
                (wc[j].count == wc[maxIdx].count && my_strcmp(wc[j].word, wc[maxIdx].word) < 0)) {
                maxIdx = j;
            }
        }
        // i와 maxIdx를 스왑
        WordCount tmp = wc[i];
        wc[i] = wc[maxIdx];
        wc[maxIdx] = tmp;

        printf("%s, %d\n", wc[i].word, wc[i].count);
        printed++;
    }

    if (printed < K) {
        printf("Fewer than %d words appeared by time %d.\n", K, t);
    }

    return 0;
}

```

**\< 동작 과정 \>**
---
1. 파일에서 timestamp 이하의 단어들을 읽으며 빈도수를 세고 고유한 단어의 배열 make
2. 가장 빈도수가 많은 단어를 찾을 때마다 전체 단어 리스트를 매번 순회해서 최댓값을 찾고 앞으로 이동.
3. 최댓값을 찾기 위해 K번 동안 반복적으로 배열 전체 탐색
---
**\< 시간 복잡도 \>**
---
- 고유한 단어가 총 **N개** 있다고 가정하면,
- 1번째 최대값 찾기: **O(N)**
- 2번째 최대값 찾기: **O(N-1)** ≈ **O(N)**
- 3번째 최대값 찾기: **O(N-2)** ≈ **O(N)**
- ...
- K번째 최대값 찾기: **O(N-K+1)** ≈ **O(N)** (K가 작을 때)
- 전체 시간복잡도는 **O(K × N)**
---
✅ 즉,  이 방법은 **최댓값을 찾기 위해 매번 모든 데이터를 탐색 ⇒ ****데이터가 많을수록 급격히 느려짐**
### Use MaxHeap


## code

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

#define MAX_WORD_LEN 100
#define MAX_UNIQUE   10000
#define K            5

typedef struct {
    char word[MAX_WORD_LEN];
    int  count;
} WordCount;

WordCount wc[MAX_UNIQUE];
int unique = 0;

// --- max‐heap 정의 ---
WordCount heap_arr[MAX_UNIQUE + 1];
int heapSize = 0;

// 문자열 비교: a < b → -1, a == b → 0, a > b → 1
int my_strcmp(const char* a, const char* b) {
    int i = 0;
    while (a[i] && b[i]) {
        if (a[i] != b[i])
            return (a[i] < b[i]) ? -1 : 1;
        i++;
    }
    if (!a[i] && !b[i]) return 0;
    return (!a[i]) ? -1 : 1;
}

// 문자열 복사
void my_strcpy(char* dest, const char* src) {
    int i = 0;
    while (src[i]) {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

// 두 WordCount 중 a가 더 “크다”(count 크거나, count 같으면 word 사전순 앞)
int cmpGreater(const WordCount* a, const WordCount* b) {
    if (a->count != b->count)
        return a->count > b->count;
    return my_strcmp(a->word, b->word) < 0;
}

// 힙에 삽입 (up-heap)
void addToMaxHeap(const WordCount* w) {
    int cur = ++heapSize;
    heap_arr[cur] = *w;
    while (cur > 1) {
        int p = cur / 2;
        if (cmpGreater(&heap_arr[cur], &heap_arr[p])) {
            WordCount tmp = heap_arr[p];
            heap_arr[p] = heap_arr[cur];
            heap_arr[cur] = tmp;
            cur = p;
        }
        else break;
    }
}

// 루트 꺼내기 (down-heap), 비어있으면 count=-999 리턴
WordCount delFromMaxHeap() {
    WordCount empty = { .word = "", .count = -999 };
    if (heapSize == 0) return empty;

    WordCount top = heap_arr[1];
    heap_arr[1] = heap_arr[heapSize--];
    int cur = 1;
    while (1) {
        int l = cur * 2, r = cur * 2 + 1, child = cur;
        if (l <= heapSize && cmpGreater(&heap_arr[l], &heap_arr[child]))
            child = l;
        if (r <= heapSize && cmpGreater(&heap_arr[r], &heap_arr[child]))
            child = r;
        if (child == cur) break;
        WordCount tmp = heap_arr[cur];
        heap_arr[cur] = heap_arr[child];
        heap_arr[child] = tmp;
        cur = child;
    }
    return top;
}
// --- heap 정의 끝 ---

int main(void) {
    int t;
    printf("t값 입력 : ");
    if (scanf("%d", &t) != 1) {
        fprintf(stderr, "Invalid input\n");
        return 1;
    }

    FILE* file = fopen("word_data.txt", "r");
    if (!file) {
        perror("Error opening file");
        return 1;
    }

    char word_buf[MAX_WORD_LEN];
    int timestamp;
    unique = 0;

    while (fscanf(file, "%s %d", word_buf, &timestamp) == 2) {
        if (timestamp > t) continue;
        int i;
        for (i = 0; i < unique; ++i) {
            if (my_strcmp(wc[i].word, word_buf) == 0) {
                wc[i].count++;
                break;
            }
        }
        if (i == unique && unique < MAX_UNIQUE) {
            my_strcpy(wc[unique].word, word_buf);
            wc[unique].count = 1;
            unique++;
        }
    }
    fclose(file);

    if (unique == 0) return 0;

    // max-heap에 모두 삽입
    for (int i = 0; i < unique; ++i) {
        addToMaxHeap(&wc[i]);
    }

    // 상위 K개 출력
    int printed = 0;
    for (; printed < K; ++printed) {
        WordCount top = delFromMaxHeap();
        if (top.count == -999) break;
        printf("%s, %d\n", top.word, top.count);
    }

    // K개 미만이면 영어 메시지
    if (printed < K) {
        printf("Fewer than %d words appeared by time %d.\n", K, t);
    }

    return 0;
}

```

**\< 동작 과정 \>**
---
1. 파일에서 단어를 읽으며 빈도수를 센 뒤 고유 단어 배열 make   → 이 과정까지는 위와 같음
2. 고유한 단어의 빈도수를 모두 MaxHeap에 삽입
3. 힙에서 가장 큰 원소 (= root 원소)를 삭제하면서 최대값을 얻을 수 있음
---
= 즉, Heap 특성상 가장 큰 값으 찾기와 삭제가 빠르게 진행 
**\< 시간 복잡도 \>**
---
- 고유한 단어 수가 N개일 때,
- 힙에 N개의 원소를 삽입 : O(N log N) 
- why? : 힙에 원소 하나 삽입이 O(log N)이기 때문
- 힙에서 최댓값을 K번 꺼내기 : O( K log N)
- why? : 힙에 원소 하나 삭제가 O(log N)이기 때문
- 즉, 전체 시간 복잡도 : O(N log N + K log N) ≈ **O(N log N)  → K ≤ N일 경우**
---
###  근데 왜 MaxHeap을 사용하지?
**데이터 개수(n) = 10,000, 찾으려는 단어 개수(K) = 5라고 할 때 :**
- Max-Heap을 안 쓰면:
- 데이터 저장: O(10,000)
- 상위 5개 찾기: O(5 × 10,000) = O(50,000)
- 총: **O(60,000)** ≈ **O(n × K)**
- Max-Heap을 쓰면:
- 데이터 저장 + 힙 구성: O(10,000 log 10,000) ≈ **O(10,000 × 13)** = O(130,000)
- 상위 5개 추출: O(5 log 10,000) ≈ 매우 작음(무시 가능)
- 총: **O(130,000)** ≈ **O(n log n)**
<table header-row="true">

<tr>
<td>데이터 수(n)</td>
<td>찾을 단어 수(K)</td>
<td>Max-Heap 안 쓸 때</td>
<td>Max-Heap 쓸 때</td>
</tr>
<tr>
<td>10,000</td>
<td>5</td>
<td>60,000 (빠름)</td>
<td>130,000 (느림)</td>
</tr>
<tr>
<td>10,000</td>
<td>500</td>
<td>5,010,000 (느림)</td>
<td>130,000 (매우 빠름)</td>
</tr>
<tr>
<td>100,000</td>
<td>10</td>
<td>1,000,000 (느림)</td>
<td>1,700,000 (비슷)</td>
</tr>
<tr>
<td>100,000</td>
<td>1000</td>
<td>100,100,000 (매우 느림)</td>
<td>1,700,000 (매우 빠름)</td>
</tr>
</table>
→ 즉, 상위 K개를 찾는 K값이 커지면 커질수록 Max-Heap 방식이 압도적으로 빠름
### 💡 Hint : MaxHeap Project
---
1. 파일에서  `fscanf(file, "%s %d", word_buf, &timestamp)`  이런식으로 단어와 타임 스탬프 read
- timestamp와 input t를 비교
1. 단어별 빈도수 세기
- 배열에 단어를 저장하고 단어의 개수도 저장
- 즉, 새로운 데이터타입 (= 구조체)를 만들고 해당 구조체에는 단어와 빈도수가 변수가 있어야 함.
- 이미 저장된 단어가 있는지  =  똑같은 단어가 있는지 ⇒  단어가 같은지 비교하는 함수 
- 단어가 같은지 비교하는 함수는 추가적으로 단어의 오름차순도 계산할 수 있음 
- 저장된 단어가 없다면 단어를 저장 ⇒ 단어를 복사할 수 있는 함수
2. MaxHeap 구성
- MaxHeap을 구성할 때 필요한 함수
- heap 배열 생성
- addToMaxHeap 
- delFromMaxHeap
⇒ 총 5개를 출력 = 즉, root를 5번 없애기

---

# 최단 경로 찾기

---
## 목차
---
## Dijkstra
= 가중치가 0 이상인 그래프에서 하나의 시작 정점 →  모든 정점까지의 최단 거리를 구하는 알고리즘
= **greedy(탐욕적) 알고리즘**
→ 매 단계마다 ‘지금 당장 가장 좋아 보이는 선택’을 골라서 전체 문제의 해답을 만들어 가는 기법.
[image omitted: personal or temporary Notion asset]
**ㄴ[ **✅ **전제 조건 ]**
- 간선 가중치는 반드시 음수 X
- 그래프는 방향 & 무방향 모두 가능
- 연결되지 않은 정점은 도달 불가 상태(무한대 거리)
**[ 🌊 flow ] **
1. 시작 정점의 거리를 0, 나머지는 무한대로 초기화
2. 미확정 정점 중 최단 거리를 가진 정점을 선택해 방문(확정)
3. 그 정점의 모든 인접 정점에 대해
- 새 거리 = 현재 거리 + 간선 가중치
- 새 거리가 이전 기록보다 작으면 거리와 직전 노드(prev) 갱신
4. 모든 정점이 확정될 때까지 (2\~3)을 반복
## 과제 Hint
### 함수 구성 
⚠️ 함수명을 똑같게 할 필요는 없습니다 ⚠️
1. **is_vaild**
→ 해당 좌표가 지도 안에 있는지 & 해당 칸이 장애물(value = 1)이 아닌지 검사하는 함수
2. **load_map**
→ map.txt 파일에서 지도 크기와 각 칸의 정보를 읽어 저장하는 함수
3. **dijkstra**
→ 우리의 주요 알고리즘!
4. **print_path**
→ 경로 시각화
### 주의 사항
- 무조건 map.txt 파일을 읽어서 지도를 구성해야 합니다. 
- 배열에 직접적인 map이 구현되어있으면 0점 처리 
- 에너지의 개수는 0\~n개
- 파일을 읽고 에너지의 개수를 확인하세요! (동적 할당)
- 만일 배열로 구성한다면 최대의 개수는 = 80\*80-2 = 6398개
- 경로가 없다면 없다는 출력이 무조건 나와야 합니다.
- 출력 지도 형식을 모두 따라주세요
- 경로 중 단순 경로 : +
- 경로 중 에너지가 있던 위치 : @
- 장애물 : 1
- 나머지 경로에 포함되지 않는 공간 : .

---

# 미로 만들기 과제 



## **전체 코드**

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>  // exit(0)를 위해 필요
#include <time.h>

#define MAX_SIZE 30
#define STACK_SIZE (MAX_SIZE * MAX_SIZE)

// 좌표 구조체
typedef struct {
    int row;
    int col;
} COORD;

// 전역 변수
static int maze[MAX_SIZE][MAX_SIZE]; // 미로 배열
static int n;                        // 미로 크기

// 스택
static COORD path_stack[STACK_SIZE];
static int top = -1;

// 방문 배열 (첫 번째 방법 - 백트래킹 시 visited 해제 없음)
static COORD visited[STACK_SIZE];
static int visitedIndex = -1;

// 스택 함수
int isStackEmpty(void) {
    return (top == -1);
}

int isStackFull(void) {
    return (top == STACK_SIZE - 1);
}

void push(COORD c) {
    if (!isStackFull()) {
        path_stack[++top] = c;
    }
}

COORD pop(void) {
    COORD errorCoord = { -1, -1 };
    if (isStackEmpty()) {
        return errorCoord;
    }
    return path_stack[top--];
}

COORD peek(void) {
    COORD errorCoord = { -1, -1 };
    if (isStackEmpty()) {
        return errorCoord;
    }
    return path_stack[top];
}

// visited 배열 관련
int checkVisited(COORD c) {
    for (int i = 0; i <= visitedIndex; i++) {
        if (visited[i].row == c.row && visited[i].col == c.col) {
            return 1;
        }
    }
    return 0;
}

void addToVisited(COORD c) {
    if (!checkVisited(c)) {
        visited[++visitedIndex] = c;
    }
}

// 미로 범위 체크
int isWithinMap(COORD c) {
    return (c.row >= 0 && c.row < n && c.col >= 0 && c.col < n);
}

// 벽 체크
int isWall(COORD c) {
    return (maze[c.row][c.col] == 1);
}

// 목적지 도달 여부
int checkDestination(COORD current, COORD dest) {
    return (current.row == dest.row && current.col == dest.col);
}

// current에서 갈 수 있는 다음 좌표를 찾음 (없으면 -1, -1)
COORD findWhereToGo(COORD current) {
    COORD next;

    // 위
    next.row = current.row - 1;
    next.col = current.col;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 아래
    next.row = current.row + 1;
    next.col = current.col;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 왼쪽
    next.row = current.row;
    next.col = current.col - 1;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 오른쪽
    next.row = current.row;
    next.col = current.col + 1;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    COORD noWay = { -1, -1 };
    return noWay;
}

/*
    DFS를 이용하여 (0,0)에서 (n-1,n-1)까지 경로가 존재하는지 확인
    첫 번째 방법: pop 시 visited 해제 X
*/
int pathExists(COORD start, COORD dest) {
    // 스택, visited 초기화
    top = -1;
    visitedIndex = -1;

    addToVisited(start);
    COORD current = start;

    while (1) {
        COORD whereToGo = findWhereToGo(current);

        if (whereToGo.row != -1 && whereToGo.col != -1) {
            // 갈 수 있으면 스택에 push 후 이동
            push(current);
            current = whereToGo;
            addToVisited(current);

            if (checkDestination(current, dest)) {
                return 1; // 경로 있음
            }
        }
        else {
            // 갈 곳이 없으므로 백트래킹
            while (1) {
                COORD topCoord = peek();
                if (topCoord.row == -1 && topCoord.col == -1) {
                    // 스택이 비었으면 경로 없음
                    return 0;
                }
                COORD nextTry = findWhereToGo(topCoord);
                if (nextTry.row == -1 && nextTry.col == -1) {
                    // 더 이상 갈 곳 없으면 pop
                    pop();
                }
                else {
                    current = nextTry;
                    addToVisited(current);
                    break;
                }
            }
        }
    }
    return 0; // 논리상 도달X
}

/*
    실제 경로를 출력하는 함수
    첫 번째 방법: pop 시 visited 해제 X
*/
void findPathAndPrint(COORD start, COORD dest) {
    top = -1;
    visitedIndex = -1;

    addToVisited(start);
    COORD current = start;

    while (1) {
        COORD whereToGo = findWhereToGo(current);

        if (whereToGo.row != -1 && whereToGo.col != -1) {
            push(current);
            current = whereToGo;
            addToVisited(current);

            if (checkDestination(current, dest)) {
                // 스택에 들어있는 경로 출력
                for (int i = 0; i <= top; i++) {
                    printf("(%d, %d)\n", path_stack[i].row, path_stack[i].col);
                }
                printf("목적지 도착 (%d, %d)\n", current.row, current.col);
                return;
            }
        }
        else {
            while (1) {
                COORD topCoord = peek();
                if (topCoord.row == -1 && topCoord.col == -1) {
                    printf("경로가 없습니다.\n");
                    return;
                }
                COORD nextTry = findWhereToGo(topCoord);
                if (nextTry.row == -1 && nextTry.col == -1) {
                    pop();
                }
                else {
                    current = nextTry;
                    addToVisited(current);
                    break;
                }
            }
        }
    }
}

// 현재 미로에서 벽(1)의 개수를 세는 함수
int countWalls(void) {
    int count = 0;
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            if (maze[r][c] == 1) {
                count++;
            }
        }
    }
    return count;
}

/*
   벽 비율이 70%가 될 때까지 무작위로 벽을 추가해 보고,
   경로가 끊기면 다시 제거.

   → **최대 시도 횟수**를 두어, 이를 넘으면 바로 exit(0)로 프로그램 종료.
      => 미로 출력도 하지 않고, 뒤의 경로 탐색도 하지 않음.
*/
void fillMazeUntil70Percent(void) {
    int totalCells = n * n;
    int wallThreshold = (int)(0.7 * totalCells);

    int attempts = 0;
    int maxAttempts = 100000; // 상황에 따라 조절

    while (countWalls() < wallThreshold && attempts < maxAttempts) {
        attempts++;

        int r = rand() % n;
        int c = rand() % n;

        // (0,0), (n-1,n-1)은 벽 불가
        if ((r == 0 && c == 0) || (r == n - 1 && c == n - 1)) {
            continue;
        }
        // 이미 벽이면 패스
        if (maze[r][c] == 1) {
            continue;
        }

        // 임시로 벽 추가
        maze[r][c] = 1;

        COORD start = { 0, 0 };
        COORD end = { n - 1, n - 1 };

        // 경로 존재 여부 확인
        if (!pathExists(start, end)) {
            // 경로가 끊기면 다시 되돌림
            maze[r][c] = 0;
        }
    }

    // 시도 횟수 초과했는데도 70%에 도달 못했다면
    if (countWalls() < wallThreshold) {
        printf("최대 시도 횟수에 도달했습니다. 70%% 벽 조건을 만족하지 못했습니다.\n");
        exit(0);
    }
}

// 최종 미로 출력
void printMaze(void) {
    printf("최종 미로(%dx%d):\n", n, n);
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            printf("%d ", maze[r][c]);
        }
        printf("\n");
    }
}

int main(void) {
    srand((unsigned)time(NULL));

    // 미로 크기 입력 (5 <= n <= 30)
    do {
        printf("미로의 크기 n을 입력하세요(5~30): ");
        scanf("%d", &n);
    } while (n < 5 || n > 30);

    // 미로 초기화
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            maze[r][c] = 0;
        }
    }

    // 벽 70% 달성 시도 (실패 시 여기서 exit(0))
    fillMazeUntil70Percent();

    // 여기까지 오면 70% 벽을 만족했다는 뜻
    // => 미로 출력 & 경로 탐색
    printMaze();

    // 경로 확인
    COORD start = { 0, 0 };
    COORD end = { n - 1, n - 1 };

    if (pathExists(start, end)) {
        printf("\n경로가 존재합니다! 경로를 출력합니다.\n");
        printf("벽의 개수 : %d\n", countWalls());
        findPathAndPrint(start, end);
    }
    else {
        printf("\n경로가 존재하지 않습니다.\n");
    }

    return 0;
}

```



## **함수 설명**

## 1. 스택 관련 함수
### 1.1. isStackEmpty
```c
c
복사
int isStackEmpty(void) {
    return (top == -1);
}


```
- **역할:** 스택이 비어있는지를 확인합니다.
- **동작:** 전역 변수 `top`이 -1이면 스택에 아무런 요소가 없으므로 1(참)을 반환하고, 그렇지 않으면 0(거짓)을 반환합니다.
- **주의:** 스택 초기 상태에서 `top`은 -1로 설정되어 있어야 올바른 동작을 합니다.
---
### 1.2. isStackFull
```c
c
복사
int isStackFull(void) {
    return (top == STACK_SIZE - 1);
}


```
- **역할:** 스택이 꽉 찼는지 검사합니다.
- **동작:** `top`이 `STACK_SIZE - 1`과 같으면 스택에 더 이상 요소를 넣을 공간이 없으므로 1(참)을 반환합니다.
- **주의:** STACK_SIZE는 미로의 최대 크기에 따라 정해진 상수입니다.
---
### 1.3. push
```c
c
복사
void push(COORD c) {
    if (!isStackFull()) {  // 스택이 꽉 차지 않았다면
        path_stack[++top] = c;
    }
}


```
- **역할:** 스택에 새로운 좌표 `c`를 추가합니다.
- **동작:**
1. 먼저 `isStackFull()` 함수를 호출하여 스택에 여유가 있는지 확인합니다.
2. 여유가 있다면 `top`을 1 증가시킨 후, 해당 위치에 좌표 `c`를 저장합니다.
- **주의:** 스택이 꽉 찬 경우 추가하지 않음으로써 오버플로우를 방지합니다.
---
### 1.4. pop
```c
c
복사
COORD pop(void) {
    COORD errorCoord = { -1, -1 };
    if (isStackEmpty()) {
        return errorCoord;  // 스택이 비어 있으면 오류값 반환
    }
    return path_stack[top--];
}


```
- **역할:** 스택의 맨 위에 있는 좌표를 꺼내 반환합니다.
- **동작:**
1. 스택이 비어있는지 확인하고, 비어 있다면 `(-1, -1)` 오류값을 반환합니다.
2. 그렇지 않으면 현재 `top` 위치의 좌표를 반환한 후 `top`을 1 감소시킵니다.
- **주의:** 후입선출(LIFO) 원칙을 따릅니다.
---
### 1.5. peek
```c
c
복사
COORD peek(void) {
    COORD errorCoord = { -1, -1 };
    if (isStackEmpty()) {
        return errorCoord;  // 스택이 비어 있으면 오류값 반환
    }
    return path_stack[top];
}


```
- **역할:** 스택의 최상위 좌표를 반환하되, 스택에서 제거하지는 않습니다.
- **동작:** 스택이 비어 있으면 `(-1, -1)` 오류값을, 그렇지 않으면 `path_stack[top]`을 반환합니다.
- **주의:** 주로 백트래킹 시 현재 상태를 확인하는 데 사용됩니다.
---
## 2. 방문 관리 함수
### 2.1. checkVisited
```c
c
복사
int checkVisited(COORD c) {
    for (int i = 0; i <= visitedIndex; i++) {
        if (visited[i].row == c.row && visited[i].col == c.col) {
            return 1;  // 이미 방문한 좌표인 경우 1 반환
        }
    }
    return 0;
}


```
- **역할:** 주어진 좌표 `c`가 이미 방문한 좌표 목록에 있는지 확인합니다.
- **동작:**
1. `visited` 배열의 0부터 `visitedIndex`까지 순회하면서 `c`와 같은 (row, col)을 찾습니다.
2. 동일 좌표를 찾으면 1을 반환, 찾지 못하면 0을 반환합니다.
- **주의:** DFS 탐색 시 중복 방문을 막아 무한 루프에 빠지는 것을 방지합니다.
---
### 2.2. addToVisited
```c
c
복사
void addToVisited(COORD c) {
    if (!checkVisited(c)) {  // 방문하지 않았다면
        visited[++visitedIndex] = c;
    }
}


```
- **역할:** 좌표 `c`가 방문되지 않았다면 `visited` 배열에 추가합니다.
- **동작:** `checkVisited`를 사용해 이미 방문한 좌표인지 확인 후, 방문하지 않았다면 `visitedIndex`를 증가시키고 좌표 `c`를 추가합니다.
- **주의:** 중복 추가를 피하기 위해 항상 `checkVisited`로 검증합니다.
---
## 3. 미로 관련 기본 함수
### 3.1. isWithinMap
```c
c
복사
int isWithinMap(COORD c) {
    return (c.row >= 0 && c.row < n && c.col >= 0 && c.col < n);
}


```
- **역할:** 주어진 좌표 `c`가 미로의 범위 내에 있는지 확인합니다.
- **동작:** `c.row`와 `c.col`이 0 이상이고 n 미만인지 검사하여, 조건을 만족하면 1, 아니면 0을 반환합니다.
- **주의:** 미로 외부로의 접근을 방지하는 중요한 함수입니다.
---
### 3.2. isWall
```c
c
복사
int isWall(COORD c) {
    return (maze[c.row][c.col] == 1);
}


```
- **역할:** 해당 좌표 `c`가 벽인지(값이 1인지) 검사합니다.
- **동작:** `maze[c.row][c.col]`의 값이 1이면 벽으로 간주하고 1을 반환, 그렇지 않으면 0을 반환합니다.
- **주의:** 경로 탐색 시 이동 가능한 셀(0, 통로)과 벽(1)을 구분하는 데 사용됩니다.
---
### 3.3. checkDestination
```c
c
복사
int checkDestination(COORD current, COORD dest) {
    return (current.row == dest.row && current.col == dest.col);
}


```
- **역할:** 현재 좌표 `current`가 목표(출구) 좌표 `dest`와 동일한지 확인합니다.
- **동작:** 두 좌표의 `row`와 `col` 값이 모두 같으면 1(도착)을 반환합니다.
- **주의:** DFS 탐색 중 목적지에 도달했는지 판단하는 기준입니다.
---
## 4. DFS 이동 선택 함수
### 4.1. findWhereToGo
```c
c
복사
COORD findWhereToGo(COORD current) {
    COORD next;

    // 위쪽 방향 검사
    next.row = current.row - 1;
    next.col = current.col;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 아래쪽 방향 검사
    next.row = current.row + 1;
    next.col = current.col;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 왼쪽 방향 검사
    next.row = current.row;
    next.col = current.col - 1;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 오른쪽 방향 검사
    next.row = current.row;
    next.col = current.col + 1;
    if (isWithinMap(next) && !isWall(next) && !checkVisited(next)) {
        return next;
    }

    // 네 방향 모두 이동 불가능할 경우 (-1, -1) 반환
    COORD noWay = { -1, -1 };
    return noWay;
}


```
- **역할:** 현재 좌표 `current`에서 이동할 수 있는 인접 셀 중 하나를 결정합니다.
- **동작:**
1. 위, 아래, 왼쪽, 오른쪽 순서대로 인접 셀을 검사합니다.
2. 각 방향에서 먼저 미로 범위 내에 있는지(`isWithinMap`), 벽이 아닌지(`!isWall`), 그리고 아직 방문하지 않은지(`!checkVisited`) 확인합니다.
3. 조건을 만족하는 첫 번째 방향의 좌표를 반환합니다.
4. 네 방향 모두 조건을 만족하지 않으면 `(-1, -1)`을 반환하여 이동 불가능을 알립니다.
- **주의:** DFS의 다음 이동 방향 결정에 핵심적인 역할을 합니다.
---
## 5. DFS 기반 경로 탐색 및 출력 함수
### 5.1. pathExists
```c
c
복사
int pathExists(COORD start, COORD dest) {
    // 스택과 방문 배열 초기화
    top = -1;
    visitedIndex = -1;

    // 시작 좌표 방문 처리
    addToVisited(start);
    COORD current = start;

    while (1) {
        COORD whereToGo = findWhereToGo(current);

        // 이동 가능한 좌표가 있으면
        if (whereToGo.row != -1 && whereToGo.col != -1) {
            push(current);         // 현재 좌표를 스택에 저장
            current = whereToGo;     // 다음 좌표로 이동
            addToVisited(current);   // 새 좌표 방문 처리

            // 목적지에 도달했는지 확인
            if (checkDestination(current, dest)) {
                return 1;  // 경로 존재
            }
        }
        else {
            // 이동할 수 없는 경우, 백트래킹 수행
            while (1) {
                COORD topCoord = peek();
                if (topCoord.row == -1 && topCoord.col == -1) {
                    return 0;  // 스택이 비면 경로 없음
                }
                COORD nextTry = findWhereToGo(topCoord);
                if (nextTry.row == -1 && nextTry.col == -1) {
                    pop();  // 가능한 경로가 없으면 스택에서 제거
                }
                else {
                    current = nextTry;  // 새 경로로 이동
                    addToVisited(current);
                    break;
                }
            }
        }
    }
    return 0;  // (논리상 도달하지 않음)
}


```
- **역할:** DFS를 이용해 시작점 `start`에서 출구 `dest`까지의 경로가 존재하는지 검사합니다.
- **동작:**
1. 스택(`path_stack`)과 방문 배열(`visited`)을 초기화한 후 시작 좌표를 방문 처리합니다.
2. `findWhereToGo`로 이동 가능한 방향을 찾고, 가능하면 현재 좌표를 스택에 저장한 뒤 다음 좌표로 이동합니다.
3. 새 좌표를 방문 처리하며, 목적지에 도달하면 1을 반환합니다.
4. 만약 이동할 수 없는 경우 백트래킹을 통해 이전 위치로 돌아가며 다른 경로를 탐색합니다.
5. 모든 경로를 탐색했음에도 목적지에 도달하지 못하면 0을 반환합니다.
- **주의:** 실제 경로 출력은 하지 않고 단순히 경로의 존재 여부만 확인합니다.
---
### 5.2. findPathAndPrint
```c
c
복사
void findPathAndPrint(COORD start, COORD dest) {
    // 스택과 방문 배열 초기화
    top = -1;
    visitedIndex = -1;

    // 시작 좌표 방문 처리
    addToVisited(start);
    COORD current = start;

    while (1) {
        COORD whereToGo = findWhereToGo(current);

        if (whereToGo.row != -1 && whereToGo.col != -1) {
            push(current);         // 현재 좌표를 스택에 저장
            current = whereToGo;     // 다음 좌표로 이동
            addToVisited(current);   // 새 좌표 방문 처리

            if (checkDestination(current, dest)) {
                // 스택에 저장된 경로 출력
                for (int i = 0; i <= top; i++) {
                    printf("(%d, %d)\n", path_stack[i].row, path_stack[i].col);
                }
                printf("목적지 도착 (%d, %d)\n", current.row, current.col);
                return;
            }
        }
        else {
            // 백트래킹: 더 이상 진행할 수 없으면 스택에서 pop하면서 새로운 경로 탐색
            while (1) {
                COORD topCoord = peek();
                if (topCoord.row == -1 && topCoord.col == -1) {
                    printf("경로가 없습니다.\n");
                    return;
                }
                COORD nextTry = findWhereToGo(topCoord);
                if (nextTry.row == -1 && nextTry.col == -1) {
                    pop();
                }
                else {
                    current = nextTry;
                    addToVisited(current);
                    break;
                }
            }
        }
    }
}


```
- **역할:** DFS를 사용하여 실제 경로를 찾고, 경로가 발견되면 스택에 저장된 좌표들을 출력합니다.
- **동작:**
1. `pathExists`와 유사하게 DFS 방식으로 경로 탐색을 진행합니다.
2. 목적지에 도달하면 스택에 쌓인 좌표들을 순차적으로 출력하고 마지막에 도착한 좌표도 출력합니다.
3. 경로를 찾지 못하면 "경로가 없습니다." 메시지를 출력합니다.
- **주의:** 실제 경로를 사용자에게 보여주기 때문에 경로의 순서를 확인할 수 있습니다.
---
## 6. 미로 구성 및 출력 함수
### 6.1. countWalls
```c
c
복사
int countWalls(void) {
    int count = 0;
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            if (maze[r][c] == 1) {
                count++;
            }
        }
    }
    return count;
}


```
- **역할:** 현재 미로에서 벽(값이 1인 셀)의 총 개수를 센 후 반환합니다.
- **동작:** 이중 for문을 통해 미로 전체를 순회하며 값이 1인 셀을 카운트합니다.
- **주의:** 미로의 벽 비율 계산이나 벽 추가 로직에서 사용됩니다.
---
### 6.2. fillMazeUntil70Percent
```c
c
복사
void fillMazeUntil70Percent(void) {
    int totalCells = n * n;
    int wallThreshold = (int)(0.7 * totalCells);

    // 벽의 개수가 목표치(70%)에 도달할 때까지 반복
    while (countWalls() < wallThreshold) {
        // 임의의 좌표 선택
        int r = rand() % n;
        int c = rand() % n;

        // 시작점과 출구는 벽이 될 수 없음
        if ((r == 0 && c == 0) || (r == n - 1 && c == n - 1)) {
            continue;
        }
        // 이미 벽인 경우는 건너뜀
        if (maze[r][c] == 1) {
            continue;
        }

        // 임시로 벽 추가
        maze[r][c] = 1;

        COORD start = { 0, 0 };
        COORD end = { n - 1, n - 1 };

        // 추가한 벽 때문에 경로가 끊기는지 확인
        if (!pathExists(start, end)) {
            // 경로가 끊겼으면 추가한 벽 제거
            maze[r][c] = 0;
        }
    }
}


```
- **역할:** 전체 셀의 70%가 벽이 될 때까지 무작위로 벽을 추가하되, (0,0)에서 (n-1,n-1)까지 항상 경로가 유지되도록 합니다.
- **동작:**
1. 전체 셀의 개수와 목표 벽 개수를 계산합니다.
2. 무한 루프를 통해 임의의 좌표를 선택하고, 시작점과 출구는 제외한 후 벽이 아닌 경우 임시로 벽(1)을 추가합니다.
3. `pathExists` 함수를 사용하여 경로가 유지되는지 확인하고, 경로가 끊겼다면 추가한 벽을 제거합니다.
- **주의:** 경로 유지가 중요한 조건이므로, 추가된 벽으로 인해 경로가 끊기는 경우 바로 복원합니다.
---
### 6.3. printMaze
```c
c
복사
void printMaze(void) {
    printf("최종 미로(%dx%d):\n", n, n);
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            printf("%d ", maze[r][c]);
        }
        printf("\n");
    }
}


```
- **역할:** 최종적으로 생성된 미로의 상태를 화면에 출력합니다.
- **동작:**
1. 미로의 크기와 각 행별로 셀의 값(0: 통로, 1: 벽)을 반복문을 통해 출력합니다.
- **주의:** 미로의 구조를 시각적으로 확인할 수 있도록 합니다.
---
## 7. 메인 함수
```c
c
복사
int main(void) {
    srand((unsigned)time(NULL));  // 실행마다 다른 난수를 위해 시드 초기화

    // 미로 크기 n 입력 (5~30 사이)
    do {
        printf("미로의 크기 n을 입력하세요(5~30): ");
        scanf("%d", &n);
    } while (n < 5 || n > 30);

    // 미로 배열 초기화: 모든 셀을 통로(0)로 설정
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            maze[r][c] = 0;
        }
    }
    // (0,0)과 (n-1,n-1)는 반드시 통로여야 함

    // 미로에 벽 추가: 전체 셀의 70%가 벽이 될 때까지 벽 추가 시도 (경로 유지 조건 포함)
    fillMazeUntil70Percent();

    // 최종 미로 상태 출력
    printMaze();

    // 경로 탐색 후, 경로가 존재하면 경로 출력
    COORD start = { 0, 0 };
    COORD end = { n - 1, n - 1 };

    if (pathExists(start, end)) {
        printf("\n경로가 존재합니다! 경로를 출력합니다.\n");
        findPathAndPrint(start, end);
    }
    else {
        printf("\n경로가 존재하지 않습니다.\n");
    }

    return 0;
}


```
- **역할:** 프로그램의 전체 흐름을 제어합니다.
- **동작:**
1. 랜덤 시드를 초기화한 후, 사용자로부터 미로 크기 `n`을 입력받습니다.
2. 미로 배열을 모두 0(통로)로 초기화합니다.
3. `fillMazeUntil70Percent()` 함수를 통해 미로에 벽을 추가하면서 경로 유지 여부를 검사합니다.
4. 최종 미로 상태를 `printMaze()`로 출력하고, 경로가 존재하는 경우 DFS를 통해 경로를 찾아 출력합니다.
- **주의:** 사용자의 입력 값에 따라 미로 크기가 달라지며, 경로 탐색 결과에 따라 경로 출력 여부가 달라집니다.
---
이와 같이 각 함수별로 코드를 함께 제시하고 자세히 설명하면, 프로그램의 전체 흐름과 각 함수가 담당하는 역할을 쉽게 이해할 수 있습니다.

---
### 3/25 수업 진행
- 출석 부르기
- 과제에 대한 안내 및 힌트
- 손코딩에 대한 안내
---


## **코드**

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

// 미로 찾기 문제
// 2차원 배열 (7*7)ㅁ
// 경로 블럭 : 0
// 벽 블럭 : 1
#define size 7
#define stack_size (size*size)

// 좌표 정보를 나타내는 구조체
struct COORD {
   int row; // 행
   int col; // 열
};

#if 1

struct COORD path_stack[stack_size];
int top = -1;

// stack 관련 함수
// push
// pop
// isStackEmpty
// isStackFull
// peek (pop과 유사하지만, 실제로 pop은 하지 않고, 맨 위에 있는 값을 보기만 한다.)

// if stack is empty, return 1(non-zero)
int isStackEmpty(void) {
   if (top == -1) {
      return 1;
   }
   else {
      return 0;
   }
}

// if stack is full, return 1(non-zero)
int isStackFull(void) {
   if (top == (stack_size - 1)) {
      return 1;
   }
   else {
      return 0;
   }
}

void push(struct COORD _c) {
   if (isStackFull()) {
      return;
   }
   top += 1;
   path_stack[top] = _c;
   return;
}

struct COORD pop(void) {

   struct COORD result = { -1 , -1 };
   // 구조체는 처음 선언할 때는 값을 넣을 수 있지만, 선언하고 나서는 값을 못 넣음

   if (isStackEmpty()) {
      return result; // stack이 비었음을 암시
   }

   result = path_stack[top];
   top -= 1;
   return result;
}

// 살짝 들여다보기만 하는 peek (pop 안함)
struct COORD peek(void) {

   struct COORD result = { -1 , -1 };

   if (isStackEmpty()) {
      return result; // stack이 비었음을 암시
   }

   result = path_stack[top];
   return result;
}
#endif

int maze[size][size] = {
   {0, 1, 0, 1, 0, 0, 0},
   {0, 1, 0, 1, 0, 1, 0},
   {0, 0, 0, 1, 0, 1, 1},
   {0, 1, 1, 1, 0, 1, 0},
   {0, 1, 0, 0, 0, 0, 0},
   {0, 1, 0, 1, 0, 1, 0},
   {0, 1, 0, 1, 0, 1, 0}
};

// 방문했던 곳을 저장하는 배열
struct COORD visited[size * size];

int visitedIndex = -1; //

// 좌표 _c를 방문했었는지 판단
// 했었으면 1, 안했으면 0
int checkVisited(struct COORD _c) {
   for (int i = 0; i <= visitedIndex; i++) {
      if ((visited[i].row == _c.row) && (visited[i].col == _c.col)) {
         return 1;
      }
   }
   return 0;
}

void addToVisited(struct COORD _c) {
   // 있는지를 검사한다.
   // 없으면, 추가한다. 이때 visitedIndex를 먼저 증가시킨다.
   if (checkVisited(_c) == 0) {
      visitedIndex++; //이게 중요,, 먼저 증가
      visited[visitedIndex] = _c;
   }
}
// 1. _c가 지도 영역 내에 있는가?
// 있다면 1, 없다면 0 (0 ~ size -1)
int isWithinMap(struct COORD _c) {
   if ((_c.row >= 0) && (_c.row < size) && (_c.col >= 0) && (_c.col < size)) {
      return 1;
   }
   else {
      return 0;
   }
}
// 2. 벽인가?
// 벽이면 1, 아니면 0
int isWall(struct COORD _c) {
   if (maze[_c.row][_c.col] == 1) {
      return 1;
   }
   else {
      return 0;
   }
}

// _c를 기준으로 갈 수 있는 좌표 1곳을 반환
// 만약 없으면 (-1,-1)을 반환
struct COORD findWhereToGo(struct COORD _c) {

   // 체크할 방향을 저장하는 변수
   struct COORD target;

   // 위
   target.row = _c.row - 1;
   target.col = _c.col;
   // 1. 지도 영역 내인가?
   // 2. 벽인가?
   // 3. 가본 적 있는 곳인가?
   if (isWithinMap(target) == 1 && isWall(target) == 0 && checkVisited(target) == 0) {
      return target;
   }

   // 아래
   target.row = _c.row + 1;
   target.col = _c.col;
   if (isWithinMap(target) == 1 && isWall(target) == 0 && checkVisited(target) == 0) {
      return target;
   }

   // 왼쪽
   target.row = _c.row;
   target.col = _c.col - 1;
   if (isWithinMap(target) == 1 && isWall(target) == 0 && checkVisited(target) == 0) {
      return target;
   }

   // 오른쪽
   target.row = _c.row;
   target.col = _c.col + 1;
   if (isWithinMap(target) == 1 && isWall(target) == 0 && checkVisited(target) == 0) {
      return target;
   }

   target.row = -1;
   target.col = -1;
   return target;
}

// _c가 _dst와 같은지 판단
// 같으면 1을 반환, 아니면 0을 반환
int checkDestination(struct COORD _c, struct COORD _dst) {
   if ((_c.row == _dst.row) && (_c.col == _dst.col)) {
      return 1;
   }
   else {
      return 0;
   }
}

// _s에서 _d까지 경로를 출력하는 함수
void findPath(struct COORD _s, struct COORD _d) {

   //현재 위치를 설정 -> 시작 위치 = current
   struct COORD current = _s;

   // 출발 좌표 저장
   addToVisited(current);

   while (1) {

      // 현재 위치에서 가는 곳을 찾는다.
      struct COORD whereToGo = findWhereToGo(current);

      if ((whereToGo.row != -1) && (whereToGo.col != -1)) {
         // 갈 곳이 있음
         push(current); // 경로를 저장해둔다.
         current = whereToGo; // current는 항상 현재 위치를 나타내는 역할
         addToVisited(current); // 내가 여기 왔다고 표시
         if (checkDestination(current, _d) == 1) {
            // 끝 도착함

            // stack에 들어있는 경로를 모두 출력
            for (int i = 0; i <= top; i++) {
               printf("(%d, %d) \n", path_stack[i].row, path_stack[i].col);
            }

            printf("목적지 도착 (%d, %d)\n", current.row, current.col);
            return;
         }
      }
      else {
         // 갈 곳이 없음
         // 갈림길 판단할 때 -> findwheretogo() 사용하면 갔던곳인지 아닌지 판단 가능
         // 현 위치와 가장 가까운 갈림길부터 판단

         while (1) {

            // top_coord <= stack의 맨 위에 있는 곳을 peek (pop 아님)
            struct COORD top_coord = peek();

            // top_coord(-1,-1) : stack이 비었음. 더 이상 돌아갈 곳이 없음 = 경로가 없음 => 끝임 프로그램 종료
            // ㄴ> isEmpty에서 판별되서 나온 좌표
            if ((top_coord.row == -1) && (top_coord.col == -1)) {
               printf("경로가 없습니다.\n");
               return;
            }

            whereToGo = findWhereToGo(top_coord);

            // findWhereToGo(top_coord) ==> (-1, -1)
            // 이 좌표(top_coord)는 도움이 안됨. 즉, 일방통행일때, 더 이전으로 돌아가야함. 이 좌표는 버림 (pop해서 버리자)
            if ((whereToGo.row == -1) && (whereToGo.col == -1)) {
               pop(); // top_coord를 날려버림.
            }

            else {
               // findWhereToGo(top_coord) ==> (유효한 좌표)
               // current = findWhereToGo
               // !! push가 필요없음. => top_coord가 아직 stack에 있기 때문에
               // addToVisited(current)
               current = whereToGo;
               addToVisited(current);
               break;
            }
         }
      }
   }
}
// 스택을 구현하여 작성은 다음시간에.


int main(void) {

   struct COORD start_point = { 0, 0 };
   struct COORD dest_point = { 6, 6 };

   // start_point ---> dest_point 까지 경로 표시
   findPath(start_point, dest_point);

   return 0;
}


```



## **함수 설명**

- Stack 관련 함수
- **isStackEmpty()**
스택이 비어있는지 확인합니다.
- top 값이 -1이면 스택이 비어 있으므로 1을 반환하고, 그렇지 않으면 0을 반환합니다.
- **isStackFull()**
스택이 꽉 찼는지 확인합니다.
- top 값이 스택 최대 크기(stack_size-1)와 같으면 꽉 찬 것으로 1을 반환하고, 아니면 0을 반환합니다.
- **push(struct COORD _c)**
스택에 좌표를 추가합니다.
- 스택이 꽉 차지 않았다면 top을 1 증가시키고, 해당 좌표를 스택에 저장합니다.
- **pop()**
스택의 맨 위 좌표를 꺼내 반환합니다.
- 스택이 비어있으면 (-1, -1)을 반환하며, 아니라면 top에 있는 좌표를 반환한 후 top 값을 1 감소시킵니다.
- **peek()**
스택의 맨 위 좌표를 확인합니다.
- 스택에서 좌표를 제거하지 않고 조회하며, 스택이 비어있을 경우 (-1, -1)을 반환합니다.
---
- Maze 관련 함수
- **checkVisited(struct COORD _c)**
주어진 좌표를 이미 방문했는지 검사합니다.
- 방문 배열을 순회하여 이미 방문한 좌표라면 1, 아니면 0을 반환합니다.
- **addToVisited(struct COORD _c)**
아직 방문하지 않은 좌표라면 방문 배열에 추가합니다.
- **isWithinMap(struct COORD _c)**
좌표가 미로 범위 내에 있는지 확인합니다.
- 좌표의 행(row)과 열(col)이 0 이상 size 미만이면 1, 아니면 0을 반환합니다.
- **isWall(struct COORD _c)**
주어진 좌표가 벽(maze 배열에서 1인 경우)인지 확인합니다.
- 벽이면 1, 아니라면 0을 반환합니다.
- **findWhereToGo(struct COORD _c)**
현재 좌표에서 갈 수 있는 다음 좌표를 찾습니다.
- 순서대로 **위**, **아래**, **왼쪽**, **오른쪽**을 확인하여,
1. 미로 범위 내인지,
2. 벽이 아니며,
3. 이미 방문하지 않은 곳인지를 검사합니다.
- 조건을 만족하는 좌표가 있으면 반환하고, 없으면 (-1, -1)을 반환합니다.
- **checkDestination(struct COORD _c, struct COORD _dst)**
현재 좌표와 목적지 좌표가 같은지 비교합니다.
- 같으면 1, 다르면 0을 반환합니다.
---
- 거의 main
- **findPath(struct COORD _s, struct COORD _d)**
시작점에서 목적지까지의 경로를 찾는 핵심 함수입니다.
- **깊이 우선 탐색(DFS)** 방식을 사용해 스택에 경로를 저장하고,
- 현재 위치에서 갈 수 있는 방향을 찾은 후, 갈 수 있으면 스택에 현재 좌표를 저장하고 이동합니다.
- 만약 이동할 곳이 없으면 스택에서 이전 좌표로 돌아가면서 다시 가능한 길을 찾습니다.
- 목적지에 도달하면 스택에 저장된 경로와 마지막 좌표(목적지)를 출력합니다.
---
- 진짜 main
- **main()**
프로그램의 시작점입니다.
- 시작점 (0, 0)과 도착점 (6, 6)을 설정한 후, findPath 함수를 호출하여 미로에서 경로를 찾습니다.



## path_stack과 visited의 다른 점

### 1. `path_stack`:
**현재 "경로"를 추적하기 위한 스택**입니다.
- \*DFS가 지나온 길(경로)\*\*을 저장합니다.
- 경로를 따라 한 칸 전진할 때마다 `push()`,
더 이상 갈 곳이 없을 때 `pop()`하면서 백트래킹합니다.
- 스택이기 때문에 순서를 지키면서 되돌아갈 수 있어요.
- 나중에 경로를 출력할 때도 이 스택을 그대로 출력하면 됨.
---
### 2. `visited`:
**이미 방문한 적이 있는지를 체크하기 위한 배열**입니다.
- 미로를 탐색할 때 **같은 칸을 두 번 이상 방문하지 않도록** 하기 위한 기록용입니다.
- push/pop과 관계없이, 한 번 방문했으면 `visited`에 기록되고,
다음에 그 칸에 갈 수 있는지 판단할 때 `checkVisited()`로 확인합니다.
- 다시 말해, **중복 방문 방지용**입니다.
- 스택처럼 되돌릴 때 `pop()` 하지 않습니다.

- 그 외 질의응답 

현재 개개인의 코드를 보고 플로우를 이해한 다음에 세부적으로 알려드리기에는 시간적 여유가 없기 때문에 그런 질문은 수업 이후에 카톡으로 해주시고 지금은 세세한 질문보다는 방금 설명드린 코드에서나 또는 과제 문제에서의 질의응답을 받도록 하겠습니다.

### 3. 과제에 대한 안내 및 힌트
[image omitted: personal or temporary Notion asset]
- 기존 코드에서 추가해야 하는 점
1. 기존 코드에서는 미로의 크기가 고정되어 있으나 과제에서는 입력을 받아서 미로의 사이즈를 조절해야 함.
2. 미로의 벽을 랜덤으로 생성해야 하며 생성할 때 마다 경로가 있는지를 확인해야 함.
1. 경로가 있으면 벽을 더 생성해서 벽이 (n-1 \* n-1)의 70%이상이 될 때까지 
2. 경로가 없다면 만든 벽을 허물고 다시 벽을 생성
- 내 코드 실행하는 거 보여주기
- 10 넣었을 때 → 경로 출력
- 5 넣었을 때 → 경로가 없다는 것 출력
- 이때 해당 경로가 왜 이렇게 나오는지  그 과정을 설명해주기
- 내 코드에서의 힌트

코드는 정답이 없기 때문에 꼭 내가 한 방식대로 하지 않고도 정답을 맞출 수 있다는 점 알려주자.

1. 벽 개수를 카운트하는 함수 (=countWalls)
2. 미로를 출력하는 함수 (=printMaze)
3. 경로 존재 여부 확인을 하는 함수 (=pathExists)
4. **랜덤 벽을 생성하는 함수 (fillUntil70Percnet) **
