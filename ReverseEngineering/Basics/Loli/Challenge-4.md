## crackme0x05
this crackmes is series of intresting crackmes for practicing radare2 and fun.
we start with `r2 -d <binary>`
for analyze all and print disassebly use  `aa` and `pdf`.
we see: 
```
    [0x080483d0]> aa
    [0x080483d0]> pdf@sym.main
    / function: sym.main (92)
    |     0x08048540  sym.main:
    |     0x08048540     55               push ebp
    |     0x08048541     89e5             mov ebp, esp
    |     0x08048543     81ec88000000     sub esp, 0x88
    |     0x08048549     83e4f0           and esp, 0xfffffff0
    |     0x0804854c     b800000000       mov eax, 0x0
    |     0x08048551     83c00f           add eax, 0xf
    |     0x08048554     83c00f           add eax, 0xf
    |     0x08048557     c1e804           shr eax, 0x4
    |     0x0804855a     c1e004           shl eax, 0x4
    |     0x0804855d     29c4             sub esp, eax
    |     0x0804855f     c704248e860408   mov dword [esp], str.IOLICrackmeLevel0x05
    |     0x08048566     e829feffff       call dword imp.printf
    |        ; imp.printf()
    |     0x0804856b     c70424a7860408   mov dword [esp], str.Password
    |     0x08048572     e81dfeffff       call dword imp.printf
    |        ; imp.printf()
    |     0x08048577     8d4588           lea eax, [ebp-0x78]
    |     0x0804857a     89442404         mov [esp+0x4], eax
    |     0x0804857e     c70424b2860408   mov dword [esp], 0x80486b2
    |     0x08048585     e8eafdffff       call dword imp.scanf
    |        ; imp.scanf()
    |     0x0804858a     8d4588           lea eax, [ebp-0x78]
    |     0x0804858d     890424           mov [esp], eax
    |     0x08048590     e833ffffff       call dword sym.check
    |        ; sym.check()
    |     0x08048595     b800000000       mov eax, 0x0
    |     0x0804859a     c9               leave
    \     0x0804859b     c3               ret
        ; ------------


    [0x080483d0]> pdf@sym.check
            ; CODE (CALL) XREF 0x08048590 (sym.main)
    / function: sym.check (120)
    |        0x080484c8  sym.check:
    |        0x080484c8     55               push ebp
    |        0x080484c9     89e5             mov ebp, esp
    |        0x080484cb     83ec28           sub esp, 0x28
    |        0x080484ce     c745f800000000   mov dword [ebp-0x8], 0x0
    |        0x080484d5     c745f400000000   mov dword [ebp-0xc], 0x0
    |  .     ; CODE (JMP) XREF 0x08048530 (sym.check)
    / loc: loc.080484dc (100)
    |  .     0x080484dc  loc.080484dc:
    |  .---> 0x080484dc     8b4508           mov eax, [ebp+0x8]
    |  |     0x080484df     890424           mov [esp], eax
    |  |     0x080484e2     e89dfeffff       call dword imp.strlen
    |  |        ; imp.strlen()
    |  |     0x080484e7     3945f4           cmp [ebp-0xc], eax
    |  | ,=< 0x080484ea     7346             jae loc.08048532
    |  | |   0x080484ec     8b45f4           mov eax, [ebp-0xc]
    |  | |   0x080484ef     034508           add eax, [ebp+0x8]
    |  | |   0x080484f2     0fb600           movzx eax, byte [eax]
    |  | |   0x080484f5     8845f3           mov [ebp-0xd], al
    |  | |   0x080484f8     8d45fc           lea eax, [ebp-0x4]
    |  | |   0x080484fb     89442408         mov [esp+0x8], eax
    |  | |   0x080484ff     c744240468860408 mov dword [esp+0x4], 0x8048668
    |  | |   0x08048507     8d45f3           lea eax, [ebp-0xd]
    |  | |   0x0804850a     890424           mov [esp], eax
    |  | |   0x0804850d     e892feffff       call dword imp.sscanf
    |  | |      ; imp.sscanf()
    |  | |   0x08048512     8b55fc           mov edx, [ebp-0x4]
    |  | |   0x08048515     8d45f8           lea eax, [ebp-0x8]
    |  | |   0x08048518     0110             add [eax], edx
    |  | |   0x0804851a     837df810         cmp dword [ebp-0x8], 0x10
    |  |,==< 0x0804851e     750b             jnz loc.0804852b
    |  |||   0x08048520     8b4508           mov eax, [ebp+0x8]
    |  |||   0x08048523     890424           mov [esp], eax
    |  |||   0x08048526     e859ffffff       call dword sym.parell
    |  |||      ; sym.parell()
    |  ||    ; CODE (JMP) XREF 0x0804851e (sym.check)
    / loc: loc.0804852b (21)
    |  ||    0x0804852b  loc.0804852b:
    |  |`--> 0x0804852b     8d45f4           lea eax, [ebp-0xc]
    |  | |   0x0804852e     ff00             inc dword [eax]
    |  `===< 0x08048530     ebaa             jmp loc.080484dc
    |    |   ; CODE (JMP) XREF 0x080484ea (sym.check)
    / loc: loc.08048532 (14)
    |    |   0x08048532  loc.08048532:
    |    `-> 0x08048532     c7042479860408   mov dword [esp], str.PasswordIncorrect!
    |        0x08048539     e856feffff       call dword imp.printf
    |           ; imp.printf()
    |        0x0804853e     c9               leave
    \        0x0804853f     c3               ret
            ; ------------
