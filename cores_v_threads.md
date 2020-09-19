# Brief Note on the Difference Between Cores, Sockets, vCPUs, and Threads

## How do you calculate logical cores? 
If you open windows resmon or in proxmox, you might see something like this:
> CPU(s) 12 x AMD Ryzen 5 3600 6-Core Processor (1 Socket)

This means it's a 6 core CPU on 1 socket, running 2 threads per core. Here's what that means:

- The **cores** are the actual physical number of cores. To clarify, the definition of cores aren't really well defined, and are up to the manufacturer. [Wikichip](https://en.wikichip.org/wiki/physical_core) describes a core as a "well-partitioned piece of logic capable of independently performing all functions of a processor." It's essentially the number of ALUs that a CPU can address and multiplex at a time.
- The **socket** is the actual physical socket on a mobo. Most consumer boards have 1 socket, but server boards can have multiple CPU sockets.
- The **threads** are the number of Simultanious Multithreading (SMT) threads suppported per core. SMT for intel chips is HyperThreading, and for AMD it's just called AMD SMT.

## How does SMT work
(note: I didn't do a whole lot of research on this part, could be expanded)

SMT isn't actually multiple virtialised cores sitting on a single core, even though that's how it's presented. It's more like "hardware scheduler dynamically assign threads to run on a core so that the one core can do up to theoretically N numbers of threads where N is how many threads the hardware scheduler can assign to a core at the same time." Pretty much all modern CPUs only have N=2 threads per core, but apparently AMD is working on CPUs that can do N=4.

## What do the number of logical cores tell you
The number of threads that can be parallelised. 

## Is it more efficient to parallelise a task through treads or processes?
It depends on the CPU, application design and task at hand. On CPU level, it doesn't really matter as CPUs just run threads as it needs to based on what's in the cache and the next instruction to run you can easily find applications that run well on multiprocess and applications that runs well on multithreading. The only real way to figure it out is to run benchmarks to guage the performance of the SMT scheduler, etc.

## Why do hypervisors let you speificy the number of sockets?
A lot of hypervisors, such as proxmox, VM ware ones, ESX etc let you specify the number of cores, as well as the number of sockets. So you can do things like pass 4 Cores on 1 socket, or pass 1 core per socket, but assign the VM 4 sockets. There's two reason's why you might want to do that. 
1. Licensing and OS limitations. Some OS's don't like you passing a certain number of cores per socket, or charger per core per socket. So an OS might not like you passing a 8 core CPU, but is perfectly happy supporting dual core CPUs on 4 sockets.
2. NUMA. 
