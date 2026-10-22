Depth is a good bet, because it's the one thing that's hard to fake. But depth only helps if people can see it, so the ordering below leans toward things you can show.

**1. Finish the C server and make it solid.**  
A finished, tested, documented project beats three half-built ones. Include `epoll`, proper request parsing, timeouts, and graceful handling of bad input. Then benchmark it with `wrk` against nginx and write down what you find, even if you lose. That kind of analysis is what separates you from someone who only prompts a model.

**2. Do the CSAPP labs, not just the book.**  
The CMU labs (data, bomb, attack, malloc, proxy) are free for self-study. The malloc lab maps onto your own allocator, the proxy lab onto your server, and the attack lab onto the binary-exploitation side. Reading CSAPP and writing notes is useful, but the labs are where it sticks.

**3. Fuzz your HTTP parser.**  
Run AFL++ or libFuzzer against it and fix what breaks. That ties your memory-safety goal to concrete bugs you found and fixed, which makes for a strong story in an interview, and it's also a good way to find out whether security work appeals to you.

**4. Learn the tools until they're automatic.**  
`gdb`, `strace`, `perf`, `objdump`, Wireshark, and sanitizers (build a libc version with ASan to catch bugs your freestanding build hides). Being fast with these is a visible skill. Debugging ability is one of the clearest gaps between a deep engineer and a surface one.

**5. Read real code.**  
musl libc is small and readable, and ideal next to your mini-libc. Then look at pieces of nginx, redis, and the Linux kernel. Reading good code teaches taste you can't get from your own projects alone.

**6. Follow a structured OS path before the hobby OS.**  
OSTEP (free book), then xv6 and the MIT 6.1810 labs, then the OSDev wiki for bare metal on x86-64. xv6 is a small working kernel, so you learn how the pieces fit before designing your own.

**7. Write about what you build.**  
A README or short post per project covering the design choices, the bugs, the benchmarks, and what you'd change. It costs little and makes the work visible to someone who won't read your source.

**8. Keep a few mainstream skills warm.**  
Git, Linux administration, basic SQL, and Docker. Most backend and systems jobs expect them, and without them a strong candidate can still get filtered out early.

**A caution about notes.**  
It's easy to spend months writing notes and never build. A rough rule: for every hour of reading, spend about the same time writing code that uses it. Understanding you've never tested usually has holes in it.

Languages come after all of this. Rust is the one I'd add once the C server is done.

If you want, I can sketch a 3-month plan that combines the server, the CSAPP labs, and the first bare-metal steps.