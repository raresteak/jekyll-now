###Intro 
I wanted my first blog post to be about memcached since it’s been in the news recently with the recent largest DDOS attack ever orchestrated at 1.7 Tbps recently.  Play with with troublesome code and use Python to do it.   
###Installation of memcached
Step 1 install memcache on Centos in my lab.   Yum install memcached
Step 2.   configure to listen on the non loopback.    Edit /etc/sysconfig/memcached OPTIONS line.  Add -l <IP>
Step 3 start memcached, systemctl start memcached
Step 4 test.
Test TCP
 echo "stats" | nc 192.168.168.168 11211 | grep total_items
STAT total_items 19

Test UDP:
 echo "stats" | nc -u 192.168.168.168 11211 | grep total_items
hung, not sure why

adding data to memcached via telnet
Format set KEY META_DATA EXPIRY_TIME LENGTH_IN_BYTES

telnet ip 11211
set myteststringkey 0 0 11<enter>
Hello World<enter>
STORED
get myteststringkey<enter>
Hello World
flush_all <enter>

###Python fun
Now the fun with Python using pylibmc http://sendapatch.se/projects/pylibmc/
A script to load and retrieve data
$ cat memcache_query.pylibmc.sh 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#
# Connect to a memcached server over UDP, retrieve some keys
import pylibmc
# Setup connection
MEMCONN = pylibmc.Client(["udp:192.168.168.168"])
# Populate data with a key called myteststringkey
MEMCONN["myteststringkey"] = "The quick brown fox jumps over the lazy dog"

RET_DATA = MEMCONN["myteststringkey"]
print(RET_DATA)

Running the program produced:
Traceback (most recent call last):
  File "./memcache_query.pylibmc.sh", line 14, in <module>
    RET_DATA = MEMCONN["myteststringkey"]
  File "/usr/local/lib/python3.5/dist-packages/pylibmc/client.py", line 158, in __getitem__
    value = self.get(key, _MISS_SENTINEL)
pylibmc.NotSupportedError: error 28 from memcached_get(myteststringkey): (0x20f87a0) ACTION NOT SUPPORTED -> libmemcached/get.cc:216

Searching the Internet confirmed my suspicion that udp support of the mylibmc was not supported like the message says.   The module does support loading via UDP so my key was present in memcached when checking with telnet.

Next wanted to try out Python’s socket module since simple TCP connections can be done from telnet.

$ cat memcache_query.tcp.sh 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#
# Connect to a memcached server over UDP, retrieve some keys
import socket
TCP_IP = "192.168.168.168"
TCP_PORT = 11211
MESSAGE = "get myteststringkey\r\n"
BUFFER_SIZE = 1024

sock = socket.socket(socket.AF_INET, # Internet
		    socket.SOCK_STREAM) # TCP
sock.connect((TCP_IP, TCP_PORT))
sock.send(MESSAGE.encode('utf-8') )
# If you don't add .encode() you get error TypeError: a bytes-like object is required, not 'str'
RESPONSE = sock.recv(BUFFER_SIZE)
sock.close()
print("Received..." + str(RESPONSE) )

Basically a copy and paste from https://wiki.python.org/moin/TcpCommunication
$ ./memcache_query.tcp.sh 
Received...b'VALUE myteststringkey 1 58\r\n\x80\x04\x95/\x00\x00\x00\x00\x00\x00\x00\x8c+The quick brown fox jumps over the lazy dog\x94.\r\nEND\r\n'

None the less learned how to setup a TCP connection, send data, and catch the response.

Also tried a UDP socket connection to send "get myteststringkey\r\n" but that was also handing.    Likewise $ echo "get myteststringkey\r\n" | nc 192.168.168.168 112211 was hanging.    

Need to keep looking into this because the news of this was very interesting, so hopefully more to come...



###How to protect your exposed memcached server?    
Simple.  By default it listens on both tcp and udp, disable listening on UDP if you absolutely don’t need it, which was the contributing factor to the DDOS attack.   Have it listen to loopback only, or configure host based firewall to only respond to queries from known machines.
Config change:
add -U 0 to OPTIONS line /etc/sysconfig/memcached
this option is documented in the memcached man page

Iptables eample:
firewall-cmd --new-zone=memchached –permanent
firewall-cmd --zone=memcached –add-source=127.0.0.1/32 –permanent
firewall-cmd --zone=memcached –add-source=10.10.10.10/32 –permanent
firewall-cmd --zone=memcached –add-port=11211/tcp –permanent
firewall-cmd --reload
