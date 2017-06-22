# 병합정렬

목표: 오름차순 또는 내림차순으로 배열을 정렬하는 것.

 1945년  John von Neumann이 개발했다. 병합정렬은 최고, 최악 그리고, 평균**O(n log n)**의 시간 복잡도를 가진 효율적인 알고리즘이다.

병합정렬 알고리즘은 큰문제를 작은문제로 분할하여 해결하는 분할 정복 알고리즘을 사용한다. 즉, 병합정렬 알고리즘은 **분할**한 후 **병합**하는 것이다. 

n개의 숫자의 배열을 순서대로 정렬 한다고 가정하자. 
병합정렬 알고리즘은 아래와 같이 동작한다:

- 정렬되지 않은 더미에 숫자들을 입력한다.
- 더미를 2개로 나눈다.  ( **2개의 정렬되지 않은 숫자 더미**가 있다.)
- 더이상 나눌 수 없을 때까지 더미를 계속 나눈다. 마지막에는 각 더미에 숫자 한개만 남게 된다.
- 순서대로 짝을 지어 더미들을 결합하여 **병합**한다.  병합할 때 마다, 정렬 순서에 따라 숫자들을 넣는다. 이 방법은 각각의 더미들이 이미 정렬 되어 있기 때문에 쉽게 이용할 수 있는 방법이다.

## 예제

### 분할

 *n* 개의 숫자를 가진 배열 `[2, 1, 5, 4, 9]`이 있다고 하자. 이 배열은 정렬되지 않은 배열이다. 더미를 더 이상 나눌 수  없을 때까지 더미를  계속 나눈다.

먼저, 배열을 2개의 배열로 나눈다: `[2, 1]`  그리고  `[5, 4, 9]`.

좌측 배열 `[2, 1]`을  `[2] `와`[1]`로 나눈다.

우측 배열  `[5, 4, 9]` 을 `[5]` 그리고  `[4, 9]`로 나눈다. 배열 `[5]`는 더 이상 나눌 수 없는 배열이다. 하지만,  배열`[4, 9]` 는  `[4]`그리고 `[9]`로 한번 더 나눌 수 있다.

분할프로세스가 끝나면 아래와 같은 더미들이 남는다: `[2]` `[1]` `[5]` `[4]` `[9]`. 각 더미는 하나의 원소로 구성된다.

### 병합

이제 앞서서 분할한 배열을 갖고서 각각의 더미들을 결합하여 병합할 수 있다. 여기서 기억해야 할 것은 병합정렬은 큰 문제 하나가 아니라 여러개의 작은 문제들을 해결해 나가는 전략이라는 것이다. 병합을 반복할 때 마다 중요한 것은 하나의 더미를 다른 더미에 병합한다라는 개념이다.



여기  `[2]` `[1]` `[5]` `[4]` `[9]` 가 있다. 1단계로  `[2]`과 `[1]`을 비교 병합 `[5]` 와`[4]` 을 비교 병합한다. 그 결과  `[1, 2]` 과 `[4, 5]`그리고 `[9]`가 된다.   `[9]` 는 하나만 남았기 때문에 , 1단계에서는 어떤 더미와도 병합할 수 없다.

다음 단계로 `[1, 2]`과 `[4, 5]` 을 병합한다. 그 결과 `[1, 2, 4, 5]` , 또다시`[9]` 만 남았다.



 이제 2개의 더미만 남았고(`[1, 2, 4, 5]`그리고`[9]` ),  합병을 하면  결과 배열로`[1, 2, 4, 5, 9]`라는 정렬된 배열이 된다.

## Top-down 구현

Swift로 구현한 병합정렬:

```swift
func mergeSort(_ array: [Int]) -> [Int] {
  guard array.count > 1 else { return array }    // 1

  let middleIndex = array.count / 2              // 2

  let leftArray = mergeSort(Array(array[0..<middleIndex]))             // 3

  let rightArray = mergeSort(Array(array[middleIndex..<array.count]))  // 4

  return merge(leftPile: leftArray, rightPile: rightArray)             // 5
}
```

코드가 어떻게 동작하는지 단계별로 설명하겠다 :

