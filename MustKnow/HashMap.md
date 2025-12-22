# MustKnow / HashMap
`(Быстрый поиск, Подсчет вхождений, Групировка по ключу)`

---
### [`Leetcode 217. Contains Duplicate`](https://leetcode.com/problems/contains-duplicate).

> _Дан массив __nums__. Верните __true__ или __false__, если есть дубликаты._

```js
containsDuplicate([1,2,3,1]) // true
containsDuplicate([1,2,3,4]) // false
containsDuplicate([1,1,1,3,3,4,3,2,4,2]) // true
```
<details>
   <summary><h4>Логика</h4></summary>

1) Для быстрого поиска чисел создадим __new Map()__ (или new Set())
2) Проверяем наличие в map числа `if map[ nums[i] ]` 
   - Если нет, сохраняем в map текущее число `map[nums[i]] = 1`
   - Если есть, возвращем `true`
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
  var containsDuplicate = function (nums) {
    var map = new Map();
    for (var i = 0; i < nums.length; i++) {
        if (map.has(nums[i])) return true;
        map.set(nums[i], 1);
    }
    return false;
};
```

</details>

---
---

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
   <summary><h4>Логика</h4></summary>

1) Для быстрого поиска чисел создадим __new Map()__
2) Проверяем наличие в map разницы чисел `if map[ target - nums[i] ]` 
   - Если нет, сохраняем в map текущее число и его индекс `map[nums[i]] = i`
   - Если есть, возвращем пару индексов `[map[target - nums[i]], i]`
      
</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
  function twoSum(nums, target) {
   var map = new Map();
   for(let i = 0; i < nums.length; i++){      
       if(map.has(target - nums[i])){
         return [map.get(target - nums[i]), i];
       }
       map.set(nums[i],i);
   }
   return [];
  };
```

</details>

---
---

### [`Leetcode 219. Contains Duplicate II`](https://leetcode.com/problems/contains-duplicate-ii).

> _Дан массив __nums__ и число __K__._
> 
> _Верните __true__, если `абсолютная` разница индексов двух __ближайших__ дубликатов __<= K__._

```js
containsNearbyDuplicate([1,2,3,1], 3)      // true
containsNearbyDuplicate(1,0,1,1], 1)       // true
containsNearbyDuplicate([1,2,3,1,2,3], 2)  // false 
```
<details>
   <summary><h4>Логика</h4></summary>

1) Для быстрого поиска чисел создадим __new Map()__
2) Проверяем наличие в __map__ чисела `if map[ nums[i] ]` 
   - Если есть и абсолютная разница сохраненного индекса и текущего `Math.abs(map[num[i]] - i) меньше или равна <= k` возвращаем  __true__
   - Иначе сохраняем/перезаписываем в __map__ текущее число и его индекс `map[nums[i]] = i`

</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
  function containsNearbyDuplicate(nums, k) {
   var map = new Map();
   for(let i = 0; i < nums.length; i++){      
       if(map.has(nums[i]) && Math.abs(map.get(nums[i]) - i ) <= k){
         return true;
       }
       map.set(nums[i], i);
   }
   return false;
  };
```

</details>

---
---

### [`Leetcode 242. Valid Anagram`](https://leetcode.com/problems/valid-anagram).
>[!NOTE]
> Анаграмма имеет ту же длину и число повторяющихся символов

> _Дано две строки __s__ и __t__._
> 
> _Верните __true__, если строка являются анаграмой_

```js
isAnagram(anagram","nagaram")      // true
isAnagram("rat","car")             // false 
```
<details>
   <summary><h4>Логика</h4></summary>

1) Если длины строк не совпадают, то это не анаграмма
2) Для подсчета символов создадим __new Map()__
3) Ищем символы второй строки в полученой карте __map__
   - Если симола нет в карте возвращаем __false__
   - Иначе уменьшаем счетчик символа в __map__
      - Если количество меньше нуля возвращаем __false__

</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var isAnagram = function (s, t) {
    if (s.length !== t.length) return false;

    var map = new Map();
    for (var i = 0; i < s.length; i++) {
        if (map.has(s[i])) {
            map.set(s[i], map.get(s[i]) + 1);
        } else {
            map.set(s[i], 1);
        }
    }

    for (var i = 0; i < t.length; i++) {
        if (!map.has(t[i])) {
            return false;
        }
        
        map.set(t[i], map.get(t[i]) - 1);        
        if (map.get(t[i]) < 0) {
            return false;
        }
    }
    return true;
};
```

</details>

---
---
### [`Leetcode 49. Group Anagrams`](https://leetcode.com/problems/group-anagrams).

> _Дан массив строк __strs__._
> 
> _Верните массив с сгрупироваными по общим символам строки_

> [!TIP]
> Для создания уникального ключа можно использовать Bucket массив
```js
groupAnagrams(["eat","tea","tan","ate","nat","bat"])  // [["bat"],["nat","tan"],["ate","eat","tea"]]
```
<details>
   <summary><h4>Логика</h4></summary>

1) Для группировки понадобится __new Map()__
2) Проходим по строкам
3) Получаем уникальный ключ на основе используемых символов
   - Если ключа нет в карте, создаём его и сохраняем строку в группе
   - Иначе добавляем строку в группу      
4) Возвращаем группы



</details>

<details>
  <summary><h4>Решение</h4></summary>
  
```js 
var groupAnagrams = function(strs) {
    function getKey(s){
        var bucket = new Array(26).fill(0);
        for(var i = 0; i < s.length; i++){
            bucket[(s[i]).charCodeAt()-97]++;
        }
        return bucket.toString()
    }

    var groups = new Map();
    for(var i = 0; i < strs.length; i++){
        var key = getKey(strs[i])  // strs[i].split("").sort().join("");
        
        if(!groups.has(key)){
            groups.set(key, [strs[i]]);
        }else{
            groups.get(key).push(strs[i]);
        }
    }
    return Array.from(groups.values())
};
```

</details>

---
---
