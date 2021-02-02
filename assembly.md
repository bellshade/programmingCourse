## list assembly programming ebook

- [ARM assembly language](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/ARM%20Assembly%20Language%20Programming%20-%20Pete%20Cockerell.pdf)
- [Assembly language step by step](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Assembly%20Language%20Step-by-Step%20Programming%20with%20DOS%20and%20Linux.chm)
- [Assembly for x86 processor](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Assembly%20Language%20for%20x86%20Processors%206th%20Ed.pdf)
- [Assembly language for programmers](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Assembly%20Language%2C%20The%20True%20Language%20Of%20Programmers.pdf)
- [IA 32 20 intel architecture software developer](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/IA-32%20Intel%C2%AE%20Architecture%20Software%20Developer%27s%20Manual%20%28Volume%201%20-%20Basic%20Architecture%29.pdf)
- [introduction to 80x86 assembly programming](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Jones%20and%20Bartlett%2CIntroduction%20to%2080x86%20Assembly%20Language%20and%20Computer%20Architecture.pdf)
- [Linux assemlby programming 2000](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Linux%20Assembly%20Language%20Programming%202000.pdf)
- [MASM Reference](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/MASMReference.pdf)
- [MIPS assembly language programming](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/MIPS%20Assembly%20Language%20Programming%202003.pdf)
- [Professional assembly language](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Professional%20Assembly%20Language.pdf)
- [Guide to assembly programming](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Springer%20-%20Guide%20to%20Assembly%20Language%20Programming%20in%20Linux.pdf)
- [The art for assembly programming](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/The%20Art%20Of%20Assembly%20Language%202003.chm)
- [The zen of assembly language](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/The%20Zen%20Of%20Assembly%20Language%201990%20-%20Michael%20Abrash.pdf)
- [Windows assembly language](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Windows%20Assembly%20Language%20%26%20Systems%20Programming-%2016%20And%2032%20Bit%20Low-Level%20Programming%20for%20the%20PC%20and%20Windows%201997%20-%20Barry%20Kauler.pdf)
- [Windows assembly language and system programming](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Windows%20assembly%20language%20and%20systems%20programming%201997.pdf)
- [Write great code understanding the machine](https://theswissbay.ch/pdf/Gentoomen%20Library/Programming/Assembly/Write%20Great%20Code%20Understanding%20the%20Machine%2C%20Volume%20I.chm)

## hello world!

**assembler mips**

```asm
.data
    hw:        .asciiz        "Hello, World!"
.text
main:
    la $a0, hw           #load the address of hw into $a0
    li $v0, 4            #load 4 into $v0
    syscall              #perform the print_string syscall
    li $v0, 10           #load 10 into $v0
    syscall              #perform the exit syscall
```

**assembler masm win64**

```asm
GetStdHandle PROTO
ExitProcess PROTO
WriteConsoleA PROTO

.data
msg BYTE "Hello World!",0
bytesWritten DWORD ?

.code
main PROC
    sub rsp, 5 * 8              ; reserve shadow space

    mov rcx, -11                ; nStdHandle (STD_OUTPUT_HANDLE)
    call GetStdHandle

    mov  rcx, rax               ; hConsoleOutput
    lea  rdx, msg               ; *lpBuffer
    mov  r8, LENGTHOF msg - 1   ; nNumberOfCharsToWrite
    lea  r9, bytesWritten       ; lpNumberOfCharsWritten
    mov  QWORD PTR [rsp + 4 * SIZEOF QWORD], 0  ; lpReserved
    call WriteConsoleA

    mov rcx, 0                  ; uExitCode
    call ExitProcess
main ENDP
END
```

**assembler masm win32**

```asm
.386
.model flat,stdcall
.stack 4096

EXTRN ExitProcess@4 : PROC
EXTRN GetStdHandle@4 : PROC
EXTRN WriteConsoleA@20 : PROC

.data
msg BYTE "Hello World!",0
bytesWritten DWORD ?

.code
main PROC
    push -11                 ; nStdHandle (STD_OUTPUT_HANDLE)
    call GetStdHandle@4

    push 0                   ; lpReserved
    push OFFSET bytesWritten ; lpNumberOfCharsWritten
    push LENGTHOF msg - 1    ; nNumberOfCharsToWrite
    push OFFSET msg          ; *lpBuffer
    push eax                 ; hConsoleOutput
    call WriteConsoleA@20

    push 0                   ; uExitCode
    call ExitProcess@4
main ENDP
END main
```

**assembler nasm linux**

```asm
.386
.model flat,stdcall
.stack 4096

EXTRN ExitProcess@4 : PROC
EXTRN GetStdHandle@4 : PROC
EXTRN WriteConsoleA@20 : PROC

.data
msg BYTE "Hello World!",0
bytesWritten DWORD ?

.code
main PROC
    push -11                 ; nStdHandle (STD_OUTPUT_HANDLE)
    call GetStdHandle@4

    push 0                   ; lpReserved
    push OFFSET bytesWritten ; lpNumberOfCharsWritten
    push LENGTHOF msg - 1    ; nNumberOfCharsToWrite
    push OFFSET msg          ; *lpBuffer
    push eax                 ; hConsoleOutput
    call WriteConsoleA@20

    push 0                   ; uExitCode
    call ExitProcess@4
main ENDP
END main
```

**assembler nasm linux 64**

```asm
section .rodata
    msg db "Hello World", 0xA       ; String to print
    len equ $- msg                  ; Length of string

section	.text
    global _start                   ; Specify entry point to linker

_start:
	mov	eax, 1                      ; System call ID (sys_write)
	mov	edi, eax                    ; File descriptor (stdout)
	mov	esi, msg                    ; Text to print
    mov edx, len                    ; Length of text to print
	syscall                         ; Call kernel

    mov eax, 60                     ; System call ID (sys_exit)
	xor	edi, edi                    ; Error code (EXIT_SUCCESS)
	syscall                         ; Call kernel
```
