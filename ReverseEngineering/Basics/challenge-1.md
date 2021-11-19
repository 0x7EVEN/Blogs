Challenge : Random
Dificulty : Beginner 
//will soon add challenge binary in same directory as blog;
  
---
#Prerequisite
- Basics of Reverse Engineering 
- Debugger / Decompiler // for this challenge we will use Radare2 and GNU debugger GDB.
- Assembly
---

after dowloading the challenge we need to add executable perm by 
```shell
Chmod +x <binary name here>
```

opening binary in Radare2 and disassembling main() with
`r2 -d filename`

then with analyze all `aa`
and print disaasemble main with `pdf`
we can see there are two checkes : `user` and `pass`
```
[0x08048150]> s main
[0x08048309]> pdf
/ (fcn) main 225
|   main (int argc, char **argv, char **envp);
|           ; var int local_10h @ ebp-0x10
|           ; var int local_ch @ ebp-0xc
|           ; var char *s1 @ ebp-0x8
|           ; var char *s2 @ esp+0x4
|           ; DATA XREF from entry0 (0x8048167)
|           0x08048309      8d4c2404       lea ecx, [s2]               ; 4
|           0x0804830d      83e4f0         and esp, 0xfffffff0
|           0x08048310      ff71fc         push dword [ecx - 4]
|           0x08048313      55             push ebp
|           0x08048314      89e5           mov ebp, esp
|           0x08048316      51             push ecx
|           0x08048317      83ec24         sub esp, 0x24               ; '$'
|           0x0804831a      c745f4196b0a.  mov dword [local_ch], str.john ; 0x80a6b19 ; "john"
|           0x08048321      c745f01e6b0a.  mov dword [local_10h], str.the_ripper ; 0x80a6b1e ; "the ripper"
|           0x08048328      c704242c6b0a.  mov dword [esp], 0x80a6b2c  ; [0x80a6b2c:4]=0x23232323 ; "############################################################" ; const char *s
|           0x0804832f      e8ac0a0000     call sym.puts               ; int puts(const char *s)
|           0x08048334      c704246c6b0a.  mov dword [esp], str.Bienvennue_dans_ce_challenge_de_cracking ; [0x80a6b6c:4]=0x20202323 ; "##        Bienvennue dans ce challenge de cracking        ##" ; const char *s
|           0x0804833b      e8a00a0000     call sym.puts               ; int puts(const char *s)
|           0x08048340      c70424ac6b0a.  mov dword [esp], str.       ; [0x80a6bac:4]=0x23232323 ; "############################################################\n" ; const char *s
|           0x08048347      e8940a0000     call sym.puts               ; int puts(const char *s)
|           0x0804834c      c70424ea6b0a.  mov dword [esp], str.username: ; [0x80a6bea:4]=0x72657375 ; "username: "
|           0x08048353      e8580a0000     call sym.__printf
|           0x08048358      8b45f8         mov eax, dword [s1]
|           0x0804835b      890424         mov dword [esp], eax
|           0x0804835e      e807ffffff     call sym.getString
|           0x08048363      8945f8         mov dword [s1], eax
|           0x08048366      8b45f4         mov eax, dword [local_ch]
|           0x08048369      89442404       mov dword [s2], eax         ; const char *s2
|           0x0804836d      8b45f8         mov eax, dword [s1]
|           0x08048370      890424         mov dword [esp], eax        ; const char *s1
|           0x08048373      e8787f0000     call sym.strcmp             ; int strcmp(const char *s1, const char *s2)
|           0x08048378      85c0           test eax, eax
|       ,=< 0x0804837a      7554           jne 0x80483d0
|       |   0x0804837c      c70424f56b0a.  mov dword [esp], str.password: ; [0x80a6bf5:4]=0x73736170 ; "password: "
|       |   0x08048383      e8280a0000     call sym.__printf
|       |   0x08048388      8b45f8         mov eax, dword [s1]
|       |   0x0804838b      890424         mov dword [esp], eax
|       |   0x0804838e      e8d7feffff     call sym.getString
|       |   0x08048393      8945f8         mov dword [s1], eax
|       |   0x08048396      8b45f0         mov eax, dword [local_10h]
|       |   0x08048399      89442404       mov dword [s2], eax         ; const char *s2
|       |   0x0804839d      8b45f8         mov eax, dword [s1]
|       |   0x080483a0      890424         mov dword [esp], eax        ; const char *s1
|       |   0x080483a3      e8487f0000     call sym.strcmp             ; int strcmp(const char *s1, const char *s2)
|       |   0x080483a8      85c0           test eax, eax
|      ,==< 0x080483aa      7516           jne 0x80483c2
|      ||   0x080483ac      c7442404006c.  mov dword [s2], str.987654321 ; [0x80a6c00:4]=0x36373839 ; "987654321"
|      ||   0x080483b4      c704240c6c0a.  mov dword [esp], str.Bien_joue__vous_pouvez_valider_l_epreuve_avec_le_mot_de_passe_:__s ; [0x80a6c0c:4]=0x6e656942 ; "Bien joue, vous pouvez valider l'epreuve avec le mot de passe : %s !\n"
|      ||   0x080483bb      e8f0090000     call sym.__printf
|     ,===< 0x080483c0      eb1a           jmp 0x80483dc
|     |||   ; CODE XREF from main (0x80483aa)
|     |`--> 0x080483c2      c70424526c0a.  mov dword [esp], str.Bad_password ; [0x80a6c52:4]=0x20646142 ; "Bad password" ; const char *s
|     | |   0x080483c9      e8120a0000     call sym.puts               ; int puts(const char *s)
|     |,==< 0x080483ce      eb0c           jmp 0x80483dc
|     |||   ; CODE XREF from main (0x804837a)
|     ||`-> 0x080483d0      c704245f6c0a.  mov dword [esp], str.Bad_username ; [0x80a6c5f:4]=0x20646142 ; "Bad username" ; const char *s
|     ||    0x080483d7      e8040a0000     call sym.puts               ; int puts(const char *s)
|     ||    ; CODE XREFS from main (0x80483c0, 0x80483ce)
|     ``--> 0x080483dc      b800000000     mov eax, 0
|           0x080483e1      83c424         add esp, 0x24               ; '$'
|           0x080483e4      59             pop ecx
|           0x080483e5      5d             pop ebp
|           0x080483e6      8d61fc         lea esp, [ecx - 4]
\           0x080483e9      c3             ret
[0x08048309]> 
```

as we see there are two tests which check register EAX value and makes a jump based on result stored in EAX.
we can modify EAS register with set command.

for solving this challenge and for revisiting GDB, opening this in GDB (no plugins).
we can set breakpoint at respective address with `b <address>`
and then we can use `ni` for next instruction.
and we see we esacped check and it printed something in frech/spanish whaterver, its password !!!.

![Solved ! <image>](https://github.com/0x7EVEN/Blogs/blob/main/images/solved.jpg?raw=true)
