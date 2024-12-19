``` txt
ASM #4 - Simple Addition - 100 điểm Viết chương trình tính tổng 2 số nguyên dương nhập vào từ bàn phím
Yêu cầu:
Hệ điều hành: Windows & Linux
Độ lớn tối đa số nhập vào: 31-bit (2^31 - 1)
```
Bài này có 2 cách là cộng bằng số và cộng bằng xâu, mình thấy cộng bằng số code sẽ đỡ rối hơn nên mình code theo cách này. 
``` asm
section .bss
    a resq 1
    b resq 1
    sum resq 1
    buffer resb 12

section .text
    global _start

_start:
    call input
    mov rax, qword [buffer]
    mov qword [a], rax

    call input
    mov rax, qword [buffer]
    mov qword [b], rax

    mov rax, qword [a]
    add rax, qword [b]
    mov qword [sum], rax
    mov rax, qword [sum]
    call output

    mov rax, 60            
    mov rdi, 0           
    syscall

input:
    mov rax, 0             
    mov rdi, 0            
    lea rsi, [buffer]       
    mov rdx, 12           
    syscall

    xor rax, rax          
    xor rbx, rbx          
    lea rsi, [buffer]

loop_convert:
    mov bl, byte [rsi]     
    cmp bl, 0xa           
    je end_input

    sub bl, '0'           
    imul rax, 10     
    add rax, rbx          
    inc rsi               
    jmp loop_convert

end_input:
    ret

output:
    xor rbx, rbx
    lea rdi, [buffer + 11] 
    mov byte [rdi], 0      

loop_print:
    xor rdx, rdx           
    mov rbx, 10            
    div rbx                
    add dl, '0'            
    dec rdi                
    mov [rdi], dl          
    test rax, rax          
    jnz loop_print         

    mov rax, 1
    mov rdi, 1                         
    lea rsi, [buffer + 12]
    mov rdx, 12
    syscall
    ret
```
+ input: nhập input vào và chuyển thành số để thực hiện phép cộng
+ output: chuyển kết quả của phép cộng thành xâu và in ra bằng syscall 
