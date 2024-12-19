``` txt
ASM #3 - Uppercase - 100 điểm Viết chương trình nhập vào chuỗi văn bản, in ra chuỗi văn bản được in hoa
Yêu cầu:
Hệ điều hành: Windows & Linux
Chiều dài đoạn văn bản: 32 ký tự
```
Bài này ta sẽ duyệt qua từng kí tự, nếu kí tự nào đang viết thường thì sẽ -32 (chuyển thường thành hoa bằng ascii) 
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

    mov rsi, s 
    mov rcx, 32

upper:
    mov al, byte [rsi]
    cmp al, 0x61 
    jl skip
    cmp al, 0x7a
    jg skip
    sub al, 0x20
    mov byte [rsi], al

skip: 
    inc rsi

    loop upper

    mov rax, 1
    mov rdi, 1
    mov rsi, s
    mov rdx, 32
    syscall 

    mov rax, 60
    mov rdi, 0
    syscall 
```
