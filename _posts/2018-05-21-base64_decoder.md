# 2018-05-21 base64 decoder

Needed a decoder to go along with the encoder from last week.

Install pyperclip before running.

The script takes a base64 encoded string from the _clipboard_ and ouputs a decoded string to the screen 
and places the decoded string back on the clipboard should one need to keep decoding.

Enjoy

https://raw.githubusercontent.com/raresteak/py/master/b64d.py

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Base64 string decoder 
import base64, pyperclip

base64ToDecode = pyperclip.paste()   # Take clipboard contents and put it into variable

if len(base64ToDecode) < 1: # Ensure there's text on the clipboard
    print("Usage error: Place text on clipboard before calling program")

try: # Catch when attempting to decode non base64
# probably is a better way to catch this
    decodedOutput = str(base64.b64decode(base64ToDecode), 'utf-8')
# Need to convert to string because output of base64.b64decode() is in bytes
except: 
    print("\"" + base64ToDecode + "\" is not properly padded, try adding 1 to 3, = chars at the end.")
    # helped with understanding error https://stackoverflow.com/questions/38683439/how-to-decode-base64-in-python3
    print("Or you've completly decoded!")
    exit()

print("Output print below AND placed back on clipboard.")
print(decodedOutput)
print("If needed run program again to decode further.")
pyperclip.copy(decodedOutput) #place back on clipboard
```
