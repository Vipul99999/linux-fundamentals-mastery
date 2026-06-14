# Linux Process Management — Ultimate Interview Questions

> This file contains interview questions ranging from:
>
> ```text
> Beginner
> Intermediate
> Advanced
> Expert
> Production/SRE
> Docker/Kubernetes
> Kernel Internals
> ```
>
> Topics covered:
>
> ```text
> Process Basics
> Process Lifecycle
> Process States
> PCB
> Context Switching
> Scheduling
> Fork/Exec/Wait
> Threads
> Jobs
> Signals
> kill
> nice/renice
> Zombies
> Orphans
> Namespaces
> Cgroups
> Containers
> Kubernetes
> Troubleshooting
> ```

---

# Section 1 — Process Fundamentals

### Q1. What is a process?

A process is a running instance of a program along with its memory, resources, registers, and execution state.

---

### Q2. Difference between a program and a process?

Program:

```text
Passive file on disk
```

Process:

```text
Active execution of a program
```

---

### Q3. Can multiple processes run the same program?

Yes.

Example:

```bash
firefox
firefox
firefox
```

Each instance is a separate process.

---

### Q4. What uniquely identifies a process?

```text
PID (Process ID)
```

---

### Q5. Can two running processes have the same PID?

No.

PID is unique at a given time.

---

### Q6. What is PPID?

```text
Parent Process ID
```

---

### Q7. How do you view process hierarchy?

```bash
pstree
```

or

```bash
ps axjf
```

---

### Q8. What process usually has PID 1?

```text
systemd
```

(modern Linux)

---

### Q9. Why is PID 1 special?

Responsible for:

```text
Orphan adoption
Zombie reaping
System initialization
```

---

### Q10. What command shows running processes?

```bash
ps
```

---

# Section 2 — Process Lifecycle

### Q11. What are the major stages of a process lifecycle?

```text
New
Ready
Running
Waiting
Terminated
```

---

### Q12. What system call creates a process?

```c
fork()
```

---

### Q13. What system call replaces a process image?

```c
exec()
```

family.

---

### Q14. What system call terminates a process?

```c
exit()
```

---

### Q15. What system call waits for child termination?

```c
wait()
```

or

```c
waitpid()
```

---

### Q16. What happens after fork()?

A child process is created.

Both parent and child continue execution.

---

### Q17. What value does fork() return in parent?

Child PID.

---

### Q18. What value does fork() return in child?

```text
0
```

---

### Q19. What happens if fork() fails?

Returns:

```text
-1
```

---

### Q20. What resources are copied during fork()?

Mostly logical copies using:

```text
Copy-On-Write (COW)
```

---

# Section 3 — Process States

### Q21. What does R mean in ps?

```text
Running
```

---

### Q22. What does S mean?

```text
Interruptible Sleep
```

---

### Q23. What does D mean?

```text
Uninterruptible Sleep
```

Usually waiting on I/O.

---

### Q24. What does T mean?

```text
Stopped/Traced
```

---

### Q25. What does Z mean?

```text
Zombie
```

---

### Q26. Which state is often associated with disk issues?

```text
D State
```

---

### Q27. Can kill -9 immediately terminate D-state processes?

Often no.

Because process is inside a kernel operation.

---

### Q28. Which state consumes CPU?

Typically:

```text
R
```

---

### Q29. Which state is most common?

```text
Sleeping
```

---

### Q30. Why are most processes sleeping?

Waiting for events, I/O, timers, network activity.

---

# Section 4 — PCB (Process Control Block)

### Q31. What is PCB?

```text
Process Control Block
```

Kernel structure storing process metadata.

---

### Q32. What information does PCB contain?

```text
PID
State
Registers
Scheduling Info
Memory Info
Open Files
Signals
```

---

### Q33. Is PCB stored in user space?

No.

Kernel space.

---

### Q34. Why is PCB important?

Allows process management and scheduling.

---

### Q35. What happens to PCB when process exits?

Eventually removed after cleanup.

---

# Section 5 — Context Switching

### Q36. What is a context switch?

Switching CPU execution from one process/thread to another.

---

### Q37. Why is context switching needed?

CPU can run limited tasks simultaneously.

---

### Q38. What is saved during context switch?

```text
Registers
Program Counter
Stack Pointer
CPU State
```

---

### Q39. Is context switching free?

No.

It has overhead.

---

### Q40. Why are excessive context switches bad?

Reduce CPU efficiency.

---

### Q41. How do you measure context switches?

```bash
vmstat
```

or

```bash
pidstat
```

---

### Q42. What is voluntary context switch?

Process willingly yields CPU.

---

### Q43. What is involuntary context switch?

Scheduler preempts process.

