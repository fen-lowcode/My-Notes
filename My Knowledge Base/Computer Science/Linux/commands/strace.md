#linux #linux-command

`source`: https://systeminternals.dev/linux/strace/
`also refer to manpage`:  man strace

`strace` is the universal "what is this thing actually doing?" tool on Linux. **==It prints every system call a process makes, with arguments and return values, in human-readable form==**. 


It works on any binary Go, Rust, Python, a closed-source blob because all of them ultimately go through the same kernel syscall interface.

The mechanism is `ptrace(2)`, the same primitive that powers gdb. Every syscall triggers two stops in the target process: one before, one after. The tracer reads the target's registers, decodes the arguments, prints them, and resumes. 

This is powerful but expensive easily 20x slowdown for syscall-heavy programs. For production observability, modern [[eBPF]] tools (`bpftrace`, `bcc`) do the same job at a fraction of the cost.


---

## How strace Works

A four-stage dance between tracer, target, and kernel. Every single syscall.

![[Pasted image 20260911032038.png]]



##### strace use cases

```
# Trace a command from start to finish
$ strace ls /tmp

# Follow forks and threads (essential for any non-trivial program)
$ strace -f make -j4

# Attach to a running process
$ strace -p 1234

# Filter to just file-related syscalls
$ strace -e trace=open,openat,close,read,write,stat,lstat,fstat ./app

# Use syscall classes (faster to type)
$ strace -e trace=%file ./app          # all file ops
$ strace -e trace=%network ./app       # all socket ops
$ strace -e trace=%process ./app       # fork/exec/wait
$ strace -e trace=%signal ./app        # signal delivery

# Show only failed syscalls (great for "why won't it open?")
$ strace -e trace=openat -e fault=openat:error=EACCES ./app   # inject failure
$ strace -Z ./app                                              # only show errors

# Aggregate counts and timings (what's the program spending time on?)
$ strace -c ./app
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 84.23    0.012345           4      3145           read
 10.11    0.001482           2       846       12 openat
  3.55    0.000520           1       512           close

# Print full strings (default is truncated to 32 bytes)
$ strace -s 1024 -e trace=write ./app

# Save to file, with timestamps and one PID per file
$ strace -ff -tt -o /tmp/trace ./app
$ ls /tmp/trace.*
trace.4123  trace.4124  trace.4125

# Decode socket address structures
$ strace -yy -e trace=%network curl https://example.com
```


#### strace vs bpftrace


|Aspect|strace|bpftrace|
|---|---|---|
|Mechanism|ptrace stops|eBPF programs attached to tracepoints/kprobes|
|Overhead|~20x for syscall-heavy programs|~1-5% even at high rates|
|Scope|One process (and children with -f)|System-wide or filtered to PIDs|
|Output|One line per syscall, full args|Aggregations: counts, histograms, summaries|
|Filtering|By syscall name and class|Arbitrary expressions in BPF DSL|
|Per-call args|All decoded by default|You write what you want|
|Production-safe?|No (slowdown affects user-visible behavior)|Yes (designed for it)|


```
# bpftrace: count syscalls by name across the whole system
bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[ksym(args->id)] = count(); }'

# bpftrace: latency histogram for read()
bpftrace -e '
  tracepoint:syscalls:sys_enter_read { @s[tid] = nsecs; }
  tracepoint:syscalls:sys_exit_read /@s[tid]/ { @ns = hist(nsecs - @s[tid]); delete(@s[tid]); }
'
```

##### Trade Off


**Pros**

- Universal — works on any binary, any language
- Pretty-prints hundreds of syscall arg structures for free
- No kernel-side setup; ships in every distro
- Shows exactly the userspace/kernel boundary, with arguments

**Cons**

- 20x overhead can mask the bug you're hunting (timing-dependent races vanish)
- Single-tracer limit conflicts with debuggers
- Useless for kernel-internal events (page faults, scheduling)
- Output volume swamps your terminal for busy programs