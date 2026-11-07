# xv6-labs-2020 (6.S081)

MIT [6.S081: Operating System Engineering (2020)](https://pdos.csail.mit.edu/6.S081/2020/schedule.html)
的实验代码。基于 [mit-pdos/xv6-labs-2020](https://github.com/mit-pdos/xv6-labs-2020) 的 `util` 等分支。

## 环境

在 Apple Silicon macOS 上用较新的工具链跑通了（原仓库面向 2020 年的 gcc 9 / qemu 5）：

```fish
brew install riscv64-elf-gcc riscv64-elf-gdb qemu
set -Ux TOOLPREFIX riscv64-elf-    # Makefile 默认猜 riscv64-unknown-elf-
make qemu                          # 退出: Ctrl-a x
```

为适配新工具链做了 4 处改动（不影响实验逻辑）：

| 文件 | 改动 | 原因 |
| --- | --- | --- |
| `Makefile` | 加 `-Wno-infinite-recursion` | GCC 14+ 把该警告并入 `-Wall`，`sh.c:runcmd` 的合法递归被误判，配上 `-Werror` 直接失败 |
| `Makefile` | 加 `-Wno-unused-but-set-variable` | 同上，`usertests.c` 的 `fsblocks` 触发 |
| `kernel/start.c`、`kernel/riscv.h` | `mret` 前配置 PMP（`pmpaddr0`/`pmpcfg0`） | QEMU 11 严格按规范：PMP 已实现但无匹配项时 S 模式访问一律失败，不补则内核在 `mret` 后第一条取指就 fault |
| `grade-lab-util` | shebang 改 `python3` | macOS 没有 `python` |

## 进度

| Lab | 内容 | 状态 |
| --- | --- | --- |
| util | sleep / pingpong / primes / find / xargs | 进行中 |
| syscall | System calls | 未开始 |
| pgtbl | Page tables | 未开始 |
| traps | Trap | 未开始 |
| lazy | Lazy allocation | 未开始 |
| cow | Copy-on-write fork | 未开始 |
| thread | Multithreading | 未开始 |
| lock | Parallelism/locking | 未开始 |
| fs | File system | 未开始 |
| mmap | Mmap | 未开始 |
| net | Network stack | 未开始 |

## 参考

- [课程主页](https://pdos.csail.mit.edu/6.S081/2020/schedule.html)
- [xv6 book (riscv-rev1)](https://pdos.csail.mit.edu/6.S081/2020/xv6/book-riscv-rev1.pdf)
- [xv6-riscv 源码](https://github.com/mit-pdos/xv6-riscv)
