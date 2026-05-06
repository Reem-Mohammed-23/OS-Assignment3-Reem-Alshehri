# Assignment 3 - Complete Documentation

**Student Name**: [Reem alshehri]  
**Student ID**: [444051646]  
**Date Submitted**: [7 may]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 6, 8:00 PM]
**What I implemented**:
I started by understanding the given code and identifying shared resources such as counters and execution logs

**Challenges encountered**: It was difficult to identify where race conditions might occur in the code

**How I solved it**: 
I carefully traced the execution of threads and analyzed shared variables

**Testing approach**:Ran the program multiple times to observe inconsistent results. 

**Time spent**: 1 hours


---

### Entry 2 - [May 6,9:00 PM]
**What I implemented**: I implemented synchronization using ReentrantLock to protect shared counters.

**Challenges encountered**: I forgot to release the lock properly, which caused the program to freeze.

**How I solved it**: I used try-finally blocks to ensure the lock is always released.

**Testing approach**: Executed the program multiple times to verify stability.

**Time spent**: 1 hours

---

### Entry 3 - [May 6, 10:00 PM]
**What I implemented**:
I implemented Semaphore to control access to CPU resources and limit concurrent threads. 

**Challenges encountered**: Understanding how permits affect thread execution

**How I solved it**: Tested different permit values and observed behavior

**Testing approach**:Compared outputs before and after using Semaphore 

**Time spent**: 1 hours

---

### Entry 4 - [May 6, 11:00 PM]
**What I implemented**:I added synchronization to execution logs to avoid ConcurrentModificationException 

**Challenges encountered**: Log entries were sometimes missing or duplicated.

**How I solved it**:Used synchronized blocks to protect the log. 

**Testing approach**: Checked log consistency after multiple runs

**Time spent**: 30 min

---

### Entry 5 - [May 6, 11:30 PM]
**What I implemented**: Final testing and verification of all synchronization mechanisms.

**Challenges encountered**: Ensuring all parts work together without deadlocks.

**How I solved it**: Reviewed lock ordering and tested edge cases.

**Testing approach**:Ran the program multiple times and verified consistent results.
 

**Time spent**: 30 min

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

A race condition occurs when multiple threads access and modify shared data at the same time without proper synchronization.

The first race condition is on the shared counter variables (such as total burst time or context switches). Multiple threads may update these variables simultaneously, leading to incorrect values. For example, two threads may read the same value and update it, causing one update to be lost.

The second race condition is in the execution log (such as a shared list). If multiple threads write to the log at the same time, it may cause inconsistent data or even a ConcurrentModificationException.

Concurrent access is a problem because threads do not execute in a predictable order. Without synchronization, the program may produce incorrect or inconsistent results.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to ensure mutual exclusion, meaning only one thread can access a critical section at a time. It provides more control than synchronized blocks, such as manual lock and unlock operations.

Semaphore, on the other hand, controls access to a resource by allowing a limited number of threads to enter a critical section. It uses permits instead of a single lock.

In my code, I used ReentrantLock to protect shared counters and execution logs because only one thread should modify them at a time. I used Semaphore to simulate CPU access, allowing a limited number of threads to execute concurrently. This improves control over resource allocation.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

Deadlock occurs when two or more threads are waiting for each other indefinitely, preventing the program from progressing.

One prevention technique is using try-finally blocks to ensure that locks are always released, even if an exception occurs. This avoids situations where a thread holds a lock forever.

Another technique is maintaining a consistent lock ordering, meaning all threads acquire locks in the same order. This prevents circular waiting.

