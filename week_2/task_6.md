``` txt
Bài 6. Viết chương trình đảo ngược xâu nhập vào.
Yêu cầu:
- HĐH: Win + Linux
- Chiều dài xâu nhập vào: 256 ký tự
- Sử dụng ít nhất 1 procedure
```
Mình tận dụng stack của assembly để lưu và reverse luôn xâu nhập vào. 
``` asm
section .bss 
    s resb 256

section .text 
    global _start 
 
_start: 
    sub rsp, 0xff
    lea r10, [rsp]
    mov rax, 0
    mov rdi, 0
    lea rsi, [r10]
    mov rdx, 0xff
    syscall 
   
    lea rsi, [r10]
    call reverse_length

    call reverse_string

    mov rax, 1
    mov rdi, 1
    lea rsi, [s]
    mov rdx, 0xff
    syscall 

    mov rax, 60
    mov rdi, 0
    syscall 

reverse_length: 
    xor rcx, rcx 

loop_length:
    cmp byte [rsi + rcx], 0
    je done 
    inc rcx 
    jmp loop_length 

reverse_string: 
    lea rsi, [s]
    mov rbx, rcx
    dec rbx 

loop_reverse:
    dec rbx
    jl done  
    mov al, [r10 + rbx]
    mov [rsi], al 
    inc rsi 
    jmp loop_reverse

done: 
    ret  
```
