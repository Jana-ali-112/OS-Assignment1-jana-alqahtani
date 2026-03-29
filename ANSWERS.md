# Assignment Questions

## Instructions
Answer all 4 questions with detailed explanations. Each answer should be **3-5 sentences minimum** and demonstrate your understanding of the concepts.

---

## Question 1: Thread vs Process

**Question**: Explain the difference between a **thread** and a **process**. Why did we use threads in this assignment instead of creating separate processes?

A Process is an independent execution unit that has its own dedicated memory space, stack, and resources provided by the Operating System. In contrast, a Thread (often called a lightweight process) is a unit of execution within a process, where multiple threads share the same memory and resources but maintain their own execution stacks. In this assignment, we used threads because they are much more efficient for this simulation; creating a new process is resource-intensive ("high overhead"), whereas threads allow for faster context switching. For example, in our code, we implement the Runnable interface in the Process class and use new Thread(process) to represent our simulated processes. This allows all simulated processes to exist within a single JVM process, sharing the processMap and processQueue while executing concurrently. If we had used separate OS processes, sharing data like the contextSwitchCount would have been significantly more complex and slower.

## Question 2: Ready Queue Behavior

**Question**: In Round-Robin scheduling, what happens when a process doesn't finish within its time quantum? Explain using an example from your program output.

**Your Answer:**

In Round-Robin scheduling, if a process does not finish within its assigned Time Quantum, it is preempted by the scheduler to allow other processes a fair share of the CPU. The process is moved from the "Running" state back to the end of the Ready Queue, where it must wait for its next turn.

Example from my output:

  ? P1 executing quantum [4000ms] 
  ? Quantum progress: [???????????????] 100%
  ? P1 completed quantum 4000ms ? Overall progress: [????????????????????] 71%
     Remaining time: 1611ms
  ? P1 yields CPU for context switch

  ? P1 (Priority: 2) added to ready queue ? Burst time: 5611ms
  
**Explanation of example:**
Looking at our program output, when a process like P1 executes and its remainingTime is still greater than zero, the code prints: P1 yields CPU for context switch. At this point, the scheduler calls processQueue.add(thread), effectively re-queuing it. This ensures that no single process monopolizes the CPU for an extended period, which is the core principle of time-sharing systems. The process will eventually reach the front of the queue again to continue its execution from where it left off

## Question 3: Thread States

**Question**: A thread can be in different states: **New**, **Runnable**, **Running**, **Waiting**, **Terminated**. Walk through these states for one process (P1) from your simulation.

**Your Answer:**

A thread (P1) transitions through these states during the execution of our simulation:
• New State: The process (P1) is created as a thread object in the memory. It hasn't started execution yet.
• Runnable State: When we call P1.start(), the thread moves to the Runnable state. It is now waiting in the ready queue for the CPU scheduler to pick it up.
• Running State: The CPU begins executing the code inside the run() method of P1.
• Waiting/Blocked State: The thread enters this state when it is temporarily inactive:
• Thread.sleep(duration): Used in the simulation to mimic the execution time, putting P1 into a "Timed Waiting" state.
• Thread.join(): Used when one thread must wait for P1 to finish its execution before proceeding.
• Terminated State: Once the run() method finishes execution or an error occurs, P1 enters this final state and cannot be restarted.

## Question 4: Real-World Applications

**Question**: Give **TWO** real-world examples where Round-Robin scheduling with threads would be useful. Explain why this scheduling algorithm works well for those scenarios.

**Your Answer:**

Example 1: Web Servers (like Apache or Nginx). Round-Robin scheduling is highly useful here to handle thousands of incoming client requests fairly. By giving each request a small time slice, the server ensures that no single large file download blocks other users from loading small text pages. This maintains high responsiveness and prevents "starvation" of smaller tasks.
Example 2: Multi-user Operating Systems (like Linux or Windows). When multiple users are logged into a server or even a single user is running many apps (Spotify, Chrome, Word), Round-Robin ensures each application gets CPU time. This provides predictability and a smooth user experience, as the rapid context switching (tracked in our code by contextSwitchCount) creates the illusion that all programs are running perfectly in parallel.

## Summary

**Key concepts I understood through these questions:**

1. The fundamental distinction between Threads and Processes: I now understand that while both are units of execution, threads are much more efficient for simulation due to their ability to share memory and resources within a single process.
2. Round-Robin Scheduling Logic: I learned how the Time Quantum enforces fairness among tasks, ensuring that long-running processes are preempted and re-queued to maintain system responsiveness.
3. Thread Lifecycle and States: I can now trace a thread's journey from its creation (New) through its execution (Running) and waiting periods (Ready/Waiting) until it finally completes its task (Terminated).

**Concepts I need to study more:**

1. Advanced Synchronization Mechanisms: I want to dive deeper into how "Locks" and "Semaphores" function internally to prevent complex race conditions in large-scale multithreaded systems.
2. Context Switch Overhead: I am interested in learning more about the specific hardware-level costs involved when the OS saves and restores thread states, and how to minimize this overhead in real-time systems.
