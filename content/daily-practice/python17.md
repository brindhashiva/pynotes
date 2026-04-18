---
title: Python17
date: 2026-04-19
author: Your Name
cell_count: 3
score: 0
---

```python
nums = [3, 6, 9, 12]
for i in range(len(nums)):
    print(i, nums[i])
```

    0 3
    1 6
    2 9
    3 12
    


```python
nums = [1, 2, 3]
squares = [x * x for x in nums]
print(squares)
```

    [1, 4, 9]
    


```python
nums = [1, 2, 3, 4, 5]
odd = [x for x in nums if x % 2 != 0]
print(odd)
```

    [1, 3, 5]
    


---
**Score: 0**