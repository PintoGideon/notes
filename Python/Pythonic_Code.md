### Flat is better than nested

### Why Dictionaries

Dictionaries are everywhere in Python

- Backing store for types, classes and objects
- Isomorphic with JSON
- Keyword arguments in methods
- Add significant performance to random access
- Database records (RDBMS and NoSQL)

Stop using lists for everything

Relative performance of two alogrithms?

```python

data_list=[]

for d_id in range(500000):
    interesting_ids= {
        random.randint(0,len(data_list)) for _ in range(0,100)}

        

```

```python

data_lookup=dict(....)
interesting_points=[....]
for i in interesting_ids:
    pt=data[i]
    interesting_points.append(pt)
```

