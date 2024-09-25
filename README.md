# AlgoCheatSheet
`(Шпаргалка по алгоритмам для подготовки к собеседованию)`

- [Hash & Base](#hash-&-base)
  - [Monotonic array](#monotonic-array)
  - [largest substring between two equal characters](#largest-substring-between-two-equal-characters/description/)
- [Prefix sum](#prefix-sum)
- [Bucket sort](#bucket-sort)
- [Pointers](#pointers)
  - [Move zeroes](#move-zeroes)
  - [Boats to save people](#boats-to-save-people)
  - [Valid Palindrome II](#valid-palindrome-ii)
  - [Two sum II input array is sorted`](#two-sum-ii-input-array-is-sorted)
  - [Remove Duplicates II`](#remove-duplicates-ii)
  - [Trapping Rain Water`](#trapping-rain-water)
- [Sliding window](#sliding-window)
- [Stack/Queue](#stack-queue)  
   - [Backspace String Compare](#backspace-string-compare)
   - [Duplicate Zeros](#duplicate-zeros)
- [Monotonic stack](#monotonic-stack)
- [Math](#math)
- [Knapsack](#knapsack)
- [Backtracking](#backtracking)
   - [Permutations](#permutations)
- [BFS](#bfs)
- [DFS](#dfs)

## Hash & Base
### [`Monotonic array`](https://leetcode.com/problems/monotonic-array)
> Задача: Определить упорядочины (нискодящие/восходящие) ли элементы массива (true) или хаотичны (false).
> 
> [5,3,2,4,1] - исходный массив

```javascript
/** 
 * 
 *   Необходимо завести два булевых флага asc и desc;
 *   и установить им значение на основе сравнения вервых двух элементов.
 *   При проходе по массиву выключаем флаги. Если элементы понижаются asc = false
 *   Если элементы повышаются desc = false
 */

var isMonotonic = function (nums) {
  if (nums.length === 1 ) return true;

  var asc = nums[0] <= nums[1];
  var desc = nums[0] >= nums[1];

  for (var i = 2; i < nums.length; i++) {
    if (nums[i-1] < nums[i]) {
      desc = false;
    }
    if (nums[i-1] > nums[i]) {
      asc = false;
    }
  }
  return asc || desc;
};
```

### [`largest substring between two equal characters`](https://leetcode.com/problems/largest-substring-between-two-equal-characters)
> Задача: Найти максимальную длину подстроки между двумя одинаковых символов
> 
> "abca" - исходная строка

```javascript
/** 
 * 
 *   Необходимо завести пустую hashmap и переменную ответа с базовым значением.
 *   Проходим по строке им заполняем hashmap пока уникальными символами с соответствующим индексом
 *   Если если символ уже есть в таблице, то мы перезаписываем ответ из максимума предыдущего значенияи
 *   и разницы текущего индекса - индекса первого парного символа - 1.
 */

var maxLengthBetweenEqualCharacters = function (s) {
  var map = {};
  var ans = -1;

  for (var i = 0; i < s.length; i++) {
    if (map[s[i]] === undefined) {
      map[s[i]] = i;
      continue;
    }
    ans = Math.max(ans, (i - 1) - map[s[i]]);
  }
  return ans;
};
```

## Prefix sum

## Bucket sort

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

    (nums[l] + nums[r] > target) ? r-- : l++;
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
### [`Trapping Rain Water`](https://leetcode.com/problems/trapping-rain-water)
> Задача: Дан массив высот блоков. Посчитать число заполненых водой ечеек.
> 
> [ 0, 0, 1, 1, 1, 1, 2, 3, 3 ] - исходный массив
> 

```javascript
/** 
 *      L                 R        | 5 < 6
 *     [5,5,1,7,1,1,5,2,7,6]       | lmax = 5
 *  lMax|                 |rMax    | emptyCell += lMax - nums[l] = 0; 
 *                                 | L++;
 *                                 | 
 *        L               R        | 5 < 6
 *     [5,5,1,7,1,1,5,2,7,6]       | lmax = 5
 *  lMax|                 |rMax    | emptyCell += lMax - nums[l] = 0; 
 *                                 | L++;
 *                                 | 
 *          L             R        | 1 < 6
 *     [5,5,1,7,1,1,5,2,7,6]       | lmax = 5
 *  lMax|                 |rMax    | emptyCell += lMax - nums[l] = 4; 
 *                                 | L++;
 *                                 | 
 *            L           R        | 7 > 6
 *     [5,5,1,7,1,1,5,2,7,6]       | rmax = 6
 *  lMax|                 |rMax    | emptyCell += rMax - nums[r] = 0; 
 *                                 | R++;
 *                                 | 
 *            L         R          | 7 === 7
 *     [5,5,1,7,1,1,5,2,7,6]       | lMax = 7
 *  lMax|                 |rMax    | emptyCell += lMax - nums[l] = 0; 
 *                                 | L++;
 *                                 | 
 *              L       R          | 1 < 7
 *     [5,5,1,7,1,1,5,2,7,6]       | lMax = 7
 *        lMax|           |rMax    | emptyCell += lMax - nums[l] = 10; 
 *                                 | L++;
 *                                 | 
 *                L     R          | 1 < 7
 *     [5,5,1,7,1,1,5,2,7,6]       | lMax = 7
 *        lMax|           |rMax    | emptyCell += lMax - nums[l] = 16; 
 *                                 | L++;
 *                                 | 
 *                  L   R          | 5 < 7
 *     [5,5,1,7,1,1,5,2,7,6]       | lMax = 7
 *        lMax|           |rMax    | emptyCell += lMax - nums[l] = 18; 
 *                                 | L++;
 *                                 | 
 *                    L R          | 2 < 7
 *     [5,5,1,7,1,1,5,2,7,6]       | lMax = 7
 *        lMax|           |rMax    | emptyCell += lMax - nums[l] = 23; 
 *                                 | L++;
 *                                 | 
 *                                 |
 *
 *   
 *   
 *   
 *   
 *   
 */

var trap = function (nums) {
    var l = 0, r = nums.length - 1;
    var leftMax = nums[l], rightMax = nums[r];    
    var emptyCell = 0;

    while (l < r) {
        if (nums[l] <= nums[r]) {
            leftMax = Math.max(leftMax, nums[l])
            emptyCell += leftMax - nums[l]; 
            l++;
        } else {
            rightMax = Math.max(rightMax, nums[r])
            emptyCell += rightMax - nums[r];
            r--;
        }
    }
    return emptyCell;
};

```

## Sliding window

## Stack Queue
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
 *   заполняются два стека
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
### [`Duplicate Zeros`](https://leetcode.com/problems/duplicate-zeros)
 > Задача: дублировать нули сохраняя порядок чисел и длину массива.
 > [1,0,2,3,0,4,5,0] 
 
```js

/** 
 *    i                  |  
 *   [1,0,2,3,0,4,5,0]   |  queue = [] 
 *                       |  
 *      i                |  
 *   [1,0,2,3,0,4,5,0]   |  queue = [0] 
 *                       |  
 *        i              |  
 *   [1,0,2,3,0,4,5,0]   |  queue = [0,2] 
 *   [1,0,0,3,0,4,5,0]   |  queue = [2] 
 *                       |  
 *          i            |  
 *   [1,0,0,3,0,4,5,0]   |  queue = [2,3] 
 *   [1,0,0,2,0,4,5,0]   |  queue = [3] 
 *                       |
 *            i          |  
 *   [1,0,0,2,0,4,5,0]   |  queue = [3,0] 
 *   [1,0,0,2,3,4,5,0]   |  queue = [0,0] 
 *                       |  
 *              i        |  
 *   [1,0,0,2,0,4,5,0]   |  queue = [0,0,4] 
 *   [1,0,0,2,3,0,5,0]   |  queue = [0,4] 
 *                       |  
 *                i      |  
 *   [1,0,0,2,3,0,5,0]   |  queue = [0,4] 
 *   [1,0,0,2,3,0,0,0]   |  queue = [4] 
 *                       |  
 *                  i    |  
 *   [1,0,0,2,3,0,0,0]   |  queue = [4] 
 *   [1,0,0,2,3,0,0,4]   |  queue = [] 
 * 
 *   Создаётся стэк (очередь)
 *   Если элемент равен нулю в стэк (очередь) добавляется ноль
 *   Если в стэк(очередь) не пуст
 *       текущий элемент добавляется в стэк(очередь)
 *       первый (левый) элемент стэка (очереди) измается и записывается вместо текущего num[i]
 */

var duplicateZeros = function(arr) {
    var stack = [];
    for(var i = 0; i < arr.length; i++){
        if(arr[i] === 0) stack.push(0);

        if(stack.length){
            stack.push(arr[i]);
            arr[i] = stack.shift();
        }
    }
};
```

## Monotonic stack

## Math

## Knapsack

## Backtracking
### [`Permutations`](https://leetcode.com/problems/permutations/)
 > Задача: Вернуть массив подмассивов со всеми перестановками чисел.
 > [1,2,3] 
 
```js

/** 
 *   
 *   backtrack - > (1),2,3          |  
 *   backtrack - - > *1,(2),3       |
 *   backtrack - - - > *1,*2,(3)    |  
 *   
 *   backtrack - > (1),2,3          |
 *   backtrack - - > *1,2,(3)       |
 *   backtrack - - - > *1,(2),*3    |  
 *   
 *   backtrack - > 1,(2),3          |
 *   backtrack - - > (1),2*,3       |  
 *   backtrack - - - > *1,*2,(3)    |  
 *   
 *   backtrack - > 1,(2),3          |
 *   backtrack - - > 1*,2*,(3)      |
 *   backtrack - - - > (1),2*,3*    |  
 *   
 *   backtrack - > 1,2,(3)          |
 *   backtrack - - > (1),2,3*       |
 *   backtrack - - - > 1*,(2),3*    |  
 *   
 *   backtrack - > 1,2,(3)          |
 *   backtrack - - > 1,(2),3*       |
 *   backtrack - - - > (1),2*,3*    |  
 *   [[1,2,3], [1,3.2], [2,1,3],[2,1,3], [3,1,2],[3,2,1]]
 *   
 *   Создаётся Set (набор) для записи использованных чисел
 *   если длина Set равна длине искодного массива, записать набор в ответ
 *
 *   запускается цикл 
 *   Если в наборе нет текущего числа, добавить число в набор и вызвать рекурсию 
 *   удалить число из набора.
 *     
 */

function permute(nums) {
  function backtrack(nums, used, result) {
    if (used.size === nums.length) {
      result.push(Array.from(used));
      return;
    }
    for (var n of nums) {
      if (!used.has(n)) {
        used.add(n);
        backtrack(nums, used, result)
        used.delete(n);
      }
    }
  }

  var result = [], used = new Set();
  backtrack(nums, used, result);
  return result;
};


```

## BFS

## DFS
