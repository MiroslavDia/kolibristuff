eax = 74 - Work directly with network interface
ebx = -1 (Get number of active network devices)

    out:
        eax = number of active network devices 

bh = device number, for all following functions !

bl = 0 (Get device type)

    out:
        eax = device type number 

bl = 1 (Get device name)

    in:
        ecx = pointer to 64 byte buffer 
    out:
        name is copied into the buffer
        eax = -1 on error 

bl = 2 (Reset the device)

    in
        none 
    out
        eax = -1 on error 

bl = 3 (Stop device)

    in
        none 
    out
        eax = -1 on error 

TO BE FIGURED OUT

eax = 75 - Work with Sockets

These functions work like the ones found in UNIX (and windows)
for more info, please read http://beej.us/guide/bgnet/

bl = 0 (Open Socket)

    in:
        ecx = domain
        edx = type
        esi = protocol 
    out:
        eax = socket number, -1 on error 

bl = 1 (Close Socket)

    in:
        ecx = socket number 
    out:
        eax = -1 on error 

bl = 2 (Bind)

    in:
        ecx = socket number
        edx = pointer to sockaddr structure
        esi = length of sockaddr structure 
    out:
        eax = -1 on error 

bl = 3 (Listen)

    in:
        ecx = socket number
        edx = backlog 
    out:
        eax = -1 on error 

bl = 4 (connect)

    in:
        ecx = socket number
        edx = pointer to sockaddr structure
        esi = length of sockaddr structure 
    out:
        eax = -1 on error 

bl = 5 (accept)

    in:
        ecx = socket number
        edx = pointer to sockaddr structure
        esi = length of sockaddr structure 
    out:
        eax = socket number, -1 on error 

bl = 6 (send)

    in:
        ecx = socket number
        edx = pointer to buffer
        esi = length of buffer
        edi = flags 
    out:
        eax = -1 on error 

bl = 7 (receive)

    in:
        ecx = socket number
        edx = pointer to buffer
        esi = length of buffer
        edi = flags 
    out:
        eax = number of bytes copied, -1 on error 

bl = 8 (set socket options)

    in:
        ecx = socket number
        edx = ptr to optstruct

  Optstruct: dd level
             dd optionname
             dd optlength
             db options...

The buffer's first dword is the length of the buffer, minus the first dword offcourse

    out:
        eax = -1 on error 

bl = 9 (get socket options)

    in:
        ecx = socket number
        edx = ptr to optstruct

  Optstruct: dd level
             dd optionname
             dd optlength
             db options...
    out:
        eax = -1 on error, socket option otherwise 

bl = 10 (get IPC socketpair)

    in:
	/
    out:
	eax = -1 on error, socketnum1 otherwise
	ebx = socketnum2

TIP

when you import 'network.inc' and 'macros.inc' into your source code, you can use the following syntax to work with sockets:


for example, to open a socket

mcall socket, AF_INET, SOCK_DGRAM,0
mov [socketnum], eax

then to connect to a server

mcall connect, [socketnum], sockaddr, 18


eax = 76 - Work with protocols

high half of ebx = protocol number (for all subfunctions!)
bh = device number (for all subfunctions!)
bl = subfunction number, depends on protocol type

For Ethernet protocol

0 - Read # Packets send
1 - Read # Packets received
2 - Read # Bytes send
3 - Read # Bytes received
4 - Read MAC
5 - Write MAC
6 - Read IN-QUEUE size
7 - Read OUT-QUEUE size
For IPv4 protocol

0 - Read # IP packets send
1 - Read # IP packets received
2 - Read IP
3 - Write IP
4 - Read DNS
5 - Write DNS
6 - Read subnet
7 - Write subnet
8 - Read gateway
9 - Write gateway
For ARP protocol

0 - Read # ARP packets send
1 - Read # ARP packets received
2 - Get # ARP entry's
3 - Read ARP entry
4 - Add static ARP entry
5 - Remove ARP entry (-1 = remove all)
For ICMP protocol

0 - Read # ICMP packets send
1 - Read # ICMP packets received
3 - enable/disable ICMP echo reply
For UDP protocol

0 - Read # UDP packets send
1 - Read # UDP packets received
For TCP protocol

0 - Read # TCP packets send
1 - Read # TCP packets received 
