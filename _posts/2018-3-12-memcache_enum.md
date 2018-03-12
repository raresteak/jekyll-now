# More memcached fun - enumeration 
This time enumerate all key/value pairs from the memcache server.

  Start out of with data already loaded into memcached.

1. Send 'stats items' after connect

Sample output

'''
STAT items:2:number 1
STAT items:2:age 365
STAT items:2:evicted 0
STAT items:2:evicted_nonzero 0
STAT items:2:evicted_time 0
STAT items:2:outofmemory 0
STAT items:2:tailrepairs 0
STAT items:2:reclaimed 0
STAT items:2:expired_unfetched 0
STAT items:2:evicted_unfetched 0
...
END
'''

2. Pull out the key number through regex, 

''' 
'items:(\d):number'
'''

3. Get key by name with 'stats cachedump <keynumber> 100

'''
stats cachedump 4 100
ITEM testkey2 [100 b; 1520852401 s]
END
'''

4. Get value of key.   This is not working from script.

'''
get testkey2
VALUE testkey2 1 100
��Y�UIf the code and the comments disagree, then both are probably wrong. -- Norm Schryer �.
END
'''

What comes out in the script is just END

Script output.  

'''
testkey1:b'END\r\n'
testkey0:b'END\r\n'
testkey2:b'END\r\n'
'''

Need to figure out why the mult-line output is not being capture like it was in step 1 with the stats items command.

Script memcache_query.tcp.enumerate.sh posted in github.    More to come.


'''python
#!/usr/bin/env python3
#Connect to a memcached server and dump key value pairs

####################
#WORK IN PROGRESS #
####################

import socket, re
TCP_IP = "192.168.168.168"
TCP_PORT = 11211
BUFFER_SIZE = 1024

statsItemsRegex = re.compile(r'items:(\d):number')

cacheDumpRegex = re.compile(r'ITEM\s(\S*)\s\[')

#setup connection Ref https://wiki.python.org/moin/TcpCommunication
sock = socket.socket(socket.AF_INET, # Internet
		    socket.SOCK_STREAM) # TCP
sock.connect((TCP_IP, TCP_PORT))
MESSAGE = "stats items\r\n"
sock.send(MESSAGE.encode('utf-8') )
#If you don't add .encode() you get error TypeError: a bytes-like object is required, not 'str'
RESPONSE = sock.recv(BUFFER_SIZE)

for KEYNUM in statsItemsRegex.findall(str(RESPONSE) ):
	#print(KEYNUM)
	MESSAGE2 = str("stats cachedump " + KEYNUM + " 100\r\n")
	#print("Sending: " + MESSAGE2 )
	sock.send(MESSAGE2.encode('utf-8') )
	RESPONSE2 = sock.recv(BUFFER_SIZE) 
	#print("Received..." + str(RESPONSE2) )
	keyName = cacheDumpRegex.findall(str(RESPONSE2) )
	#print(keyName[0])
	MESSAGE3 = str("get keyName[0]\r\n")
	sock.send(MESSAGE3.encode('utf-8') )
	RESPONSE3 = sock.recv(BUFFER_SIZE)
	print(keyName[0] + ":" + str(RESPONSE3) )
sock.shutdown(2)
sock.close()

'''
