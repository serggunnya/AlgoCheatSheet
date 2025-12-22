# MustKnow / TwoPointers
`(Поиск пар, Сравнение пар, Перестановка пар)`

---
### [`Leetcode 167. Two Sum II`](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted).

> _Дан массив __nums__._
> 
> _Вернуть индексы чисел сумма которых равна __target__._

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

> _Дана строка __s__._
> 
> _Вернуть __true__ если строка палиндром._

```js
isPalindrome("A man, a plan, a canal: Panama")   // true "amanaplanacanalpanama" is a palindrome.
isPalindrome("race a car")                       // false "raceacar" is not a palindrome.
isPalindrome(" ")                                // true
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
### [`Leetcode 680. Valid Palindrome II`](https://leetcode.com/problems/valid-palindrome-ii).

> _Дана строка __s__._
> 
> _Вернуть __true__ если строка станет палиндромом после одаления одного символа._

```js
validPalindrome("aba")    // true is a palindrome.
validPalindrome("abca")   // true is a palindrome.
validPalindrome("abc")    // false
```
<details>
   <summary><h4>Идея</h4></summary>

1) У палиндрома символы совпадаю в обоих направлениях.
2) Оба указателя ставим в концы строки.
3) Пока указатели не встретятся
   - Если `nums[l] !== nums[r] ` return `false`
   - Иначе
      - проверяем строку увеличив __левый__ указатель
      - проверяем строку уменьшив __правый__ указатель
      - вернуть `true` если один из вариантов правильный
4) вернуть `true`
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var validPalindrome = function (s) {
    function isPalingdome(s, l, r) {
        while (l < r) {
            if (s[l] !== s[r]) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }

    var l = 0, r = s.length - 1;
    while (l < r) {
        if (s[l] !== s[r]) {
            var testA = isPalingdome(s, l + 1, r);
            var testB = isPalingdome(s, l, r - 1);
            return testA || testB;
        }
        l++;
        r--;
    }
    return true;
};
```

</details>

---
---

### [`Leetcode 283. Move Zeroes`](https://leetcode.com/problems/move-zeroes).

> _Дан массив __nums__._
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
### [`Leetcode 15. 3Sum`](https://leetcode.com/problems/3sum).

> _Дан массив __nums__._
> 
> _Найти тройки сумма которых равна нулю, избегая повторений_

```js
threeSum([-1,0,1,2,-1,-4])   // [[-1,-1,2],[-1,0,1]]
threeSum([0,0,0])            // [[0,0,0]]
threeSum([0,1,1])            // []
```
<details>
   <summary><h4>Идея</h4></summary>

1) При поиске суммы N чисел, удобно использовать сортировку
2) Удобно свести решение к поиску суммы двух чисел как __Two Sum II__
3) Основной цикл проходит по числам для получения первого числа.  
     - пропусткаем дубликаты первого числа `nums[i] === nums[i - 1]`
4) Внутрений цикл использует два указателя для поиска пары чисел
     - __Левый__ указатель следующий после i `l = i+1`. __Правый__ указатель на последнем числе
     - Если сумма трёх чисел больше нуля уменшаем __правый__ указатель `r--`
     - Если сумма трёх чисел меньше нуля увеличиваем __левый__ указатель `l++`
     - Иначе сохраняем массив с тройкой в массив с ответом.
         - пропускаем повторение второго числа `nums[l]`
            - `while (l < r && nums[l] === nums[l + 1]) l++;`
         - пропускаем повторение третьего числа `nums[r]`
            - `while (l < r && nums[r] === nums[r - 1]) r--`
         - сдвигаем указатели
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var threeSum = function (nums) {
    var res = [];
    if (nums.length < 3) {
        return result;
    }

    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length - 2; i++) {
        if (nums[i] === nums[i - 1]) {
            continue;
        }

        var l = i + 1;
        var r = nums.length - 1;

        while (l < r) {
            var sum = nums[i] + nums[l] + nums[r];
            if (sum > 0) {
                r--;
            } else if (sum < 0) {
                l++;
            } else {
                res.push([nums[i], nums[l], nums[r]]);

                while (l < r && nums[l] === nums[l + 1]) {
                    l++;
                }

                while (l < r && nums[r] === nums[r - 1]) {
                    r--;
                }

                l++;
                r--
            }
        }
    }
    return res;
};
```

</details>

---
---
