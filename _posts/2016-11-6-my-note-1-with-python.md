---
layout: post
title: "my note 1 with python"
categories: test
---
```python
cities = ['city1','city2','city3']
#solution 1
i = 0
for city in cities:
    print(i,city)
    i += 1

# solution 2
for i, city in enumerate (cities):
    print(i,city)
```