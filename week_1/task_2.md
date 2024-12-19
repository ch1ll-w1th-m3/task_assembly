``` txt
ASM #2 - Echo - 100 điểm Viết chương trình nhập vào đoạn văn bản, in ra đoạn văn bản vừa nhập
Yêu cầu:
Hệ điều hành: Windows & Linux
Chiều dài đoạn văn bản: 32 ký tự
```
Bài này chỉ cần dùng syscall để nhập vào văn bản rồi in ra ngược lại bằng syscall. 
``` asm
section .bss 
    s resb 32

section .text 
    global _start 

_start: 
    mov rax, 0
    mov rdi, 0
    mov rsi, s
    mov rdx, 32
    syscall 

    mov rax, 1
    mov rdi, 1
    mov rsi, s
    mov rdx, 32
    syscall 

    mov rax, 60
    mov rdi, 0
    syscall 
```