```
here we see parell function leads to something suspicious while missing check does not invokes it! 
instead it goes to "passwordIncorrect!". let's see what parell has to say.

 ```
    [0x080483d0]> pdf@sym.parell
        ; CODE (CALL) XREF 0x08048526 (sym.check)
    / function: sym.parell (68)
    |      0x08048484  sym.parell:
    |      0x08048484     55               push ebp
    |      0x08048485     89e5             mov ebp, esp
    |      0x08048487     83ec18           sub esp, 0x18
    |      0x0804848a     8d45fc           lea eax, [ebp-0x4]
    |      0x0804848d     89442408         mov [esp+0x8], eax
    |      0x08048491     c744240468860408 mov dword [esp+0x4], 0x8048668
    |      0x08048499     8b4508           mov eax, [ebp+0x8]
    |      0x0804849c     890424           mov [esp], eax
    |      0x0804849f     e800ffffff       call dword imp.sscanf
    |         ; imp.sscanf()
    |      0x080484a4     8b45fc           mov eax, [ebp-0x4]
    |      0x080484a7     83e001           and eax, 0x1
    |      0x080484aa     85c0             test eax, eax
    |  ,=< 0x080484ac     7518             jnz loc.080484c6
    |  |   0x080484ae     c704246b860408   mov dword [esp], str.PasswordOK!
    |  |   0x080484b5     e8dafeffff       call dword imp.printf
    |  |      ; imp.printf()
    |  |   0x080484ba     c7042400000000   mov dword [esp], 0x0
    |  |   0x080484c1     e8eefeffff       call dword imp.exit
    |  |      ; imp.exit()
    |  |   ; CODE (JMP) XREF 0x080484ac (sym.parell)
    / loc: loc.080484c6 (2)
    |  |   0x080484c6  loc.080484c6:
    |  `-> 0x080484c6     c9               leave
    \      0x080484c7     c3               ret
        ; ------------

\
 ```
so sum should be equale to 0x10 and should be even!
 
 ```shell
 root@kali# ./crackme0x05
    IOLI Crackme Level 0x05
    Password: 88
    Password OK!
```
![pwned](https://github.com/0x7EVEN/Blogs/blob/main/ReverseEngineering/Basics/Loli/pwned.jpg?raw=true)


: )
