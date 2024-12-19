``` txt
Bài 8. Advanced Addition
Viết chương trình nhập vào số 2 nguyên dương có 20 chữ số, tính tổng 2 số vừa nhập vào.
Yêu cầu: - HĐH: Win + Linux
- Lưu trữ số nhập vào thành mảng, cộng từng chữ số từ phải qua trái.
- Sử dụng ít nhất 1 procedure
```
Bài này mình sẽ dùng cách còn lại ở task_4, là cộng bằng xâu (cộng theo lớp 1 :3).
``` asm
section .bss 
    a resb 22
    b resb 22
    c resb 23 
    lena resb 2
    lenb resb 2

section .text 
    global _start 

_start: 
    mov rax, 0
    mov rdi, 0
    lea rsi, [a]
    mov rdx, 21
    syscall

    mov rax, 0
    mov rdi, 0
    lea rsi, [b]
    mov rdx, 21 
    syscall

    lea rdi, [a]
    call strlen 
    mov byte [lena], al

    lea rdi, [b]
    call strlen
    mov byte[lenb], al

    call solve 

    mov rax, 1
    mov rdi, 1
    lea rsi, [c]
    mov rdx, 21
    syscall 
    
    mov rax, 60 
    mov rdi, 0 
    syscall 

strlen: 
    mov r8, 0
    mov r12, 0 

calc_strlen: 
    mov r12b, byte [rdi + r8] 
    cmp r12b, 0
    je end_calc_strlen

    cmp r12b, 0xa
    je end_calc_strlen

    inc r8
    jmp calc_strlen 

end_calc_strlen:
    mov rax, r8
    ret

solve: 
    movzx r11, byte [lena]
    mov r8, 0

init_answer: 
    cmp r11, 0
    je next
       
    lea r10, [c + 20]
    sub r10, r8 
    
    lea r12, [a - 1]
    add r12, r11 

    mov r12b, byte [r12]
    mov byte [r10], r12b

    inc r8
    dec r11
    jmp init_answer 

next: 
    mov r8, 0
    movzx r11, byte [lenb]
    mov rbx, 0
 
plus: 
    mov rax, 0 
    cmp r11, 0
    jle end_plus 

    lea r12, [b - 1] 
    add r12, r11
    
    mov al, byte [r12]    
    sub al, 48

mid_plus:
    lea r10, [c + 20]
    sub r10, r8 

    add al, byte [r10] 
    cmp al, 48
    jl push_to_answer
    sub al, 48

push_to_answer: 
    movzx rax, al 
    add rax, rbx

    mov rdx, 0
    mov rcx, 10 
    div rcx 
    mov rbx, rax
    add rdx, 48 

    lea r10, [c + 20] 
    sub r10, r8 

    mov byte [r10], dl 
    inc r8 
    dec r11
    jmp plus 

end_plus: 
    cmp rbx, 0
    je end
    jmp mid_plus 

end: 
    ret
```
+ strlen: tính độ dài của xâu vừa nhập vào (trong bài này là tính số chữ số của a và b)
+ solve: tính a + b
