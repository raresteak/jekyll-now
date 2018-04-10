# 2018-4-10 Learning datascience module

Taking a class online

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# minard.py

from datascience import *
import numpy as np

minard = Table.read_table('minard.csv')
print("-------------------------------------------------------")
print("printing table created with csv file")
print(minard)
print("-------------------------------------------------------")
print("Column method")
print(minard.column('Survivors'))
print("-------------------------------------------------------")
print("Select method")
print(minard.select('Survivors'))
print("-------------------------------------------------------")
#Get size of army starting out, element 0 of survivors column/array
initial_army_size = minard.column('Survivors').item(0)
print("-------------------------------------------------------")
print("Intial army size")
print(initial_army_size)
# add a percentage column to right of survivors to see how much the army size had changed.
# create new array
percentage_surviving = minard.column('Survivors') / initial_army_size
print("survivors percentage array")
print(percentage_surviving)
print("-------------------------------------------------------")
# add this new array, percentage_surviving, to a new table
minardWithPercentages = minard.with_column('Percent Surviving', percentage_surviving)

print (minardWithPercentages)
print("-------------------------------------------------------")
#print only row 1 of the minard table
print("take method")
print(minardWithPercentages.take(0))
```

Output

```
-------------------------------------------------------
printing table created with csv file
Longitude | Latitude | City        | Direction | Survivors
32        | 54.8     | Smolensk    | Advance   | 145000
33.2      | 54.9     | Dorogobouge | Advance   | 140000
34.4      | 55.5     | Chjat       | Advance   | 127100
37.6      | 55.8     | Moscou      | Advance   | 100000
34.3      | 55.2     | Wixma       | Retreat   | 55000
32        | 54.6     | Smolensk    | Retreat   | 24000
30.4      | 54.4     | Orscha      | Retreat   | 20000
26.8      | 54.3     | Moiodexno   | Retreat   | 12000
-------------------------------------------------------
Column method
[145000 140000 127100 100000  55000  24000  20000  12000]
-------------------------------------------------------
Select method
Survivors
145000
140000
127100
100000
55000
24000
20000
12000
-------------------------------------------------------
-------------------------------------------------------
Intial army size
145000
survivors percentage array
[1.         0.96551724 0.87655172 0.68965517 0.37931034 0.16551724
 0.13793103 0.08275862]
-------------------------------------------------------
Longitude | Latitude | City        | Direction | Survivors | Percent Surviving
32        | 54.8     | Smolensk    | Advance   | 145000    | 1
33.2      | 54.9     | Dorogobouge | Advance   | 140000    | 0.965517
34.4      | 55.5     | Chjat       | Advance   | 127100    | 0.876552
37.6      | 55.8     | Moscou      | Advance   | 100000    | 0.689655
34.3      | 55.2     | Wixma       | Retreat   | 55000     | 0.37931
32        | 54.6     | Smolensk    | Retreat   | 24000     | 0.165517
30.4      | 54.4     | Orscha      | Retreat   | 20000     | 0.137931
26.8      | 54.3     | Moiodexno   | Retreat   | 12000     | 0.0827586
-------------------------------------------------------
take method
Longitude | Latitude | City     | Direction | Survivors | Percent Surviving
32        | 54.8     | Smolensk | Advance   | 145000    | 1
```
