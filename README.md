# conditions instructions


Task:
[conditions.txt](https://github.com/user-attachments/files/21810796/conditions.txt) 


[Uplosection .data
    nums dd 4, 8, 12, 16, 20
    newline db 10
    msg db "Even numbers up to 20:", 10, 0
    msg2 db "The largest number is: ", 0

section .bss
    largest resd 1
    buffer resb 16

section .text
    global _start

_start:
    mov eax, 4
    mov ebx, 1
    mov ecx, msg
    mov edx, 24
    int 0x80

    mov ecx, 2
.print_loop:
    cmp ecx, 22
    jge .done_print
    call print_number
    call print_newline
    add ecx, 2
    jmp .print_loop
.done_print:

    mov eax, [nums]
    mov [largest], eax
    mov esi, 1
.find_largest:
    cmp esi, 5
    jge .done_largest
    mov ebx, [nums + esi*4]
    mov eax, [largest]
    cmp ebx, eax
    jle .skip
    mov [largest], ebx
.skip:
    inc esi
    jmp .find_largest

.done_largest:
    mov eax, 4
    mov ebx, 1
    mov ecx, msg2
    mov edx, 24
    int 0x80

    mov ecx, [largest]
    call print_number
    call print_newline

    mov eax, 1
    xor ebx, ebx
    int 0x80

print_newline:
    mov eax, 4
    mov ebx, 1
    mov ecx, newline
    mov edx, 1
    int 0x80
    ret

print_number:
    mov edi, buffer
    mov eax, ecx
    mov ebx, 10
    mov esi, edi
.reverse_loop:
    xor edx, edx
    div ebx
    add dl, '0'
    dec esi
    mov [esi], dl
    test eax, eax
    jnz .reverse_loop
    mov eax, 4
    mov ebx, 1
    mov ecx, esi
    mov edx, buffer + 16
    sub edx, esi
    int 0x80
    ret
ading conditions.txt…]()


Thought Process <img width="761" height="491" alt="largest" src="https://github.com/user-attachments/assets/a29e6498-3734-495b-b27d-84f0ea9b9629" />

Challenges: 

The main challenge was if the loop counter isn’t incremented or checked correctly, the loop can become infinite or skip values.
Jumping to the wrong level  causes logic bugs.
It is east to compare the wrong values.


Working Code ![largest](https://github.com/user-attachments/assets/0e9e1596-b4f3-4cf4-966f-b35f3d858a00)

