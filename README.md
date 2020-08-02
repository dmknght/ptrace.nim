# ptrace.nim
ptrace wrapper and helpers for Nim


## Installation

    $ nimble install ptrace

## Example

```nim
import ptrace
import posix

var
  child: Pid
  orig_ax: clong

child = fork()
if child == 0:
  traceMe()
  discard execl("/bin/ls", "ls")
else:
  wait(nil)

  var regs: Registers
  getRegs(child, addr regs)
  echo "Syscall number: ", regs.orig_rax
  if errno != 0:
    echo "getRegs: ", strerror(errno)

  orig_ax = peekUser(child, SYSCALL_NUM)
  if errno != 0:
    echo "peekUser: ", errno, " ", strerror(errno)
  echo "The child made a system call: ", orig_ax
  cont(child)

```
