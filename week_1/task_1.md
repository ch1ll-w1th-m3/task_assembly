``` txt
ASM #1 - Hello World - 100 điểm Viết chương trình in ra màn hình dòng chữ "Hello, World!"
Yêu cầu:
Hệ điều hành: Windows & Linux
Hiểu các thành phần của 1 chương trình assembly, giải thích từng dòng code
```
Cấu trúc của 1 chương trình: 
``` asm
section .data 
    s db "Hello, World!", 0
    
section .text 
    global _start
    
_start: 
    mov rax, 1 
    mov rdi, 1
    mov rsi, s 
    mov rdx, 13
    syscall
    
    mov rax, 60
    mov rdi, 0
    syscall
```
Để in dòng chữ **Hello, World!** ra, ta dùng write và stdout của syscall, cụ thể: 
+ rax: lưu số hiệu của syscall (0: read, 1: write, 60: exit, ...)
+ rdi: lưu mã file descriptor (0: stdin, 1: stdout)
+ rsi: lưu tham số thứ hai của syscall (bộ nhớ chứa/ghi dữ liệu)
+ rdx: lưu tham số thứ ba của syscall (byte cần ghi/đọc) 
