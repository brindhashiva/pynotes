---
title: Python16
date: 2026-04-19
author: Your Name
cell_count: 3
score: 0
---

```python
nums = [1, 2, 3, 4, 5]
even = []
for n in nums:
    if n % 2 == 0:
        even.append(n)
print(even)
```

    [2, 4]
    


```python
text = "hello world"
count = 0
for ch in text:
    if ch == "l":
        count += 1
print("Count of l:", count)
```

    Count of l: 3
    


```python
nums = [10, 20, 30]
total = sum(nums)
print("Total:", total)
```

    Total: 60
    


---
**Score: 0**