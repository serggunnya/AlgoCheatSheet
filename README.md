# AlgoCheatSheet
`(Шпаргалка по алгоритмам для подготовки к собеседованию)`

- [Prefix sum](#prefix-sum)
- [Bucket sort](#bucket-sort)
- [Hashmap](#hashmap)
- [Pointers](#pointers)
  - [Move zeroes](#move-zeroes)
  - [Boats to save people](#boats-to-save-people)
  - [Valid Palindrome II](#valid-palindrome-ii)
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
### [`Boats to save people`](https://leetcode.com/problems/boats-to-save-people)
Задача: Переместить (парами) людей на лодки в соответствии с лимитом общего веса для лодки. 

```javascript
[3,5,2,3] // исходный массив людей разного веса
limit = 5  // лимит веса для лодки.

/** 
 *     L        R    |  5+2 = 8; 
 *   [ 5, 3, 3, 2 ]  |  boats++, left++;
 *                   |  
 *        L     R    |  3+2 = 5; 
 *   [ 5, 3, 3, 2 ]  |  right++, boats++, left++; 
 *                   |  
 *           LR      |  3+3 = 6; 
 *   [ 5, 3, 3, 2 ]  |  boats++, break;
 * 
 *   Необходимо отсортировать массив.
 *   Указатели на концах массива
 *   пока указатели не встретятся считать лодки и двигаем левый указатель вправо
 *   если сумма весов левого и правого не превышает лимита,
 *     сдвигаем правый указатель влево (уменьшаем).
 */

var numRescueBoats = function (people, limit) {
  people.sort((a, b) => b - a);

  var left = 0, right = people.length - 1, boats = 0;
  while (left <= right) {
    if (people[left] + people[right] <= limit) right--;
    boats++;
    left++
  }
  return boats;
};

```

### [`Valid Palindrome II`](https://leetcode.com/problems/Valid-Palindrome-ii)
 > Задача: вернуть true/false, если строка может стать палиндромом после удаления одного символа.

```javascript
[a,b,c,a] // исходная строка

/** 
 *     L           R                           |  L === R
 *   [ a, b, c, c, a ]                         |  left++; right++
 *                                             |  
 *        L     R                              |  L !== R
 *   [ a, b, c, c, a ]                         |  [ a, _, c, c, a ]  ||  [ a, b, c, _, a ]
 *                                             |   
 *                                             |  
 *           L  R                L  R          |  
 *   [ a, _, c, c, a ]  ||  [ a, b, c, _, a ]  |   
 *          L == R      ||      L !== R        |    
 *           True                False         |  
 *               \                             |
 *                return true                  |
 *                                             |
 *   Указатели находятся на краницах строки
 *   пока левый указательи меньше правого
 *      если левый сивол не равен правому пробуем (удалить) пропустить один из них.
 *          isPalindrome(left + 1, right) || isPalindrome(left, right - 1)
 *            в каждом вызове двигаем указатели пока они равны.
 *          из каждого вызова возращаем результат провперки  return true/false || true/false
 *      иначе возвращаем true  
 *
 */

const validPalindrome = (s) => {
    const isPalindrome = (left,right) => {
        while(left < right){            
           if(s[left++] !== s[right--]) return false;
        }
        return true;
    };

    let left = 0;
    let right = s.length-1;
    while(left < right){
        if(s[left] != s[right]){
            return isPalindrome(left+1,right) || isPalindrome(left,right-1);
        }        
        left++; 
        right--;
    }
    return true;
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
