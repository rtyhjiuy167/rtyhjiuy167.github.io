一个实际可运行的用户程序在执行完后，应该返回到 DOS 提示符状态(简称为返回DOS)，简单地用 HLT 指令使 CPU 停止运行将无法把控制权交还给 DOS 操作系统。为了能使程序正常退出并返回 DOS，可使用 DOS 系统功能调用的 4CH 号功能。用 4CH 号功能返回 DOS 的程序如下：

```assembly
CODE SEGMENT
    ASSUME CS:CODE
    START:
        MOV AH,4CH  ;功能号送 AH
        INT 21H		;返回 DOS
CODE ENDS
END START
```

