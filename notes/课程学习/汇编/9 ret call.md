### RET 和 RETF

ret 指令用栈中的数据，修改 IP 的内容，从而实现近转移。
retf 指令用栈中的数据，修改 CS 和 IP 的内容，从而实现远转移。

1. ret

```assembly
;CPU执行 ret 指令时，相当于进行 POP IP
;该程序从 START处（即 MOV AX,STACK）开始执行，遇到 RET 后 再执行  MOV AX,4C00H 与 INT 21H 两句
STACK SEGMENT
        db 16 dup(0)
STACK ENDS
CODE SEGMENT
        ASSUME CS:CODE
        MOV AX,4C00H
        INT 21H
START:
        MOV AX,STACK
        MOV SS,AX
        MOV SP,16
        MOV AX,0
        PUSH AX
        RET
CODE ENDS
END START
```

2. retf

```assembly
;CPU执行 retf 指令时，相当于先执行 POP IP，再执行 POP CS
STACK SEGMENT
        db 16 dup(0)
STACK ENDS
CODE SEGMENT
        ASSUME CS:CODE
        MOV AX,4C00H
        INT 21H
START:
        MOV AX,STACK
        MOV SS,AX
        MOV SP,16
        PUSH CS         ;此处的 CS 指向当前代码段，无需更改
        MOV AX,0
        PUSH AX
        RETF
CODE ENDS
END START
```

### CALL