In my code, I used try-finally blocks when working with locks and ensured that locks are released properly. This helped prevent deadlocks and ensured smooth execution.
[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**: 
I used separate locks for each counter (fine-grained locking).

The reason for this choice is that the counters are independent, meaning they can be updated separately without affecting each other. Using separate locks allows multiple threads to update different counters at the same time, improving concurrency.

In contrast, using a single lock (coarse-grained locking) would force all threads to wait even if they are accessing different counters. This reduces performance and increases waiting time.

The trade-off is that fine-grained locking is more complex to implement and manage, while coarse-grained locking is simpler but less efficient.

Since the counters are independent, fine-grained locking provides better performance and scalability.

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: Shared counters such as totalwaitingtime and context switches

**Why they need protection**: 
Because multiple threads update them simultaneously, which may lead to incorrect values

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
/```java
// Paste your implementation here
```lock.lock();
        try {
            contextSwitchCount++;
        } finally {
            lock.unlock();
        }

**Justification**: 

ensures multual exclusion and prevents race condition
---

### Critical Section #2: Execution Log

**What resource**: shared executionLog(ArrayList)

**Why it needs protection**: arraylist is not a thread -safe

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
```java
// Paste your implementation here
``` lock.lock();
        try {
            executionLog.add(message);
        } finally {
            lock.unlock();
        }

/**Justification**: prevent data corruption and exceptions

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: control cpu access 

**Number of permits and why**: (simulate single cpu ) 1

**Where implemented**: inside run() method 

**Code snippet**:
```java
// Paste your implementation here
```SharedResources.cpuSemaphore.acquire();
    SharedResources.cpuSemaphore.release();

**Effect on program behavior**: ensures only one process executes at a time 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
#compile the program 
     javac SchedulerSimulationSync.java

#run the program at least 5 times 
java  SchedulerSimulationSync
java  SchedulerSimulationSync
java  SchedulerSimulationSync
java  SchedulerSimulationSync
java  SchedulerSimulationSync

**Results**: 
(Show that running multiple times produces consistent, correct results)
═══ Synchronization Statistics ═══
Total Context Switches: 38
Total Completed Processes: 18
Total Waiting Time: 1732752ms
Average Waiting Time: 96264ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         ٥            ١٠٨٤٢        ١٢٥١١٩      
P2         ٢            ٣٧٦٣         ٥٠٩٠        
P3         ٤            ٣٣٤٤         ٨٨٧٧        
P4         ٤            ٧٦٥٨         ٨٨١٩٥       
P5         ١            ٤٩٦٤         ١٧٣٠٨       
P6         ٢            ٩٥١٦         ٩٠٨٦٥       
P7         ٣            ١٠٤١٠        ١٢٥٩٦٦      
P8         ٤            ٥٥٤١         ١٠٠٤٦٠      
P9         ٤            ٦٥٩٥         ١٠١٠٢٠      
P10        ٢            ١٠٢٦٧        ١٢٦٣٨٨      
P11        ١            ٥٠٥٢         ١٠٧٧٠٨      
P12        ٤            ٧٧٢٨         ١٠٧٧٦٦      
P13        ٢            ٦٧٠٢         ١١٠٥٣٢      
P14        ٣            ١٢٤٦٩        ١٢٦٦٦٨      
P15        ٢            ٦٣٣٩         ١١٧٣٠٣      
P16        ٤            ٦٩٤٩         ١١٨٦٦٦      
P17        ٣            ١١٣٨٤        ١٢٩١٥٠      
P18        ٥            ٩٤٢١         ١٢٥٦٧١      

═══ Execution Log Summary ═══
Total log entries: 76


**Why synchronization is necessary**
(Explain what race conditions COULD occur without synchronization, even ifyou didnt observe them. Explain which shared resources need protection and why.)

Without synchronization, race conditions may occur when multiple threads update shared resources like counters and logs. This could result in incorrect totals or missing log entries.



/**Conclusion**: 
Synchronization ensures reliable and consistent program behavior.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**:  i executed the program multipl time while focusing on oprations involving the shared executionlog list since multiple threads attempt to writ to this list i verified whether any runtime exception occored during execution

**Results**: 
Exception occurred without synchronization but disappeared after applying synchronization

**What this proves**: Synchronization is necessary to protect shared collections

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**:  the number of compeletd process should equal the total number of created processs

**Actual values**: ═══ Synchronization Statistics ═══
Total Context Switches: 38
Total Completed Processes: 18
Total Waiting Time: 1732752ms
Average Waiting Time: 96264ms


**Analysis**: Confirms that synchronization is correctly implemented

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]Different number of processes

**Purpose**: To test scalability

**Results**: Program handled multiple processes correctly

**What I learned**: Synchronization ensures stability regardless of input size

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
I learned that synchronization is essential when multiple threads access shared resources. Without proper synchronization, race conditions can occur and lead to incorrect results. I also understood how locks provide mutual exclusion, ensuring that only one thread can access a critical section at a time. Additionally, I learned how semaphores can control the number of threads accessing a resource, which is useful for managing limited resources like CPU.

One of the main challenges I faced was identifying critical sections in the code and deciding which synchronization mechanism to use. I also learned the importance of releasing locks properly using try-finally blocks to avoid deadlocks.. 

Overall, this assignment helped me understand how concurrency works in real-world applications and how synchronization ensures data consistency and program correctness.

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems, where multiple users may access and update the same account balance at the same time. Synchronization ensures that transactions are processed correctly without losing or duplicating money.



**Example 2**: 

Operating systems, where multiple processes compete for CPU resources. Synchronization ensures fair scheduling and prevents conflicts between processes.
---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]
Synchronization is like organizing access to a shared resource. Imagine multiple people trying to use the same printer at the same time. Without rules, their tasks may overlap and cause problems.

Using synchronization is like putting a queue in front of the printer, so only one person uses it at a time. Locks act like a key that only one person can hold, while semaphores act like allowing a limited number of people to use the resource at once.

This makes sure everything works correctly without conflicts or errors.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Reem-Mohammed-23/OS-Assignment3-Reem-Alshehri.git

**Number of commits**: 16

**Commit messages**: 
1. change my std id number
2. add ReentrantLock and semaphore for synchronization
3. add semaphore and import package for synchronaiztion
4. protect shared counter using reentrantlock
5.protect the shared varibal completedProcessCount to prevent deadlock
6.protect shared varibal totalWaitingTime using reentrantlock
7.protect executionLog
8.use semaphore to control cpu access in process execution and in final…
9.apply semaphore in runToCompletion method
10.answring part 1
11.answring part 2
12.answring part 3
13.answring part 4
15.answring part 5
16.answring part 6
---

## Summary

**Total time spent on assignment**: 2 day

**Key takeaways**: 
1. Synchronization is essential to prevent race conditions
2. Locks and semaphores serve different purposes in controlling threads
3. Proper testing is required to ensure program correctness

**Most challenging aspect**: 
Identifying critical sections and selecting the appropriate synchronization technique for each part of the program.

**What I'm most proud of**: 

Successfully implementing synchronization mechanisms and ensuring that the program produces consistent and correct results across multiple runs.
---

**End of Documentation**
