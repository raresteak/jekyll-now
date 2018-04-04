# 2018-04-04 Python port scanner

Needed a port scanner on a box that didn't have nmap.   Found this great post by coderholic 
http://www.coderholic.com/python-port-scanner/
which served a majority basis of what I needed.

Made couple changes to suite my needs:
1. Changed code to run under Python3
2. Added try/except for handling bad NS lookups
3. Changed for loop to target specific ports
4. Added print for closed ports

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# port_scanner.py
# Original code: http://www.coderholic.com/python-port-scanner/ 
# Thank you coderholic
# 
# Made a couple changes from original design.
# 1. Changed code to run under Python3, commented below.
# 2. Added try/except for handling bad NS lookups, commented below
# 3. Changed for loop to target specific ports, commented below
# 4. Added print for closed ports, commented below.

# To do
# 1. Add batch mode 

from socket import * 
# explanaition of above import
# https://stackoverflow.com/questions/24872125/socket-import-does-not-work-from-does-whats-the-deal


# Updated input for python3
target = input('Enter fqdn to scan: ')

# Adding code to catch fqdn's that are unresolvable.
try:
    targetIP = gethostbyname(target)
except Exception as errorMsg:
    print ("ERROR: Bad FQDN: " + target)
    # capturing exception https://docs.python.org/3/tutorial/errors.html
    print ("Exception was {0}".format(errorMsg) )
    exit(1)

# updated print for python3
print ('Starting scan on host ', targetIP)

#scan reserved ports
#for i in range(20, 1025):
# scan specif ports, if doing range comment out for statement below, uncomment above range
for i in [3389, 389, 445, 446]:
    s = socket(AF_INET, SOCK_STREAM)

    result = s.connect_ex((targetIP, i))

    if(result == 0) :
        # updated print for python3
        print ('Port ' + str(i) + ': ' + 'OPEN')
    # added by me
    # comment out next two lines if you don't want to see list of closed ports
    else:
        print ('Port ' + str(i) + ': ' + 'CLOSED...result code: ' + str(result))
    s.close()
```

Output

```
F:\py>port_scanner.py
Enter fqdn to scan: localhost
Starting scan on host  127.0.0.1
Port 18080: OPEN
Port 389: CLOSED...result code: 10061
Port 445: OPEN
Port 446: CLOSED...result code: 10061

F:\py>port_scanner.py
Enter fqdn to scan: bogushostname
ERROR: Bad FQDN: bogushostname
Exception was [Errno 11004] getaddrinfo failed
```

Code upload to github https://github.com/raresteak/py/blob/master/port_scanner.py

Now need to get back to working on that regex from prior post that I've been ignoring to work right.
