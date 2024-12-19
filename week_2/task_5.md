``` txt
Bài 5. Viết chương trình tìm xâu con:
- Nhập vào xâu S (100 ký tự) và xâu C (10 ký tự).
- Tìm số lần xuất hiện xâu con C trong xâu S và các vị trí xuất hiện.
Yêu cầu:
- HĐH: Win + Linux - Input vào từ bàn phím xâu S và C.
- Output: Dòng 1 chứa tổng số lần xuất hiện xâu con C.
          Dòng 2 chứa vị trí xuất hiện xâu con C (tính từ 0 là vị trí bắt đầu xâu S).
- Sử dụng ít nhất 1 procedure.
Vd: INPUT: S = Cong hoa xa hoi chu nghia Viet Nam C = ho
    OUTPUT: 2 5 12
```
Bài này thuật toán thì dễ nhưng để code bằng assembly thì khá mất thời gian. 

Thuật toán của mình là duyệt qua xâu S và so sánh kí tự hiện tại với kí tự đầu của xâu C và cứ tiếp tục vậy cho tới khi kí tự cuối của xâu C giống với kí tự trong xâu S thì kết quả sẽ tăng lên. 

``` asm
section .data 
    nl db 0xa
    space db " "

section .bss 
    S resb 101 
    C resb 11 
    count resb 4
    ind resb 100 
    num resb 12 

section .text
    global _start

_start: 
    mov rax, 0
    mov rdi, 0
    lea rsi, [S]
    mov rdx, 101 
    syscall 

    mov rax, 0
    mov rdi, 0
    lea rsi, [C] 
    mov rdx, 11
    syscall 

    lea rdi, [S] 
    call strlen 
    mov r12, rax

    lea rdi, [C] 
    call strlen 
    mov r13, rax

    mov qword [count], 0
    xor r14, r14
    xor r15, r15

search_loop: 
    cmp r15, r12
    jge print_res

    lea rdi, [S] 
    add rdi, r15

    lea rsi, [C] 
    mov rdx, r13

    call cmp_strings

    test rax, rax
    jz next

    inc qword [count]
    mov [ind + r14 * 4], r15d
    inc r14

next: 
    inc r15 
    jmp search_loop 

print_res: 
    mov rax, [count] 
    call print_num
    mov rax, 1
    mov rdi, 1
    lea rsi, [nl] 
    mov rdx, 1
    syscall 

    xor r15, r15

print_ind: 
    cmp r15, r14
    jge end

    mov eax, [ind + r15 * 4] 
    call print_num
    mov rax, 1
    mov rdi, 1
    lea rsi, [space]
    mov rdx, 1
    syscall 

    inc r15 
    jmp print_ind

end: 
    mov rax, 60 
    xor rdi, rdi 
    syscall 

strlen: 
    push rbx 
    xor rax, rax 

loop: 
    mov bl, byte [rdi + rax] 
    cmp bl, 0xa 
    je done 
    cmp bl, 0
    je done 
    inc rax 
    jmp loop 

done: 
    pop rbx 
    ret 

cmp_strings: 
    push rbx 
    push rcx 
    mov rcx, rdx 
    xor rbx, rbx

loop_cmp: 
    mov al, [rdi + rbx] 
    mov ah, [rsi + rbx] 
    cmp al, ah 
    jne not_equal 

    inc rbx 
    dec rcx 
    jnz loop_cmp 

    mov rax, 1
    jmp done_cmp 

not_equal: 
    xor rax, rax 

done_cmp: 
    pop rcx 
    pop rbx 
    ret 

print_num: 
    push rbx 
    push rcx 
    push rdx 

    lea rdi, [num + 11] 
    mov byte [rdi], 0 
    mov rbx, 10 

convert: 
    dec rdi 
    xor rdx, rdx 
    div rbx 
    add dl, '0' 
    mov [rdi], dl 
    test rax, rax 
    jnz convert 

    mov rax, 1
    mov rsi, rdi 
    mov rdi, 1
    lea rdx, [num + 11] 
    sub rdx, rsi 
    syscall 

    pop rdx 
    pop rcx 
    pop rbx 
    ret 
```
+ search_loop: để kiểm tra xem có thỏa mãn xâu C tồn tại trong xâu S không.
+ cmp_strings: để so sánh, duyệt qua các kí tự trong xâu C và xâu S.

Sau khi duyệt xong hết thì chỉ cần in ra kết quả bằng các hàm print trong chương trình. 
