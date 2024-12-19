``` txt
Bài 7. Viết chương trình nhập vào số nguyên N, in ra N số Fibonaci đầu tiên.
Yêu cầu:
- HĐH: Win + Linux - N < 100
- Sử dụng ít nhất 1 procedure
```
Ở bài này, mình khai báo thêm 2 biến a và b để lưu các số fib hiện tại, sau đó cộng dần lên và chuyển thành xâu, lưu vô answer để xuất ra kết quả. 
``` asm
section .data 
    nl db 0xa 

section .bss 
    n resb 5
    answer resb 20 
    a resq 1
    b resq 1
    
section .text 
    global _start 

_start: 
    mov rax, 0
    mov rdi, 0
    lea rsi, [n] 
    mov rdx, 5
    syscall 

    lea rdi, [n] 
    call sti 
    mov rcx, rax
    
    mov qword [a], 0
    mov qword [b], 1

    call print_fib

    mov rax, 60 
    mov rdi, 0 
    syscall 

sti: 
    xor rax, rax
    xor r8, r8	
    mov r12, 0 
    
loop: 
    mov r12b, byte [rdi + r8] 
    cmp r12b, 0 
    je end_loop 
    cmp r12b, 0xa 
    je end_loop 
    
    sub r12b, '0'
    imul rax, rax, 10
    add rax, r12 
    inc r8

    jmp loop 

end_loop: 
    ret

print_fib:
    test rcx, rcx
    jz end_solve

    mov rax, [a] 
    call print

    mov rdx, [b] 
    mov rbx, [a]
    add rbx, rdx 
    mov [a], rdx
    mov [b], rbx

    dec rcx 
    loop print_fib
    ret

print: 
    lea rsi, [answer + 25]
    mov rdi, 10 

convert: 
    xor rdx, rdx 
    div rdi 
    add dl, '0' 
    dec rsi 
    mov [rsi], dl
    test rax, rax
    jnz convert

    mov rax, 1
    mov rdi, 1
    lea rdx, [answer + 25] 
    sub rdx, rsi 
    push rsi
    syscall 

    mov rax, 1
    mov rdi, 1
    lea rsi, [nl] 
    mov rdx, 1
    syscall 
    ret 

end_solve: 
    ret 
```
+ sti: chuyển input n thành số.
+ print_fib: in ra các số fib từ 1 tới n ở từng dòng.
+ convert: chuyển số fib hiện tại thành xâu và lưu vào answer. 
