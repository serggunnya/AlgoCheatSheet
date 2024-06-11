# AlgoCheatSheet
`(Шпаргалка по алгоритмам для подготовки к собеседованию)`

- [Prefix sum](#prefix-sum)
- [Bucket sort](#bucket-sort)
- [Hashmap](#hashmap)
- [Pointers](#pointers)
  - [Move zeroes](#move-zeroes)
- [Sliding window](#sliding-window)
- [Stack](#stack)
- [Monotonic stack](#monotonic-stack)
- [Math](#math)
- [Knapsack](#knapsack)
- [Backtracking](#backtracking)
- [BFS](#bfs)
- [DFS](#dfs)

## Prefix sum


## Bucket sort

## Hashmap  

## Pointers

### [`Move zeroes`](https://leetcode.com/problems/move-zeroes/)
Задача: Переместить нули в конец массива, сохраняя порядок остальных цифр

```javascript
[0,1,0,3,12] // исходный массив

/** 
 *    LR
 *   [ 0, 1, 0, 3, 12 ] 
 *
 *     L  R
 *   [ 0, 1, 0, 3, 12 ] 
 *
 *        L  R
 *   [ 1, 0, 0, 3, 12 ] 
 *
 *        L     R
 *   [ 1, 0, 0, 3, 12 ] 
 *
 *           L  R
 *   [ 1, 3, 0, 0, 12 ] 
 *
 *           L     R
 *   [ 1, 3, 0, 0, 12 ] 
 *
 *               L  R
 *   [ 1, 3, 12, 0, 0 ] 
 * 
 *   Правый указатель идет вперед пока элемент ноль.
 *   Левый указатель ждет пока правый не встретит число. 
 */

let moveZeroes = function (nums) {
  let  left = 0, right = 0;

  while (right < nums.length) {
    if (nums[right] !== 0) {
      let temp = nums[left];
      nums[left] = nums[right]
      nums[right] = temp
      left++;
    }
    right++;
  }
};
```

## Sliding window

## Stack

## Monotonic stack

## Math

## Knapsack

## Backtracking

## BFS

## DFS
