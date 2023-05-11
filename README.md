# Simple-Calculator
# I'm creating a simple calculator which is based on the assembly language x64
%include "util.asm"

GLOBAL _start

section .text

_start:
    mov rdi, num1
    call printstr
    call readint
    mov [user_num1], rax

    mov rdi, num2
    call printstr
    call readint
    mov [user_num2], rax

    mov rdi, operators
    call printstr

    mov rdi, user_operator
    mov rsi, 2
    call readstr

    cmp_operators:
    mov rdi, [user_operator]
    cmp byte [rdi], '+'
    je addition
    cmp byte [rdi], '-'
    je subtraction
    cmp byte [rdi], '*'
    je multiply
    cmp byte [rdi], '/'
    je division
    jmp exception

exception:
    mov rdi, error_mssge
    call printstr
    call endl
    jmp exit0

addition:
    mov rdi, [user_num1]
    add rdi, [user_num2]
    jmp results

subtraction:
    mov rdi, [user_num1]
    sub rdi, [user_num2]
    jmp results

multiply:
    mov rdi, [user_num1]
    imul rdi, [user_num2]
    jmp results

division:
    mov rdx, 0
    mov rax, [user_num1]
    mov rbx, [user_num2]
    idiv rbx
    mov rdi, rax
    jmp results

results:

    call printint
    call endl
    jmp exit0

section .data
    num1: db "Enter the number 1:", 0
    num2: db "Enter the number 2:", 0
    operators: db "Enter the operation to perform (+, -, *, /):", 0
    error_mssge: db "Cannot perform this operation.", 0

section .bss
    user_num1: resb 8
    user_num2: resb 8
    user_operator: resb 2
