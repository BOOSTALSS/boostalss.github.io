---
title: Search & Sorting
layout: post
author: nureesong
categories:
- algorithms
tags:
- sort
- python
---

### 어떤 배열이 주어졌을 때, 그 배열이 미리 정렬되어 있다면 훨씬 빠른 속도로 검색이 가능합니다.


## 1) 검색

- **Linear search: O(n)**
자료가 정렬되어 있지 않거나 정보가 없어 하나씩 찾아야 하는 경우에 유용.
- **Binary search: O(log n)**  
배열이 정렬되어 있을 때 유용.


## 2) 정렬

### Summary

<img width="495" alt="_2021-06-24__2 39 37" src="https://user-images.githubusercontent.com/68745983/124866645-b914db00-dff7-11eb-8192-d97f02c32dec.png">
<img width="322" alt="_2021-06-24__2 39 49" src="https://user-images.githubusercontent.com/68745983/124866647-b914db00-dff7-11eb-9c14-e5cc3c71c380.png">


### - Bubble sort: O(n^2)
- best 시간복잡도: O(n)  완전 정렬된 경우 교환이 일어나지 않음.
- **두 개의 인접한 자료 값을 비교하면서 위치를 교환하는 방식으로 정렬**하는 방법을 말합니다.
  버블 정렬은 단 두 개의 요소만 정렬해주는 좁은 범위의 정렬에 집중합니다.
- 이 접근법은 간단하지만 단 하나의 요소를 정렬하기 위해 너무 많이 교환하는 낭비가 발생할 수도 있습니다.
- n개의 원소에 대해서 버블 정렬을 한번 수행할 때마다 n번째 원소가 제자리를 찾게 됩니다.
  최댓값부터 완성

<img width="405" alt="_2021-06-24__9 42 12" src="https://user-images.githubusercontent.com/68745983/124866660-bade9e80-dff7-11eb-9136-838ba3623554.png">

```python
def bubble_sort(data):
	for i in range(len(data)):
		swap = 0
  	for j in range(len(data)-1 -i):
			if data[j] > data[j+1]:
				data[j], data[j+1] = data[j+1], data[j]
				swap += 1
		if swap == 0:	break
	return data
```


### - Selection sort: O(n^2)
배열 안의 자료 중 가장 작은 수(혹은 가장 큰 수)를 찾아 첫 번째 위치(혹은 가장 마지막 위치)의 수와 교환해주는 방식의 정렬입니다.
**선택 정렬**은 **교환 횟수를 최소화**하는 반면 각 자료를 비교하는 횟수는 증가합니다.

<img width="417" alt="_2021-06-24__1 20 01" src="https://user-images.githubusercontent.com/68745983/124866644-b74b1780-dff7-11eb-84c5-79fabe5b30b1.png">

```python
def selection_sort(data):
  for i in range(len(data)-1):
    for j in range(i+1,len(data)):
      if data[j] < data[i]:
        data[i], data[j] = data[j], data[i]
    return data
```


### - Insertion sort: O(n^2)
처음 key값은 두 번째 인덱스부터 시작합니다. **앞쪽 자료들과 비교하여 삽입할 위치를 찾고** 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시키는 정렬 방법입니다.

<img width="248" alt="_2021-06-24__3 00 55" src="https://user-images.githubusercontent.com/68745983/124866652-b9ad7180-dff7-11eb-8e85-c4b20eef8ec3.png">

① key=6 vs 두 번째 값 8
     → 8을 한 칸 뒤로 이동 

② key=6 vs 첫 번째 값 5
     → 5는 그대로, 6을 두 번째 자리에 삽입

```python
def insertion_sort(data):
  for i in range(1,len(data)):
    key = data[i]
    for j in range(i):
      if data[i-j-1] > key:
        data[i-j], data[i-j-1] = data[i-j-1], key
  return data
```


### - Merge sort: O(nlogn)     `재귀 활용` 
원소가 한 개가 될 때까지 **계속해서 반으로 나누다가 다시 합쳐나가며 정렬**을 하는 방식입니다.
<img width="441" alt="_2021-06-24__4 27 49" src="https://user-images.githubusercontent.com/68745983/124866655-ba460800-dff7-11eb-91a6-8da1850bd5d5.png">
<img width="617" alt="_2021-06-24__4 27 18" src="https://user-images.githubusercontent.com/68745983/124866653-b9ad7180-dff7-11eb-9d5c-c720aeef9430.png">

```python
def merge_sort(data):
	n = len(data)
  if n <= 1:
    return data
  left  = merge_sort(data[:n//2])
  right = merge_sort(data[n//2:])
  return merge(left, right)

def merge(left, right):
# input: already sorted or single element
  merged = []
  while len(left)*len(right) > 0:
    # Case 1: both nonempty
    if (len(left) > 0) and (len(right) > 0):
      if left[0] <= right[0]:
        merged.append(left[0])
        left = left[1:]
      else:
        merged.append(right[0])
        right = right[1:]

    # Case 2: left is empty
    elif len(left) == 0:
      merged.append(right[0])
      right = right[1:]

    # Case 3: right is empty
    elif len(right) == 0:
      merged.append(left[0])
      left = left[1:]
	
	return merged
```


### **Quick sort: O(nlogn)**   `재귀 활용` 
리스트 가운데서 `pivot` 을 선택하여, **피벗 앞에는 작은 값, 뒤에는 큰 값이 오도록 리스트를 둘로 분할**합니다. 분할된 두 개 리스트에 재귀적으로 이 과정을 반복합니다.

<img width="391" alt="_2021-06-24__5 12 49" src="https://user-images.githubusercontent.com/68745983/124866657-ba460800-dff7-11eb-9a29-7876f3b757d8.png">

              Average case: O(nlogn)   
							
<img width="277" alt="_2021-06-24__5 12 57" src="https://user-images.githubusercontent.com/68745983/124866659-bade9e80-dff7-11eb-93e5-6dcb36766a22.png">

              Worst case: O(n^2)

```python
def quick_sort(data):
    n = len(data)
    if n <= 1:
        return data	
    
    pivot = data[0]  # worst O(n^2)
    # pivot = data[n//2]  # average O(n*logn)
    left, right = [], []
    for i in range(1,n):
        if data[i] < pivot:
            left.append(data[i])
        else:
            right.append(data[i])
    return quick_sort(left) + [pivot] + quick_sort(right)
```
