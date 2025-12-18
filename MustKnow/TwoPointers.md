# MustKnow / TwoPointers
`(Поиск пар, Сравнение пар, Перестановка пар)`

---
### [`Leetcode 167. Two Sum II`](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted).

> _Дан `отсортированый` массив __numbers__.
> 
> _Найти два числа, сумма которых равна __target__ и вернуть их индексы._

```js
twoSum([2,7,11,15], 9)        // [1,2]
twoSum([2,3,4], 6)            // [1,3]
twoSum([-1,0], -1)   // [1,2]
```
<details>
   <summary><h4>Идея</h4></summary>

1) Массив отсортирован, значит меньшее число слева, большее справа
2) Можно использовать два указателя на концах массива.
   - Если сумма `больше` __target__, уменьшаем правый указатель.
   - Если сумма `меньше` __target__, увеличиваем левый указатель.
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var twoSum = function (numbers, target) {
    var l = 0, r = numbers.length - 1;
    while (l < r) {
        if (numbers[l] + numbers[r] > target) {
            r--;
        }
        else if (numbers[l] + numbers[r] < target) {
            l++;
        } else {
            return [l + 1, r + 1];
        }
    }
    return [];
};
```

</details>

---
---
