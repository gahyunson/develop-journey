Does the programmer need to write a single program that performs many independent activites?   
Does every program have to be altered for every piece of hardware?   
Does a faulty program crash everything?   
Does every program have access to all hardware?   
: Hopefully, no!

Operating Systems:
- Provide convenient abstractions to handle diverse hardware
(Convenience, protection, reliablity obtanied in creating the illusion)
- Coordinate resources and protect users from each other
(Using a few critical hardware mechanisms)
- Simplify application development by providing standard services
- Provide fault containment, fault tolerance, and fault recovery


|  |
---
|Application|
|Operating System|
|Hardware|


Processor - Thread
Memory - Address Space 
Disks, SSDs, ... - Files
Networks - Sockets
Machines - Processes

OS as an illusionist

Thread: Execution Context
- Fully describes program state
- Program Counter, Registers, Execution Flags, Stack
Address space 
Process: an instance of a running program
Dual mode operation / Protection

OS Bottome Line: Run Programs
- Write them and compile them
- Load instruction and data segments of executable file into memory
- Create stack and heap
- "Transfor control to program" 
- Provide services to program
- While protecting OS and program

Recall: Instucction Fetch/Decode/Execute
The instructin cycle
- program counter would point into the memory & allow the processor to fetch the next instruction
- pull the instruction from the memory, would decode it 
- would feed it to the execution pipeline 


First OS Concept: Thread of Control

Thread: Single unique execution context
- Program Counter, Registers, Execution Flags, Stack, Memory State
A theread is executing on a processor when it is resident in the processor registers
Resident means: Registers hold the root state (context) of the thread:
    - Including program counter register & currently executing instruction
        PC points at next instruction in memory
        Instructions stored in memory
    - Including intermediate values for ongoing computations
        Can include actual values or pointers to values in memory
    - Stack pointer holds the address of the top of stack (which is in memory)
    - The rest is "in memory"

A thread is suspended (not executing) when its state is not loaded (resident) into the processor
    - Processor state pointing at some other thread
    - Program counter register is not pointing at next instruction from this thread
    - Often: a copy of the last value for each register stored in memory

What happens during program execution?
Execution sequence
- Fetch instruction at PC
- Decode
- Execute (possibly using registers)
- Write results to registers/mean
- PC = Next Instruction(PC)
- Repeat