1. 만약 배열이 비어있거나, 원소가 하나 밖에 들어 있지 않다면, 더 이상 더 작은 단위로 나눌 수 없기 때문에 반드시 배열을 return 해야 한다.
2. middle index를 찾는다.
3. 이전 단계에서의 middle index를 사용함으로서 좌측 배열(middle index 보다 작은 index를 가진)을 재귀적으로 나눌 수 있다.
4. 마찬가지로, 우측 배열(middle index 보다 같거나 큰 index를 가진)을 재귀적으로 나눌 수 있다. 
5. Finally, merge all the values together, making sure that it is always sorted. 마지막으로, 모든 값들을 병합하고,  모든 값이 정렬되게 한다.

합병 알고리즘:

```swift
func merge(leftPile: [Int], rightPile: [Int]) -> [Int] {
  // 1
  var leftIndex = 0
  var rightIndex = 0

  // 2 
  var orderedPile = [Int]()

  // 3
  while leftIndex < leftPile.count && rightIndex < rightPile.count {
    if leftPile[leftIndex] < rightPile[rightIndex] {
      orderedPile.append(leftPile[leftIndex])
      leftIndex += 1
    } else if leftPile[leftIndex] > rightPile[rightIndex] {
      orderedPile.append(rightPile[rightIndex])
      rightIndex += 1
    } else {
      orderedPile.append(leftPile[leftIndex])
      leftIndex += 1
      orderedPile.append(rightPile[rightIndex])
      rightIndex += 1
    }
  }

  // 4
  while leftIndex < leftPile.count {
    orderedPile.append(leftPile[leftIndex])
    leftIndex += 1
  }

  while rightIndex < rightPile.count {
    orderedPile.append(rightPile[rightIndex])
    rightIndex += 1
  }

  return orderedPile
}
```

이 방법이 복잡해보일 수 있다. 하지만 아주 단순하다:

1. 합병 할 때, 두 배열의 진행상황을 확인하기 위해서 2개의 index가 필요하다.
2. This is the merged array. It is empty right now, but you will build it up in subsequent steps by appending elements from the other arrays. 이 변수가 합병된 배열을 나타낸다. 지금은 배열이 비어있지만, 다른 배열에 있는 원소들을 순서대로 넣어서 완성 될 것이다.
3. This while-loop will compare the elements from the left and right sides and append them into the  while making sure that the result stays in order. 이 while-loop는 좌측과 우측에 있는 배열의 원소들을 비교해 줄 것이다. 그리고 결과값이 순서대로 정렬되면`orderedPile` 배열에 넣어 줄 것이다. 
4. If control exits from the previous while-loop, it means that either the `leftPile` or the `rightPile` has its contents completely merged into the `orderedPile`. At this point, you no longer need to do comparisons. Just append the rest of the contents of the other array until there is no more to append. 만약 앞에 있는 while-loop에서 빠져 나왔다면, 이것은 `leftPile` 또는 `rightPile`의 원소들이 완전히 `orderedPile`에 병합되었다는 의미이다. 여기서 중요한 것은, 더이상 비교를 하지 않아도 된다는 것이다.  배열에 남아있는 원소들을 더 이상 넣을 수 없을 때까지 넣으면 된다.  

As an example of how `merge()` works, suppose that we have the following piles: `leftPile = [1, 7, 8]` and `rightPile = [3, 6, 9]`. Note that each of these piles is individually sorted already -- that is always true with merge sort. These are merged into one larger sorted pile in the following steps:

```
leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ ]
  l              r

```

The left index, here represented as `l`, points at the first item from the left pile, `1`. The right index, `r`, points at `3`. Therefore, the first item we add to `orderedPile` is `1`. We also move the left index `l` to the next item.

```
leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1 ]
  -->l           r

```

Now `l` points at `7` but `r` is still at `3`. We add the smallest item to the ordered pile, so that is `3`. The situation is now:

```
leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3 ]
     l           -->r

```

This process repeats. At each step, we pick the smallest item from either the `leftPile` or the `rightPile` and add the item into the `orderedPile`:

```
leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6 ]
     l              -->r

leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6, 7 ]
     -->l              r

leftPile       rightPile       orderedPile
[ 1, 7, 8 ]    [ 3, 6, 9 ]     [ 1, 3, 6, 7, 8 ]
        -->l           r

```

Now, there are no more items in the left pile. We simply add the remaining items from the right pile, and we are done. The merged pile is `[ 1, 3, 6, 7, 8, 9 ]`.

Notice that, this algorithm is very simple: it moves from left-to-right through the two piles and at every step picks the smallest item. This works because we guarantee that each of the piles is already sorted.