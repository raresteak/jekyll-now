# 2018-05-18
An exercise in base64 encoding a string multiple times.
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Base64 string encoder
# Take text from the clipboard, convert n times, place base64 encoded text back on clipboard.
import sys, base64, pyperclip

textToEncode = pyperclip.paste()   # Take clipboard contents and put it into variable

if len(sys.argv)-1 != 1:
	print("Usage error: call script.py number   where number is number of times to encode")	
	sys.exit()
elif len(textToEncode) < 1:
    print("Usage error")
    print("Place text on clipboard before calling program")
    print("Call script.py with one argument, number for number of times to encode")

timesToEncode = sys.argv[1]

def myB64encoder(aString):
    ''' Function to encode some text a number of times '''
    stringConverted = aString
    ConvertedStringToBytes = bytes(stringConverted, 'utf-8')
    # Example ConvertedStringToBytes will equal "b'YmxhaA=='" if input text is hello
    b64Text = base64.b64encode(ConvertedStringToBytes)
    return b64Text

# Setup for while loop
intermediateText = textToEncode

count=0

while count < int(timesToEncode):
    intermediateText = str(myB64encoder(intermediateText), 'utf-8')
    # need to convert byte output from myB64encoder back to a string
    count += 1

finalB64Encoded = intermediateText  # reassign for easier understanding what this text is.

print(textToEncode + " was encoded " + str(timesToEncode) + " times to:")
print(finalB64Encoded)
print("")
print("Output placed back on clipboard")
pyperclip.copy(finalB64Encoded)
'''
Test input:
The quick brown fox jumps over the lazy dog

Expected output, one time converted:
VGhlIHF1aWNrIGJyb3duIGZveCBqdW1wcyBvdmVyIHRoZSBsYXp5IGRvZw==
'''
```
