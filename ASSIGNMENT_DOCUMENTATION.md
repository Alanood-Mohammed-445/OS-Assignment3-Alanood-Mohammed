# Assignment 3 - Complete Documentation

**Student Name**: [Alanood Mohammed Alqahtani]  
**Student ID**: [445052096]  
**Date Submitted**: [May 2]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1QEFk0gk_q84brsJOkfdqWbMU2gVDrIa8/view?usp=drivesdk
**Video filename**: 445052096_Assignment3_Synchronization.MOV

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [April 20, 10:30]
**What I implemented**: 
 Set up the GitHub repository and cloned the starter code. · Added ReentrantLock in SharedResources to protect contextSwitchCount,completedProcessCount, and totalWaitingTime. · Wrapped incrementContextSwitch(), incrementCompletedProcess(), and addWaitingTime() with lock/unlock in try-finally blocks
**Challenges encountered**: 
Forgot to put unlock() inside finally in the first attempt, which could cause deadlock.
**How I solved it**: 
 Reviewed the assignment instructions and corrected all locks to use try-finally
**Testing approach**: 
 Ran the simulation 5 times and observed stable results (no negative values or random jumps).
**Time spent**: 
one hour

---

### Entry 2 - [April 23, 7:30]
**What I implemented**: 
Added ReentrantLock to protect the ArrayList<String> executionLog inside logExecution(). ·
**Challenges encountered**: 
 I don't find the correct location .
**How I solved it**: 
 by search about it.
**Testing approach**: 
run the program
**Time spent**:
 one hour
 
---

### Entry 3 - [Date, Time]
**What I implemented**: 
· Added binary Semaphore (1 permit) to control concurrent CPU access as required in Task 3
**Challenges encountered**: 
find the corrict location
**How I solved it**: 
by search in the code.
**Testing approach**: 
run the code
**Time spent**: 
one hour

---

### Entry 4 - [Date, Time]
**What I implemented**: 
record the video and sure all code run 
**Challenges encountered**: 
record the video many times

**How I solved it**: 
===
**Testing approach**: 
sure the video qualty is perfect
**Time spent**: 
2 hour

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
One race condition occurs when modifying a shared variable like counter++ without synchronization. Multiple threads may read, increment, and write back simultaneously, causing lost updates because the operation is not atomic. The shared resource here is the counter, and concurrent access leads to inconsistent final values.
counter++

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

The main difference between ReentrantLock and Semaphore is that ReentrantLock provides exclusive access to a single resource, while a Semaphore allows a fixed number of threads to access a resource concurrently. ReentrantLock is reentrant, meaning the same thread can acquire the lock multiple times without deadlocking, making it ideal for protecting critical sections.
lock.lock();
try {
contextSwitchCounter++;
} finally {
    lock.unlock();
}

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
deadlock is a situation where a group of threads becomes permanently blocked because each thread is waiting for a resource held by another, causing the program to stop progressing. This often occurs when multiple locks are used without proper coordination.
in the code, I used mutex locks to protect shared resources, ensured a consistent lock acquisition order, and minimized the time each mutex was held to prevent deadlocks.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
The variables are:
contextSwitchCount, completedProcessCount, totalWaitingTim
**Why they need protection**: 
These are shared variables accessed and modified by multiple threads concurrently. Without protection, race conditions can occur, leading to incorrect results such as lost updates.
**Synchronization mechanism used**: 
A mutex using ReentrantLock is used to ensure mutual exclusion.
**Code snippet**:
public static void incrementContextSwitch() {
    lock.lock();
    try {
        contextSwitchCount++;
    } finally {
        lock.unlock();
    }
}

public static void incrementCompletedProcess() {
    lock.lock();
    try {
        completedProcessCount++;
    } finally {
        lock.unlock();
    }
}

public static void addWaitingTime(long time) {
    lock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        lock.unlock();
    }
}
**Justification**: 
Using ReentrantLock ensures that only one thread can modify these variables at a time, preventing race conditions and maintaining data consistency. The try-finally block guarantees that the lock is always released, avoiding issues like deadlock.

