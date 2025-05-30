# calling_convention_cheat_sheet
oh my god i am so tired of looking it up all the time, thank god chromium made a cheetsheet lmao 
also system call numbers for 4 arch's below: [ty chromium](https://chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md#x86-32_bit)

<img width="583" alt="Screenshot 2025-05-30 at 8 59 56 AM" src="https://github.com/user-attachments/assets/e6eb6944-887c-4777-8961-3aca7940e24c" />

- arm 32: r0-r3, then rest pushed on stack
    - system call (enter kernel mode): `svc 0`, where value after svc is ignored by kernel 
- arm 64: x0-x7, rest pushed on stack
    - system call:  `svc 0` 
- x86 (i386):
    - __stdcall & __cdecl 
      - pushed onto stack (left to right order, last argument is first)
    - __fastcall
      - ECX and EDX, rest on the stack (right to left order)
    - system call: `int 0x80` im not sure if this best practice?
- x64 (amd64): first 6 registers, then rest on the stack
    - system call: `syscall` 
