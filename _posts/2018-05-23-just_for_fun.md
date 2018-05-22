# 2018-05-23 just for fun

Get my geohash using antigravity.

Have fun http://wiki.xkcd.com/geohashing/Main_Page

![explanation](https://imgs.xkcd.com/comics/python.png)

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Generate a geohash using antigravity module
# Instructions
# Find the current day's DOW opening
# Run program
# go hunt for the geohash in your area
import antigravity
import time

# Change to your coord.
LAT=28
LONG=-81
#
DATE=time.strftime('%Y-%m-%d-')
bDate = bytes(DATE, 'utf-8')

print("Enter prior days DOW close: ")
dowCloseOpen = float(input())

bDateDow = bytes(DATE + str(dowCloseOpen), 'utf-8')

antigravity.geohash(LAT,LONG,bDateDow)

'''
Having fun https://xkcd.com/353/
'''
```

### Output

```
x:\py>geoHashMe.py
Enter prior days DOW close:
99999.99
28.802283 -81.768742
```
