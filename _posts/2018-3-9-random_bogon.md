# Bogon IP generator
I needed to generator a random private IP for a future project.  Today's exercise simply generator a random IP from within 
10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, and 169.254.0.0/16 ranges.

Code snippet, full program in Git repo
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Random Private IP generator
# Needed for a later project
import random
# Randomly choose 10, 172 or 192

# Once chosen.  pick random octets within range
CLASS = random.choice([10,172,192,169])

# 10.0.0.1 - 10.255.255.254
if CLASS == 10:
    OCT1 = random.randrange(1,255)
    OCT2 = random.randrange(1,255)
    OCT3 = random.randrange(1,254)
    print( str(CLASS) + "." + str(OCT1) + "." + str(OCT2) + "." + str(OCT3) )
```
