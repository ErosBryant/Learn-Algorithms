# 분할 정복 _ 접근법
- 분할 정복은 원래 문제와 유사하지만  작은 문제로 나누어 풀고, 작은 문제를 재귀적으로 푸는 방법이다.

## 분할 정복 3 단계
1. 분할 : 현재 문제를 같은 문제를 다루는 다수의 부분 문제로 분할한다.
2. 정복 : 부분 문제를 재귀적으로 푼다.
3. 결합 : 부분 문제의 해를 결합하여 원래 문제의 해가 되도록 만든다.


### Merge Sort는 분할 정복 알고리즘의 대표적인 예이다.
1. 분할 : 정렬한 n개의 원소의 배열을 N/2개의 원소를 가진 두 배열로 분할한다.
2. 정복 : 병합 정렬을 이용해 두 부분 배병을 재귀적으로 정렬한다.
3. 결합: 정렬된 두개의 부분 배여을 병합하여 전체 배열을 정렬한다.
- merge하는 부분이 제일 핵심적인 부분인다.


### Merge pesudocode
- `경계카드`를 사용하여 두 부분 배열의 끝을 나타낸다. 

`Merge(A, p, q, r)` 
- A는 배열, p는 시작 인덱스, q는 중간 인덱스, r은 끝 인덱스
- A[p..q]와 A[q+1..r]을 병합한다.


```
Merge(A, p, q, r)
    n1 = q - p + 1   // 왼쪽 배열의 크기
    n2 = r - q    // 오른쪽 배열의 크기 
    let L[1..n1+1] and R[1..n2+1] be new arrays // 왼쪽 배열과 오른쪽 배열을 만든다.
    for i = 1 to n1 // 왼쪽 배열에 값을 넣는다.
        L[i] = A[p+i-1] 
    for j = 1 to n2 // 오른쪽 배열에 값을 넣는다.
        R[j] = A[q+j]
    L[n1+1] = ∞ // 무한대를 넣는다.
    R[n2+1] = ∞ // 이는 경계 카드이다 
    i = 1
    j = 1
    for k = p to r
        if L[i] <= R[j]
            A[k] = L[i]
            i = i + 1
        else A[k] = R[j]
            j = j + 1
```
---
## Merge Sort
![](https://cdn.programiz.com/cdn/farfuture/PRTu8e23Uz212XPrrzN_uqXkVZVY_E0Ta8GZp61-zvw/mtime:1586425911/sites/tutorial2program/files/merge-sort-example_0.png)[merge-sort](https://www.programiz.com/dsa/merge-sort)

Merge Sort pesudocode
```
MergeSort(A, p, r)
    if p < r
        q = ⌊(p+r)/2⌋
        MergeSort(A, p, q)
        MergeSort(A, q+1, r)
        Merge(A, p, q, r)
```
## Merge Sort Analysis
- 분할 : 분할 단계는 배열을 중간위치를 계산하므로  상수 시간이 걸린다.
- 정복 : 각각의 부분 배열을 재귀적으로 정렬한다. 
- 결합 : 두 부분 배열을 병합한다.

--- 

## Appendix

### 시간 복잡도
- 최선의 경우 : O(nlogn)
- 최악의 경우 : O(nlogn)
- 평균의 경우 : O(nlogn)

### 공간 복잡도
- O(n)

###  안정성
- 안정 정렬

### c++ 코드
```cpp
#include <iostream>
#include <vector>
using namespace std;

void merge(vector<int> &arr, int left, int mid, int right) {
    int i = left;
    int j = mid + 1;
    int k = left;
    vector<int> sorted(arr.size());

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            sorted[k] = arr[i];
            i++;
        }
        else {
            sorted[k] = arr[j];
            j++;
        }
        k++;
    }

    if (i > mid) {
        for (int t = j; t <= right; t++) {
            sorted[k] = arr[t];
            k++;
        }
    }
    else {
        for (int t = i; t <= mid; t++) {
            sorted[k] = arr[t];
            k++;
        }
    }

    for (int t = left; t <= right; t++) {
        arr[t] = sorted[t];
    }
}

void mergeSort(vector<int> &arr, int left, int right) {
    int mid;
    if (left < right) {
        mid = (left + right) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

int main() {
    vector<int> arr = { 69, 10, 30, 2, 16, 8, 31, 22 };
    mergeSort(arr, 0, arr.size() - 1);

    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << " ";
    }
    cout << "\n";
}
```


### python 코드
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    merged = list()
    left_point, right_point = 0, 0

    # case1: left/right 아직 남아있을 때
    while len(left) > left_point and len(right) > right_point:
        if left[left_point] > right[right_point]:
            merged.append(right[right_point])
            right_point += 1
        else:
            merged.append(left[left_point])
            left_point += 1

    # case2: left만 남아있을 때
    while len(left) > left_point:
        merged.append(left[left_point])
        left_point += 1

    # case3: right만 남아있을 때
    while len(right) > right_point:
        merged.append(right[right_point])
        right_point += 1

    return merged

arr = [69, 10, 30, 2, 16, 8, 31, 22]
print(merge_sort(arr))
```

### java 코드
```java
public class MergeSort {
    public static void main(String[] args) {
        int[] arr = { 7, 6, 5, 8, 3, 5, 9, 1 };
        mergeSort(arr, 0, arr.length - 1);
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }

    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }

    public static void merge(int[] arr, int left, int mid, int right) {
        int i = left;
        int j = mid + 1;
        int k = left;
        int[] sorted = new int[arr.length];

        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                sorted[k] = arr[i];
                i++;
            } else {
                sorted[k] = arr[j];
                j++;
            }
            k++;
        }

        if (i > mid) {
            for (int t = j; t <= right; t++) {
                sorted[k] = arr[t];
                k++;
            }
        } else {
            for (int t = i; t <= mid; t++) {
                sorted[k] = arr[t];
                k++;
            }
        }

        for (int t = left; t <= right; t++) {
            arr[t] = sorted[t];
        }
    }
}
```
