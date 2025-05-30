# calling_convention_cheat_sheet
oh my god i am so tired of looking it up all the time, thank god chromium made a cheetsheet lmao <br>
also system call numbers for 4 arch's below: [ty chromium](https://chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md#x86-32_bit)

<img width="583" alt="Screenshot 2025-05-30 at 8 59 56 AM" src="https://github.com/user-attachments/assets/e6eb6944-887c-4777-8961-3aca7940e24c" />

- arm 32: r0-r3, then rest pushed on stack
    - system call (enter kernel mode): `svc 0`, where value after svc is ignored by kernel
```
.global _start

.text
_start:
    @ write "Hi" to stdout
    mov     r0, #1            @ fd = 1 (stdout)
    adr     r1, msg           @ r1 = address of message
    mov     r2, #3            @ r2 = length of message
    mov     r7, #4            @ r7 = syscall number for write
    svc     #0                @ invoke syscall

    @ exit program 
    mov     r7, #1            @ syscall number for exit
    mov     r0, #0            @ exit code = 0
    svc     #0                @ invoke syscall

msg:
    .ascii  "Hi\n"
```
- arm 64: x0-x7, rest pushed on stack
    - system call:  `svc 0`
```
.global _start

.text
_start:
    @ write "Hi" to stdout
    mov     x0, #1            @ fd = 1 (stdout)
    adr     x1, msg           @ r1 = address of message
    mov     x2, #3            @ r2 = length of message
    mov     x8, #4            @ r7 = syscall number for write
    svc     #0                @ invoke syscall

    @ exit program 
    mov     x8, #1            @ syscall number for exit
    mov     x0, #0            @ exit code = 0
    svc     #0                @ invoke syscall

msg:
    .ascii  "Hi\n"

```
- x86 (i386):
    - __stdcall & __cdecl 
      - pushed onto stack (left to right order, last argument is first)
    - __fastcall
      - ECX and EDX, rest on the stack (right to left order)
    - system call: `int 0x80` im not sure if this best practice?
```
section .data
    msg db 'Hi', 10       ; "Hi\n"
    len equ $ - msg       ; length = 3

section .text
    global _start

_start:
    mov eax, 4            ; syscall number for sys_write = 4
    mov ebx, 1            ; fd = 1 (stdout)
    mov ecx, msg          ; pointer to message
    mov edx, len          ; length of message
    int 0x80              ; trigger syscall

    mov eax, 1            ; syscall number for sys_exit = 1
    xor ebx, ebx          ; status = 0
    int 0x80              ; trigger syscall
```
- x64 (amd64): first 6 registers, then rest on the stack
    - system call: `syscall`
```
section .data
    msg db 'Hi', 10       ; "Hi\n"
    len equ $ - msg       ; length = 3

section .text
    global _start

_start:
    mov rax, 1            ; syscall number for sys_write = 1
    mov rdi, 1            ; fd = 1 (stdout)
    mov rsi, msg          ; pointer to message
    mov rdx, len          ; length of message
    syscall               ; trigger syscall

    mov rax, 60            ; syscall number for sys_exit = 60
    xor rdi, rdi          ; status = 0, neat trick to zero out a register rather than mov 0 (faster and sets flags, where mov doesnt)
    syscall               ; trigger syscall
```

### ABI explained
- https://uvdn7.github.io/abi/ 
