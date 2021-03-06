---
layout: api-command
language: Python
permalink: api/python/order_by/
command: order_by
related_commands:
    skip: skip
    limit: limit
    '[]': slice/
---

# Command syntax #

{% apibody %}
table.order_by([key1...], index=index_name) -> selection<stream>
selection.order_by(key1, [key2...]) -> selection<array>
sequence.order_by(key1, [key2...]) -> array
{% endapibody %}

# Description #

Sort the sequence by document values of the given key(s). `orderBy` defaults to ascending
ordering. To explicitly specify the ordering, wrap the attribute with either `r.asc` or
`r.desc`.

__Example:__ Order our heroes by a series of performance metrics.

```py
r.table('marvel').order_by('enemies_vanquished', 'damsels_saved').run(conn)
```

__Example:__ Indexes can be used to perform more efficient orderings. Notice that the
index ordering always has highes precedence. Thus the following example is equivalent to
the one above.

```py
r.table('marvel').order_by('damsels_saved', index='enemies_vanquished').run(conn)
```


__Example:__ You can also specify a descending order when using an index.

```py
r.table('marvel').order_by(index=r.desc('enemies_vanquished')).run(conn)
```


__Example:__ Let's lead with our best vanquishers by specify descending ordering.

```py
r.table('marvel').order_by(
    r.desc('enemies_vanquished'),
    r.asc('damsels_saved')
).run(conn)
```

__Example:__ You can use a function for ordering instead of just selecting an attribute.

```py
r.table('marvel').order_by(lambda doc: doc['enemiesVanquished'] + doc['damselsSaved']).run(conn)
```

__Example:__ Functions can also be used descendingly.

```py
r.table('marvel').order_by(r.desc(lambda doc: doc['enemiesVanquished'] + doc['damselsSaved'])).run(conn)
```

