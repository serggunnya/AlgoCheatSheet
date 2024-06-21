# AlgoCheatSheet
`(Шпаргалка по алгоритмам для подготовки к собеседованию)`

- [Prefix sum](#prefix-sum)
- [Bucket sort](#bucket-sort)
- [Hashmap](#hashmap)
- [Pointers](#pointers)
  - [Move zeroes](#move-zeroes)
  - [Boats to save people](#boats-to-save-people)
  - [Valid Palindrome II](#valid-palindrome-ii)
  - [Two sum II input array is sorted`](#two-sum-ii-input-array-is-sorted)
  - [Remove Duplicates II`](#remove-duplicates-ii)
- [Sliding window](#sliding-window)
- [Stack](#stack)  
   - [Backspace String Compare](#backspace-string-compare)
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
> Задача: Переместить нули в конец массива, сохраняя порядок остальных цифр
> 
> [0,1,0,3,12] - исходный массив

```javascript

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
> Задача: Переместить (парами) людей на лодки в соответствии с лимитом общего веса для лодки.
> 
> [3,5,2,3]  исходный массив людей разного веса
> 
> limit = 5  лимит веса для лодки.

```javascript
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
> 
> "abca" - исходная строка

```javascript

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
 *   Указатели находятся на границах строки
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

### [`Two sum II input array is sorted`](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted)
> Задача: Вернуть индексы двух элементов в отсортированом массиве, чья сума равно заданому числу
> 
> [1,2,7,11,15] - исходный массив
> 
> target = 9 - искомая сумма

```javascript
/** 
 *     L             R     |
 *   [ 1, 2, 7, 11, 15 ]   |
 *                         |
 *     L        R          |
 *   [ 1, 2, 7, 11, 15 ]   |
 *                         |
 *     L     R             |
 *   [ 1, 2, 7, 11, 15 ]   |
 *                         |
 *        L  R             |
 *   [ 1, 2, 7, 11, 15 ]   |
 *                         |
 *   Указатели находятся на границах массива
 *   Если сумма левого и правого элемента равно target
 *      вернуть ответ 
 *   Если сумма левого и правого элемента больше target
 *      правый указатель сдвигается влево (r--)
 *      иначе левый сдвигается враво (l++)
 *
 */

var twoSum = function(nums, target) {
   var l = 0, r = nums.length-1;    
   while(l < r){
    if(nums[l] + nums[r] === target) return [l+1,r+1];    
    else if(nums[l] + nums[r] > target) r--;
    else l++;
   }
   return [];
};

```

### [`Remove Duplicates II`](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)
> Задача: Оставить не более двух дубликатов 
> 
> [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ] - исходный массив
> 

```javascript
/** 
 *    LR                             |  cnt = 1
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt++;
 *                                   |  nums[L] = nums[R]
 *                                   |  R++, L++, cnt++;
 *                                   |  
 *       LR                          |  cnt = 2
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R !== R + 1  ----> cnt = 1;
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *                                   |
 *           LR                      |  cnt = 1
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt++;
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *             LR                    |  cnt = 2
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt++;
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++
 *                                   |
 *                                   |
 *              L  R                 |  cnt = 3
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt++;
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++,
 *                                   |
 *                                   |
 *              L     R              |  cnt = 4
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt = 1 
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *                 L     R           |  cnt = 1
 *   [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt = 1 
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *                    L     R        |  cnt = 1
 *   [ 0, 0, 1, 1, 2, 1, 2, 3, 3 ]   |  R === R + 1  ----> cnt++
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *                       L     R     |  cnt = 2
 *   [ 0, 0, 1, 1, 2, 3, 2, 3, 3 ]   |  R === R + 1  ----> cnt = 1 
 *                                   |  nums[L] = nums[R] 
 *                                   |  R++, L++, 
 *                                   |
 *                          L  R     |  
 *   [ 0, 0, 1, 1, 2, 3, 3, 3, 3 ]   |  break;
 *                                   |  return L (7)
 *                                   |  
 *
 *   
 *   
 *   
 *   
 *   
 */

function removeDuplicates(nums) {
    var l = 0, cnt = 1;
    for (var r = 0; r < nums.length; r++) {
        if (nums[r] === nums[r + 1]) cnt++;
        else cnt = 1;

        if (cnt <= 2) {
            nums[l] = nums[r]
            l++;
        }
    };
    return l
}

```

## Sliding window

## Stack
### [`Backspace String Compare`](https://leetcode.com/problems/backspacce-string-compare)
 > Задача: вернуть true/false, если строки совпадают после backspace удаления симоволов (отмечено как #),
 >
 > строки s = "xwrmp" , t = "xwrmu#p"
 
```js

/** 
 *    i           |  stackA []
 *   "xwrmp"      |  stackB []   
 *   "xwrmu#p"    |  
 *                |  
 *     i          |  stackA ["x"]   
 *   "xwrmp"      |  stackB ["x"] 
 *   "xwrmu#p"    |
 *                |
 *      i         |  stackA ["x", "w"]
 *   "xwrmp"      |  stackB ["x", "w"]
 *   "xwrmu#p"    |
 *                |
 *       i        |  stackA ["x", "w", "r"]
 *   "xwrmp"      |  stackB ["x", "w", "r"] 
 *   "xwrmu#p"    |
 *                |
 *        i       |  stackA ["x", "w", "r", "m" ]
 *   "xwrmp"      |  stackB ["x", "w", "r", "m" ]  
 *   "xwrmu#p"    |
 *                |
*          i      |  stackA ["x", "w", "r", "m","p" ]
 *   "xwrmp"      |  stackB ["x", "w", "r", "m","u" ]  
 *   "xwrmu#p"    |
 *                |
 *          i     |  stackA ["x", "w", "r", "m","p" ]
 *   "xwrmp"      |  stackB ["x", "w", "r", "m","p" ]  
 *   "xwrmu#p"    |
 *                |
 * 
 *   полняются два стека
 *   eсли симол === "#" , то извлекаем из стека последний
 *   иначе добавляем текущий символ (или пустой)
 */


var backspaceCompare = function(s, t) {
    var len = s.length >= t.length ? s.length: t.length;
    var stackA = [], stackB = [];

    for(var i = 0; i < len; i++ ){
      if(s[i] === "#" ) stackA.pop()
      else stackA.push(s[i] || "") 
      
      if(t[i] === "#" ) stackB.pop()
      else stackB.push(t[i] || "")       
    }
    return stackA.join("") === stackB.join("")
};
```

## Monotonic stack

## Math

## Knapsack

## Backtracking

## BFS

## DFS
