# Insertion-Sort algorithm

> 마인드 
> - 빈 왼손에 카드를 넣으면서 정렬한다고 생각
> - 손에 많은 카드를 잡기 힘드니  **크기가 작은 정렬** 에 좋다


![](https://mblogthumb-phinf.pstatic.net/MjAyMjAyMDVfMzQg/MDAxNjQ0MDY5MTI0NzQw.dqH8ETBTKZpwL6eqbywdqAUlXCml9s0nymfF9lTUjD8g.yilHI9mvMnt1EroWVNF2SsbmKJWlymwGMveGNQkUFQog.JPEG.jurong25/20220205%EF%BC%BF224605.jpg?type=w800)


### pseudo code
``` python
for i = 1 to A.length
    key = A[i]  // 여기서 A[i]는 정렬초기 조건을 만족하는  A[1] 즉 정렬 됨 
    j = i - 1
    while j >= 0 and A[j] > key
        A[j + 1] = A[j]
        j = j - 1
    A[j + 1] = key
```
##  루프의 불변성
- A[i]는 정렬된 상태, 즉 왼손이 쥐고 있는 카드라고 생각
  - A[1 .. i-1]은 정렬된 상태
  - A[i+1 .. n]은 정렬되지 않은 상태
- A[1 .. i-1]를 루프의 불변성
  - > **뭔가 간단하게 말하면 루프시작 하기전에 A[1 .. i-1]정렬되어 있는가? 이다**

## 루프 불변성 조건 3가지 
- 초기조건 : 루프가 첫번째 반복을 시작하기 전에 루프 불변성이 참이여야 한다.
- 유지조건 : 루프가 다음 반복을 시작하되기 전까지도 불변성이 참이여야 한다.
- 종료조건 : 루프가 종료될 때 그 불변식이 알고리즘의 타당성을 보이는데 도움이 될 유용한 특성을 가져야 한다.
  

![Insertion-Sort](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
from [wikipedia](https://en.wikipedia.org/wiki/Insertion_sort)

--- 

## Appendix
### 시간복잡도
- 최선의 경우 : O(n)
- 최악의 경우 : O(n^2)
- 평균의 경우 : O(n^2)

### 공간복잡도
- O(1)

### 안정성
- 안정정렬



### C++ 코드

```cpp
#include <iostream>
#include <vector>
using namespace std;

void insertionSort(vector<int> &arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr = { 5, 2, 4, 6, 1, 3 };
    insertionSort(arr);
    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << " ";
    }
    return 0;
}
```

### python 코드
```python
def insertionSort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

arr = [5, 2, 4, 6, 1, 3]
insertionSort(arr)
print(arr)
```
### java 코드
```java
public class InsertionSort {
    public static void main(String[] args) {
        int[] arr = { 5, 2, 4, 6, 1, 3 };
        insertionSort(arr);
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }

    public static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int key = arr[i];
            int j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }
}
```
