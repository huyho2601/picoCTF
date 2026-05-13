In the first glance at source code, we can see that `vuln()` contains

`printf("Okay, time to return... Fingers Crossed... Jumping to 0x%x\n", get_return_address());`

This is the hint saying that this is the address where the program will jump to in the end. The goal is to overflow the buffer and insert the address of `win()` to make the program jump to win. Use `p win` to figure out address of `win`. In this challenge, the address of win should return `0x80491f6`

First run `gdb vuln.1 -x config.txt`. `config.txt` helps streamline the debugging by setting a breakpoint at `vuln()` and printing current stack information

When we inspect `get_return_address`, the assembly assigns whatever is at `[ebp+0x4]` to `eax`. This confirms that overwriting `vuln` and placing the address of `win()` there will redirect execution.

Notice how the stack of `vuln` is constructed. First `ebp` is pushed, then `ebx`, and finally `sub $0x24` allocates 36 bytes for `buf`. The total offset to the return address is 36 (buf) + 4 (saved EBX) + 4 (saved EBP) = **44 bytes**.

```
[ return address         ]  <- overwrite this with win()
[ saved EBP - 4 bytes    ]  <- push %ebp
[ saved EBX - 4 bytes    ]  <- push %ebx
[ buf - 36 bytes         ]  <- sub $0x24 allocates this
```

The following payload sends 44 A's followed by the address of `win()`. Note that x86 is little endian so the address bytes must be in reverse order. The `\n` at the end is required because `gets()` reads until a newline
`python3 -c "import sys; sys.stdout.buffer.write(b'A'*44 + b'\xf6\x91\x04\x08')"`
