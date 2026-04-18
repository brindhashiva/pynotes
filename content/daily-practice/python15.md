---
title: Python15
date: 2026-04-19
author: Your Name
cell_count: 3
score: 0
---

```python
a = [1, 2, 3]
b = a
b.append(4)
print("List:", a)
```

    List: [1, 2, 3, 4]
    


```python
a = (1, 2, 3)
# a[0] = 5  # error (immutable)
print(a)
```

    (1, 2, 3)
    


```python
a = {"x": 1}
b = a
b["y"] = 2
print(a)
```

    {'x': 1, 'y': 2}
    


---
**Score: 0**