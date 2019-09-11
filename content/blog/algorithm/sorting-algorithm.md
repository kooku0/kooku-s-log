---
title: Sorting Algorithm
date: 2019-09-11 22:09:19
category: algorithm
---

sorting algorithm들을 정리 해보려고 합니다.

실제 코드를 짤때는 그냥 라이브러리를 사용하겠지만 적어도 어떤 정렬 알고리즘들이 있는지, 어떤 장단점이 있는지 그리고 코드로 구현할 수 있어야합니다.

모든 알고리즘을 정리할 수는 없겠지만 [정렬 알고리즘::나무위키](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)에 있는 내용을 기반으로 많이 사용되는 알고리즘을 정리하겠습니다.

## Overview

데이터를 정렬해야 하는 이유는 탐색을 위해서입니다. 탐색할 대상 데이터가 정렬되어 있지 않다면 순차 탐색 이외에 다른 알고리즘을 사용할 수는 없겠지만 **데이터가 정렬되어 있다면 [이진 탐색](https://namu.wiki/w/%EC%9D%B4%EC%A7%84%20%ED%83%90%EC%83%89)이라는 강력한 알고리즘을 사용할 수 있습니다.** 삽입과 삭제가 자주 되는 자료의 경우 정렬에 더 많은 시간이 들어가므로 순차 탐색을 하는 경우도 있지만 대부분의 경우 삽입/삭제보다는 데이터를 조회하는 것이 압도적으로 많고 조회에 필요한 것이 바로 검색입니다.

## 1. 버블 정렬 (Bubble Sort)

1번째와 2번째 원소를 비교하여 정렬하고, 2번째와 3번째, ..., n-1번째와 n번째를 정렬한 뒤 다시 처음으로 돌아가 이번에는 n-2번째와 n-1번째까지, ...해서 최대 n(n - 1) / 2번 정렬합니다.

버블 정렬은 거의 모든 상황에서 최악의 성능을 보여줍니다. 단, 이미 정렬된 자료에서는 1번만 돌면 되기 때문에 최선의 성능을 보여줍니다.

가장 손쉽게 구현하여 사용할 수 있지만, 만들기가 쉽고 직관적일 뿐이지 알고리즘적 관점에 보면 대단히 비효율적인 정렬방식입니다.

### 시간복잡도

O(n^2)

### 구현 코드

```c++
void bubleSort(int *randomList, int listSize) {
	for (int i = listSize - 1; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			if (randomList[j] > randomList[j + 1]) {
				int tmp = randomList[j];
				randomList[j] = randomList[j + 1];
				randomList[j + 1] = tmp;
			}
		}
	}
}
```



## 2. 선택 정렬 (Selection Sort)

버블 정렬이 비교하고 바로 바꿔 넣는 걸 반복한다면 이쪽은 일단 1번째부터 끝까지 훑어서 가장 작은 게 1번째, 2번째부터 끝까지 훑어서 가장 작은 게 2번째……해서 (n-1)번 반복합니다.

어찌 보면 인간이 사용하는 정렬 방식을 가장 많이 닮았습니다. 어떻게 정렬이 되어 있든 일관성 있게 n(n - 1) / 2 에 비례하는 시간이 걸린다는게 특징입니다. 또한, 버블 정렬보다 두 배 정도 빠릅니다.

### 시간복잡도

O(n^2)

### 구현 코드

```c++
void selectionSort(int *randomList, int listSize) {
	for (int i = 0; i < listSize - 1; i++) {
		int minNumIdx = i;
		for (int j = i; j < listSize; j++) {
			if (randomList[minNumIdx] > randomList[j]) {
				minNumIdx = j;
			}
		}
		int tmp = randomList[minNumIdx];
		randomList[minNumIdx] = randomList[i];
		randomList[i] = tmp;
	}
}
```

### 3. 삽입 정렬 (Insertion Sort)

![íì¼:external/upload.wikimedia.org/Insertion-sort-example-300px.gif](https://w.namu.la/s/e2cca975b1e03bd676ae5e11433526429e9cf77953039ca19a2df4b1112eb75c9c45701ca4f75bcb78194f07ec7b60f28040a4bae7ceed58729887ff62fc13f641682dd0a76feac03e643811437a0c40f3c53b338e965e5dd6c271d7a4064bdf)

k번째 원소를 1부터 k-1까지와 비교해 적절한 위치에 끼워넣고 그 뒤의 자료를 한 칸씩 뒤로 밀어내는 방식으로, 평균적으로 O(n^2)중 빠른 편이나 자료구조에 따라선 뒤로 밀어내는데 걸리는 시간이 크며, 앞의 예시처럼 작은 게 뒤쪽에 몰려있으면 그야말로 헬게이트입니다.

그밖에도 배열이 작을 경우에 역시 상당히 효율적인데요. 일반적으로 빠르다고 알려진 알고리즘들도 배열이 작을 경우에는 대부분 삽입 정렬에 밀립니다.

 따라서 고성능 알고리즘들 중에서는 배열의 사이즈가 클때는 *O*(*n*log*n*) 알고리즘을 쓰다가 정렬해야 할 부분이 작을때 는 삽입 정렬로 전환하는 것들도 있습니다.

### 시간복잡도

O(n^2)

### 구현 코드

```c++
void insertionSort(int *randomList, int listSize) {
  
}
```



## 4. 합병 정렬 (Merge Sort)

![íì¼:external/upload.wikimedia.org/Merge-sort-example-300px.gif](https://ww.namu.la/s/30bb5bb955f72d8a4b70c88e0cb83fe97ae0c349bd9c27d1204e8939df903ef7748c25b1928455ad76d70fd7a283b1c131feecabca2fe5a9c36b4ab72fe3e778320db817cf709f625c4132640aee1d47aca18f0bd40ac09a7f95c78db18c05b4)

개발자는 [존 폰 노이만](https://namu.wiki/w/%EC%A1%B4%20%ED%8F%B0%20%EB%85%B8%EC%9D%B4%EB%A7%8C)으로 원소 개수가 1 또는 0이 될 때까지 두 부분으로 쪼개고 쪼개서 자른 순서의 역순으로 크기를 비교해 병합해 나간다. 병합된 부분 안은 이미 정렬되어 있으므로 전부 비교하지 않아도 제자리를 찾을 수 있다. 대표적인 분할 정복 알고리즘으로 존 폰 노이만의 천재성을 엿볼 수 있는 알고리즘이다.

성능은 아래의 퀵 정렬보다 전반적으로 뒤떨어지고, 데이터 크기만한 메모리가 더 필요하지만 최대의 장점은 데이터의 상태에 별 영향을 받지 않는다는 점이다. 힙이나 퀵의 경우에는 배열 `A[25]=100`, `A[33]=100`인 정수형 배열을 정렬한다고 할 때, 33번째에 있던 100이 25번째에 있던 100보다 앞으로 오는 경우가 생길 수 있다. 그에 반해서 병합정렬은 그런 거 없다.정렬되어 있는 두 배열을 합집합할 때 이 알고리즘의 마지막 단계만을 이용하면 가장 빠르게 정렬된 상태로 합칠 수 있다.

## 5. 힙 정렬 (Heap Sort)

## 6. 퀵 정렬 (Quick Sort)

## 7. 카운팅 정렬 (Counting Sort)

## 8. 쉘 정렬 (Shell Sort)

### reference

* [정렬 알고리즘::나무위키](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

