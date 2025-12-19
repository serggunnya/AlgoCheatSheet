# MustKnow / TwoPointers
`(Поиск пар, Сравнение пар, Перестановка пар)`

---
### [`Leetcode 167. Two Sum II`](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted).

> _Дана строка __s__._
> 
> _Вернуть __true__ если строка палиндром._

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

### [`Leetcode 125. Valid Palindrome`](https://leetcode.com/problems/move-zeroes).

> _Дан массив __nums__ю._
> 
> _Переместить нули в конец массива, сохраняя порядок цифр._

```js
isPalindrome("A man, a plan, a canal: Panama")   // true "amanaplanacanalpanama" is a palindrome.
isPalindrome("race a car")   // false "raceacar" is not a palindrome.
isPalindrome(" ")   // true
```
<details>
   <summary><h4>Идея</h4></summary>

1) У палиндрома символы совпадаю в обоих направлениях.
2) Оба указателя ставим в концы строки.
3) Пока указатели не встретятся
   - Если `nums[l] !== nums[r] ` return `false`
4) вернуть `true`
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var isPalindrome = function (s) {
    s = s.toLowerCase().replace(/[^a-z0-9]/gi, "");
    var l = 0, r = s.length - 1;
    while (l < r) {
        if (s[l] !== s[r]) {
            return false;
        }
        l++;
        r--;
    }
    return true
};
```

</details>

---
---

### [`Leetcode 283. Move Zeroes`](https://leetcode.com/problems/move-zeroes).

> _Дан массив __nums__.
> 
> _Переместить нули в конец массива, сохраняя порядок цифр._

```js
moveZeroes([0,1,0,3,12])   // [1,3,12,0,0]
```
<details>
   <summary><h4>Идея</h4></summary>

1) Для перемещения пар необходимы два указателя
2) Оба указателя ставим в начало.
3) __Правый__ указатель перебирает массив. __Левый__ ждет
   - Если `nums[r] !== 0` меняем местами nums[r] и nums[l]
   - Двигаем __Левый__ указатель
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var moveZeroes = function(nums) {
    for(var r = 0, l = 0; r < nums.length; r++){
        if(nums[r] !== 0){
           var temp = nums[l];
           nums[l] = nums[r];
           nums[r] = temp;
           l++;
        }
    }
};
```

</details>

---
---
