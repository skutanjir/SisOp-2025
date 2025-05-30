Nama Kelas : Sulistyo fajar pratama
KELAS : D3 IT B
NRP : 3124500037      
**Dosen Pengajar:** Dr Ferry Astika Saputra ST, M.Sc
---

## 1.1  
**Question:**  
*What are the three main purposes of an operating system?*

**Answer:**  
1. **To run programs and provide a convenient environment:**  
   The OS executes programs and offers services and interfaces that make the system easier to use.  
2. **To manage and allocate resources:**  
   The OS handles CPU, memory, and I/O devices to ensure efficient utilization.  
3. **To ensure protection and security:**  
   The OS enforces mechanisms so that programs cannot interfere with one another and protects the system and data from unauthorized access.

---

## 1.2  
**Question:**  
*We have stressed the need for an operating system to make efficient use of the computing hardware. When is it appropriate for the operating system to forsake this principle and to “waste” resources? Why is such a system not really wasteful?*

**Answer:**  
- Sometimes, **sacrificing efficiency** is acceptable to provide **user convenience** or **faster development**. For example, offering a rich graphical interface or large libraries.  
- This is **not truly wasteful** because the additional resource usage brings greater benefits in usability, compatibility, and productivity.  
- Modern hardware is powerful enough that a slight “waste” for improved user experience is often justified.

---

## 1.3  
**Question:**  
*What is the main difficulty that a programmer must overcome in writing an operating system for a real-time environment?*

**Answer:**  
- Ensuring that **every task completes before its deadline** in a predictable manner.  
- Dealing with **precise scheduling** and **minimal latency** so the system’s behavior remains deterministic.  
- Managing interrupts and synchronization carefully to avoid delays that could lead to missed deadlines.

---

## 1.4  
**Question:**  
*Keeping in mind the various definitions of operating system, consider whether the operating system should include applications such as web browsers and mail programs. Argue both that it should and that it should not, and support your answers.*

**Answer:**  
- **Argument “Should”:**  
  - Provides a **complete, ready-to-use environment** for end-users.  
  - Tight integration can enhance **security** and **updates**.  
- **Argument “Should Not”:**  
  - Increases the **complexity** and size of the OS, potentially complicating maintenance.  
  - The OS should focus on **core functionalities** (resource management, security, etc.), leaving user applications separate.

---

## 1.5  
**Question:**  
*How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?*

**Answer:**  
- In **user mode**, applications are restricted from accessing certain instructions and memory areas.  
- In **kernel mode**, the OS has full access to all resources.  
- This separation prevents user programs from performing critical or harmful operations that could compromise the entire system.

---

## 1.6  
**Question:**  
*Which of the following instructions should be privileged?*  
a. Set value of timer  
b. Read the clock  
c. Clear memory  
d. Issue a trap instruction  
e. Turn off interrupts  
f. Modify entries in device-status table  
g. Switch from user to kernel mode  
h. Access I/O device

**Answer:**  
- **Privileged instructions** typically include:  
  - Set value of timer (a)  
  - Clear memory (c)  
  - Issue a trap instruction (d)  
  - Turn off interrupts (e)  
  - Modify entries in device-status table (f)  
  - Switch from user to kernel mode (g)  
  - Access I/O device (h) (usually requires special rights)  

- **Non-privileged:**  
  - Read the clock (b), because it generally does not affect the system state in a critical way.

(Note: Some systems may permit certain I/O instructions in user mode, but commonly it is considered privileged.)

---

## 1.7  
**Question:**  
*Some early computers protected the operating system by placing it in a memory partition that could not be modified by either the user job or the operating system itself. Describe two difficulties that you think could arise with such a scheme.*

**Answer:**  
1. **Inflexibility in updating the OS:**  
   - If the OS code is in a protected partition that it cannot modify, applying patches or bug fixes is difficult.  
2. **Determining the correct partition size:**  
   - If the partition is too small, the OS may run out of space. If it is too large, memory is wasted.

---

## 1.8  
**Question:**  
*Some CPUs provide for more than two modes of operation. What are two possible uses of these multiple modes?*

**Answer:**  
1. **Hypervisor (Virtual Machine Monitor) mode:**  
   - A separate privilege level for running and managing virtual machines.  
2. **Special driver or process mode:**  
   - Allows certain drivers or processes to run in an intermediate privilege level, reducing the risk of system-wide crashes if a driver fails.

---

## 1.9  
**Question:**  
*Timers could be used to compute the current time. Provide a short description of how this could be accomplished.*

**Answer:**  
- The OS **initializes** the timer with a specific count-down value.  
- When the timer reaches zero, an **interrupt** occurs.  
- On each interrupt, the OS **increments** a variable that keeps track of the current time (for instance, in “ticks” or seconds).  
- By continually updating this variable at each timer interrupt, the OS can maintain an accurate clock.

---

## 1.10  
**Question:**  
*Give two reasons why caches are useful. What problems do they solve? What problems do they cause? If a cache can be made as large as the device for which it is caching, why not make it that large and eliminate the device?*

**Answer:**  
- **Reasons caches are useful:**  
  - They store frequently accessed data closer to the CPU or faster memory, reducing access time.  
- **Problems solved:**  
  - They mitigate the speed gap between fast processors and slower storage devices.  
- **Problems caused:**  
  - Data **coherency** issues (keeping cached data consistent with main memory or disk).  
  - **Complexity** in managing cache (replacement policies, mapping strategies).  
- **Why not make the cache as large as the device?**  
  - **Cost:** Fast cache memory (like SRAM) is significantly more expensive than bulk storage.  
  - **Latency:** A very large cache can slow down access times due to search overhead.  
  - We still need large, cheaper storage (e.g., disks) for capacity.

---
