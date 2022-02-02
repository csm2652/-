# Heap
Heap은 우선순위 큐를 위해 만들어진 자료구조로, 모든 노드에 대해서 부모와 자식 간에 일정한 대소 관계가 성립하는 완전이진트리아다. 전체 자료를 정렬하는 것이 아니라 가장 큰 값만 몇 개 필요한 경우 주로 사용되며, MAX Heap, MIN Heap 등이 있다.

## 배열을 이용한 Heap 구현

Heap은 완전이진트리이므로 배열로 구현하면,

| Root 노드의 인덱스가 0으로 시작하는 경우 |
  1. 부모의 인덱스 = (자식의 인덱스-1) / 2
  2. 왼쪽 자식 인덱스 = (부모의 인덱스) * 2 + 1
  3. 오른쪽 자식 인덱스 = (부모의 인덱스) * 2 + 2

| Root 노드의 인덱스가 1로 시작하는 경우 |
  1. 부모의 인덱스 = (자식의 인덱스) / 2
  2. 왼쪽 자식 인덱스 = (부모의 인덱스) * 2
  3. 오른쪽 자식 인덱스 = (부모의 인덱스) * 2 + 1

## 시간복잡도 
Heap의 삽입(push)은 최악의 경우 Leaf 노드에서 Root 노드까지 값을 모두 비교하기 때문에 시간복잡도는 트리의 높이인 O(log2N)이 된다. 마찬가지로 Heap의 삭제(pop)도 최악의 경우에 Root 노드에서 Leaf 노드까지 값을 모두 비교하기 때문에 O(log2N)이 된다.


Heap은 아래 예제처럼 배열로 구현하는 것이 대부분이다. 그 이유는 연결리스트로 구현할 경우, 새로운 노드를 마지막 위치에 추가하는 것이 쉽지 않기 때문인데, O(1)의 시간복잡도로 바로 마지막 위치에 접근할 수 있는 배열이 연결리스트에 비해 실행 속도 측면에서 효율적이다.


다음은 배열을 이용하여 구현한 최소 Heap의 예제 코드이다.
(Heap 인덱스는 0부터)

```#include < stdio.h >
#include < stdlib.h >

#define MAX_SIZE 100

int heap[MAX_SIZE];
int heapSize = 0;

void heapInit(void) {
	heapSize = 0;
}

int heapPush(int value) {
	if (heapSize + 1 > MAX_SIZE) {
		printf("queue is full!");
		return 0;
	}

	heap[heapSize] = value; // 마지막 노드에 값 추가

	// 마지막 노드에 추가한 값을 올바른 위치로 옮긴다
	int current = heapSize; 
	while (current > 0 && heap[current] < heap[(current - 1) / 2]) { // 부모와 자식의 우선순위 비교
		// 자식의 우선순위가 더 높으면 부모와 값을 바꿔준다
		int temp = heap[(current - 1) / 2];
		heap[(current - 1) / 2] = heap[current];
		heap[current] = temp;
		current = (current - 1) / 2;
	}

	heapSize = heapSize + 1;
	return 1;
}

int heapPop() {
	if (heapSize <= 0)
		return -1;

	int value = heap[0];
	heapSize = heapSize - 1; 
	heap[0] = heap[heapSize]; // 마지막 data를 root에 저장

	// root에 저장된 값을 올바른 위치로 옮긴다
	int current = 0; 
	while (current * 2 + 1 < heapSize) {
		int child;
		if (current * 2 + 2 == heapSize)
			child = current * 2 + 1;
		else
			child = heap[current * 2 + 1] < heap[current * 2 + 2] ? current * 2 + 1 : current * 2 + 2;

		if (heap[current] < heap[child])
			break;

		int temp = heap[current];
		heap[current] = heap[child];
		heap[child] = temp;
		current = child;
	}
	return value;
}


void main() {
	srand(11);
	heapInit();

	for (int i = 0; i < 100; i++)
		heapPush(rand() % 100);

	for (int i = 0; i < 100; i++)
		printf("%d ", heapPop());
	printf("\n");
}
```