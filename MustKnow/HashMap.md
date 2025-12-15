# MustKnow / HashMap

### [`Leetcode 1. Two Sum`](https://leetcode.com/problems/two-sum).

> _Дан массив __nums__ и число __target__._
> 
> _Верните индексы двух чисел, сумма которых равна __target__._

```js
twoSum([2,7,11,15],9) // [0,1]
twoSum([3,2,4],6)     // [1,2]
twoSum([3,3],6)       // [0,1]
```
<details>
   <summary><h4>Мышление</h4></summary>

1) Для поиска чисел создадим __new Map()__
2) Проверяем наличие в map разницы чисел `if map[ target - nums[i] ]` 
   - Если нет, сохраняем в map текущее число и его индекс `map[nums[i]] = i`
   - Если есть, возвращем пару индексов `[map[target - nums[i]], i]`
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
  function twoSum(nums, target) {
   var hash = new Map();
   for(let i = 0; i < nums.length; i++){      
       if(hash.has(target - nums[i])){
         return [hash.get(target - nums[i]), i];
       }
       hash.set(nums[i],i);
   }
   return [];
  };
```

</details>

---