---

### Critical Section #2: Execution Log

**What resource**: 
The resource is the shared executionLog list (an ArrayList).
**Why it needs protection**: 
ArrayList is not thread-safe, so concurrent modifications by multiple threads can lead to race conditions, data corruption, or runtime errors.
**Synchronization mechanism used**: 
A mutex using ReentrantLock is used to ensure only one thread accesses the list at a time.
**Code snippet**:
public static void logExecution(String message) {
    lock.lock();
    try {
        executionLog.add(message);
    } finally {
        lock.unlock();
    }
}

**Justification**: 
Using ReentrantLock ensures mutual exclusion when accessing the shared list, preventing race conditions and maintaining data consistency. The try-finally block guarantees the lock is always released, avoiding potential deadlocks.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
The Semaphore is used to control how many threads can access the CPU (shared execution resource) at the same time.
**Number of permits and why**: 
It is set to 1 permit (new Semaphore(1)), meaning only one thread can execute at a time. This simulates a single CPU core execution model and prevents overlapping execution of processes.
**Where implemented**: 
It is used inside the run() method of the Process class before execution starts, and released in the finally block after execution finishes.
**Code snippet**:
try {
    SharedResources.cpuSemaphore.acquire();

    // execution section
    SharedResources.incrementContextSwitch();


} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: 
It ensures that only one process uses the CPU at a time, preventing concurrent execution conflicts and making the scheduling simulation more realistic. It also helps reduce race conditions during execution.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results
consistency of results after adding synchronization

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```
java SchedularSimulationSync


**Results**: 
(Show that running multiple times produces consistent, correct results)
context switches :42
completed processes :5 
total wating time :1240ms
*
**Why synchronization is necessary**: 
without synchronization counter would loss updates.

**Conclusion**:
synchronization succeeded results are now stable error-free.

---

### Test 2: Exception Testing
**What I tested**: 
Checking for ConcurrentModificationException
**Testing procedure**: 
run the program 5+ times with diffrent process count.
**Results**: 
no exception occurred
**What this proves**: 
the lock on executinglog prevent concurrent modification.

---

### Test 3: Correctness Verification
**What I tested**:
Verifying correct final values (total burst time, context switches, etc.)
**Expected values**: 
**Actual values**: 
completed processes :5 
total wating time :1240ms
**Analysis**: 
all values are logical and match expectations,confirming correct synchronization

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
incresased number of process from 3 to 10 and decreased time quantem from 100ms to 50ms
**Purpose**: 
to verify synchronization holds under different loads and scheduling parametrs.
**Results**: 
no race condition, no exceptions .
**What I learned**: 
proper the synchronization makes the simulation scalable and reliable.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is essential in multithreaded systems to prevent race conditions.
ReentrantLock helps protect critical sections by allowing only one thread to access shared resources at a time.
Semaphore is useful for limiting the number of threads accessing a resource.
I understood how race conditions can lead to incorrect results in shared counters and logs.
I also learned the importance of using try-finally to avoid deadlocks.
This assignment helped me understand how operating systems manage process scheduling safely.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**:Banking systems (ATM transactions).

**Example 2**: Database connection pools / web servers

---

### How I would explain synchronization to others:

Synchronization is like controlling access to a shared bathroom.
Only one person (thread) can use it at a time (lock), or a limited number (semaphore).
Without control, people will interfere with each other and cause wrong results.

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 
   13
**Commit messages**: 
1. Set My Student id:445052096
2. Task 1
3. Task2
4. Task3

---

## Summary

**Total time spent on assignment**: 
10 hour

**Key takeaways**: 
1. race conditions happen in shared resources
2.mutex (ReentrantLock) protects critical sections
3.semaphore limits concurrent access
4.synchronization ensures correctness 

**Most challenging aspect**: 
understanding when to use lock vs semaphore
**What I'm most proud of**: 
successfully preventing race conditions and making program thread-safe

---

**End of Documentation**
