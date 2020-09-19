# FAQ
Some quick questions I encountered along the way, as well as their answeres.

## How is VCPU calculated from sockets and cores? Which should you have more of? 
tldr: $ sockets * cores * threads = vCPU $
- Socket is a physical slot for a CPU.
- Core is the physical core per CPU.
- Threads are the maximum number of HT thread allowed per core
See: cores_v_threads.md