---

# Section 6 — CPU Scheduling

### Q44. Which scheduler is used by modern Linux?

```text
CFS
(Completely Fair Scheduler)
```

---

### Q45. Goal of CFS?

Fair CPU allocation.

---

### Q46. What does nice value affect?

Scheduling priority.

---

### Q47. Nice value range?

```text
-20 to 19
```

---

### Q48. Highest priority nice value?

```text
-20
```

---

### Q49. Default nice value?

```text
0
```

---

### Q50. Which gets more CPU?

```text
Nice 0
```

vs

```text
Nice 10
```

Answer:

```text
Nice 0
```

---

# Section 7 — Threads

### Q51. Difference between process and thread?

Process:

```text
Separate memory space
```

Thread:

```text
Shared process memory
```

---

### Q52. Why use threads?

Lower overhead.

---

### Q53. Can threads share memory?

Yes.

---

### Q54. Can processes share memory by default?

No.

---

### Q55. How do you view threads?

```bash
ps -eLf
```

---

### Q56. What is a race condition?

Multiple threads access shared data unsafely.

---

### Q57. What prevents race conditions?

```text
Mutexes
Locks
Semaphores
```

---

# Section 8 — Jobs & Job Control

### Q58. What is a job?

Shell-managed unit of work.

---

### Q59. Difference between job and process?

Job:

```text
Shell concept
```

Process:

```text
Kernel concept
```

---

### Q60. Command to view jobs?

```bash
jobs
```

---

### Q61. Command to move job to foreground?

```bash
fg
```

---

### Q62. Command to move job to background?

```bash
bg
```

---

### Q63. What signal does Ctrl+Z send?

```text
SIGTSTP
```

---

### Q64. What signal does Ctrl+C send?

```text
SIGINT
```

---

# Section 9 — Signals

### Q65. What is a signal?

Asynchronous notification sent to a process.

---

### Q66. Which signal is graceful termination?

```text
SIGTERM
```

---

### Q67. Signal number of SIGKILL?

```text
9
```

---

### Q68. Can SIGKILL be ignored?

No.

---

### Q69. Can SIGSTOP be ignored?

No.

---

### Q70. Difference between SIGTERM and SIGKILL?

SIGTERM:

```text
Graceful
```

SIGKILL:

```text
Immediate
```

---

### Q71. Which signal reloads many daemons?

```text
SIGHUP
```

---

### Q72. Which signal resumes stopped process?

```text
SIGCONT
```

---

### Q73. Which signal indicates segmentation fault?

```text
SIGSEGV
```

---

# Section 10 — kill Command

### Q74. Does kill actually kill processes?

No.

It sends signals.

---

### Q75. Default signal used by kill?

```text
SIGTERM
```

---

### Q76. How to send SIGKILL?

```bash
kill -9 PID
```

---

### Q77. Why avoid kill -9 first?

No cleanup opportunity.

---

### Q78. What is pkill?

Kill processes by pattern.

---

### Q79. What is killall?

Kill processes by name.

---

### Q80. Best shutdown sequence?

```text
SIGTERM

Wait

SIGKILL if needed
```

---

# Section 11 — Zombie Processes

### Q81. What is a zombie process?

Exited child whose parent hasn't collected exit status.

---

### Q82. Zombie state?

```text
Z
```

---

### Q83. Does zombie consume CPU?

No.

---

### Q84. Can zombie be killed?

No.

Already dead.

---

### Q85. What removes zombies?

```text
wait()
waitpid()
```

---

### Q86. Why do zombies exist?

Allow parent to retrieve exit status.

---

### Q87. Can zombies exhaust resources?

Yes.

Process table entries.

---

### Q88. Difference between zombie and orphan?

Zombie:

```text
Dead Child
Alive Parent
```

Orphan:

```text
Alive Child
Dead Parent
```

---

# Section 12 — Orphan Processes

### Q89. What is an orphan process?

Running child whose parent exited.

---

### Q90. What happens to orphans?

Adopted by PID 1.

---

### Q91. Which process adopts orphans?

Usually:

```text
systemd
```

---

### Q92. Are orphans dangerous?

Usually no.

---

### Q93. Why do daemons intentionally create orphans?

Detach from terminal.

---

# Section 13 — Namespaces

### Q94. What is a namespace?

Kernel feature providing isolation.

---

### Q95. What namespace isolates processes?

```text
PID Namespace
```

---

### Q96. What namespace isolates networking?

```text
Network Namespace
```

---

### Q97. What namespace isolates hostname?

```text
UTS Namespace
```

---

### Q98. What namespace isolates filesystem mounts?

```text
Mount Namespace
```

---

### Q99. What namespace isolates users?

```text
User Namespace
```

---

### Q100. Why are namespaces important?

Foundation of containers.

---

# Section 14 — Cgroups

### Q101. What does cgroup stand for?

```text
Control Group
```

---

### Q102. Purpose of cgroups?

Resource control.

---

### Q103. What resources can cgroups control?

```text
CPU
Memory
I/O
PIDs
Network
```

---

### Q104. Difference between namespace and cgroup?

Namespace:

```text
Isolation
```

Cgroup:

```text
Resource Limits
```

---

### Q105. What is cgroup v2?

Unified modern cgroup hierarchy.

---

# Section 15 — Docker & Containers

### Q106. Are containers virtual machines?

No.

---

### Q107. What are containers really?

Linux processes with isolation.

---

### Q108. Do containers have their own kernel?

No.

Shared host kernel.

---

### Q109. What creates isolation in containers?

Namespaces.

---

### Q110. What controls resources in containers?

Cgroups.

---

### Q111. What is PID 1 inside a container?

Container's init/application process.

---

### Q112. Why is PID 1 important?

Handles:

```text
Signals
Zombies
Orphans
```

---

### Q113. What problem does tini solve?

PID 1 signal handling and reaping.

---

### Q114. What does docker stop send first?

```text
SIGTERM
```

---

### Q115. What happens if container ignores SIGTERM?

Eventually receives:

```text
SIGKILL
```

---

# Section 16 — Kubernetes

### Q116. What is a Pod?

Smallest deployable Kubernetes unit.

---

### Q117. Can a Pod contain multiple containers?

Yes.

---

### Q118. Why does Kubernetes use a pause container?

Holds shared namespaces.

---

### Q119. Do containers in same Pod share network namespace?

Yes.

---

### Q120. What signal is sent during pod termination?

```text
SIGTERM
```

---

# Section 17 — Troubleshooting Scenarios

### Q121. Server CPU is 100%. First command?

```bash
top
```

or

```bash
htop
```

---

### Q122. Find top CPU consumers?

```bash
ps -eo pid,%cpu,cmd --sort=-%cpu
```

---

### Q123. Find top memory consumers?

```bash
ps -eo pid,%mem,cmd --sort=-%mem
```

---

### Q124. How to find zombies?

```bash
ps aux | grep Z
```

---

### Q125. Process won't die even with kill -9. Why?

Possibly:

```text
D State
```

(kernel wait).

---

### Q126. Container won't stop. First suspicion?

PID 1 signal handling issue.

---

### Q127. System creates thousands of zombies. Root cause?

Parent not calling:

```text
wait()
```

---

### Q128. High load average but low CPU usage. Possible reason?

I/O bottleneck.

---

### Q129. Memory usage increasing continuously. Likely cause?

Memory leak.

---

### Q130. Which command shows process tree?

```bash
pstree
```

---

# Advanced Round

### Q131. Explain Copy-On-Write during fork().

### Q132. Explain scheduler fairness in CFS.

### Q133. Difference between kernel thread and user thread.

### Q134. Explain signal masking.

### Q135. Explain pending signals.

### Q136. Why can't SIGKILL be caught?

### Q137. Explain process group.

### Q138. Explain session leader.

### Q139. Explain controlling terminal.

### Q140. What is namespace re-parenting?

### Q141. How does systemd reap zombies?

### Q142. What is OOM Killer?

### Q143. How does Docker use namespaces internally?

### Q144. Explain containerd and runc roles.

### Q145. Why are D-state processes difficult to kill?

### Q146. Explain CPU affinity.

### Q147. Explain context-switch overhead.

### Q148. Explain run queue.

### Q149. Explain PID namespace hierarchy.

### Q150. Explain how Kubernetes pods share namespaces.

---

# Expert Challenge Questions

### Q151. Design a process supervisor.

### Q152. Build a minimal init system.

### Q153. Design a zombie-safe server.

### Q154. Debug PID exhaustion on production Linux.

### Q155. Diagnose container signal-handling failures.

### Q156. Compare Linux scheduling with Windows scheduling.

### Q157. Explain how fork() works internally.

### Q158. Explain process creation path from syscall to scheduler.

### Q159. Explain namespace creation inside kernel.

### Q160. Explain cgroup v2 architecture.

---

# Final Interview Tip

If you deeply understand:

```text
Processes
PCB
Scheduling
fork()
exec()
wait()
Signals
Threads
Zombies
Orphans
PID 1
Namespaces
Cgroups
Containers
```

then you understand a huge portion of:

```text
Linux Internals
Docker
Kubernetes
DevOps
SRE
Backend Systems
Operating Systems
```

and can confidently answer most Linux process-management interview questions from beginner to senior engineer level.
