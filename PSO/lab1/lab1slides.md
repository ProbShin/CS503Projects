# Lab1 PSO materials
Index
1. handout go through
1. process context switch
1. readylist and queue
1. timer interrupt
1. float-point operations, XINU and fixed-point library

</br>
</br>

------------------------------------------
</br>

### 1. Handout go through
1. Aging Scheduler [20pt]
1. Proportional Share Scheduler [50pt]
1. Multilevel Feedback Queueu Scheduler [70pt]
1. Process owernership [20pt]

[lab1 handout](https://www.cs.purdue.edu/homes/pfonseca/teaching/cs503/21spring/labs/lab1.html)


reminder: 
1. **Required**: Do **NOT** *define/initialize* new function or variables in `main.c`. Instead, you could *define/initialize* in the beginning of *initialized.c*, and *declare* them in *prototype.h*.
1. **Recommended**: you can use `XDEBUG_KPRINTF` function for your own debug kprint purpose, use 'XTEST_KPRINTF' function for the handout specified kprint purpose. The definitions of the above functions are under *./include/process.h*

</br>
</br>

-----------------------------------------

</br>

### 2. Context-Switch and `rescued()`


* `clkhander.c`
* `resched.c`
* `ctxsw.S`


```text
process P1(){         |        process P2(){
  int i=0;            |          int x=0;
  while(1) {          |          while(1) {
     i++;             |            x++;
  }                   |          }
  return OK;          |          return OK;
}                     |        }
```

![processes ctxsw example](https://raw.githubusercontent.com/ProbShin/myCS503ProjectsRepo/main/PSO/lab1/img1.png)

```c
void clkhandler(..) {
    if((++count1000) >= 1000) { // update time
        clktime++;
        count1000 = 0;
    }

    if((--preempt) <= 0) { // trigger context-switch
        preempt = QUANTUM;
        resched();
    }
}
```

```c
void resched(void) {
    ptold = &proctab[currpid];
    if (ptold->prstate == PR_CURR) {  /* put current process back to readylist */
        ptold->prstate = PR_READY;
        insert(currpid, readylist, ptold->prprio);
    }

    /* pick up a process from readylist */
    currpid = dequeue(readylist);  // which policy is this
    ptnew = &proctab[currpid];
    ptnew->prstate = PR_CURR;
    preempt = QUANTUM;		/* Reset time slice for process	*/
    ctxsw(&ptold->prstkptr, &ptnew->prstkptr);

    /* Old process returns here when resumed */
    return;
}
```

```asm
ctxsw:
    /* lots of push */
    push  xxx
    push  xxx
    
    /* Move to new process stack */
    movl	(%eax),%esp

    /* lots of pop */
    pop
    pop

    ret
```


</br>
</br>

-----------------------------------------

</br>

### 3. Readylist

XINU store all the `PR_READY` processes in a priority-queue data structure called **readylist**. It utilizes XINU's queue system. 
You need to implement your own operation on this data structure. i.e. `traverse`, `insert`, `remove`.

```
# An example of double linked list, (Note, this is not XINU used)
struct node {
  int    val;
  struct node*  prev;
  struct node*  next
}
```

```
# priority-queue operation demo
Head <-> 30 <-> 20 <-> 20 <-> 10 <-> Tail 
         |      |       |      |
         p1     p2      p3     p4
        

Demo: insert 40 (p5); insert 5 (p6); insert 20 (p7);

Head <-> 40 <-> 30 <-> 20 <-> 20 <-> 20 <-> 10 <-> 5 <-> Tail 
          |     |     |      |      |     |     |
         p5    p1     p2     p3     p7    p4    p6
```


The XINU's readylist related implementations.
* `queue.h` for data structure and marco functions.
* `insert.c` for `insert()` to insert a node to a **priority-queue**.
* `queue.c` for `dequeue()` to dequeue the first node from a **priority-queue**.


</br>
</br>

-----------------------------------------

</br>

### 4. Queue

```text
// XINU Node                        // Common Node                 
struct node {                       struct node {
  short val;  //16bit                   int val;
  char qnext; // 8bit       <=          struct node* next;
  char qprev; // 8bit                   struct node* prev;
}                                   }
```

XINU's queue system. Pre-assigned space for a **Head** and a **Tail** for per queue using table to implement double-linked-list.
* `queue.c` for `dequeue()`, `enqueue()` function to pop/push a node to a queue.
* `queue.h` for queue related data structure and marco functions.
* `initialize.c` for inilization of the queue table.


```
# each row of the table served as an node.
# each process assigned a node.
# each queue assigned two nodes (Head, tail)

      |<--16bit-->|<8bit|8bit>|
rowNo.|    Key    | nxt | pre |
      |-----------------------|
              ...
      |   ...     |     |     |
      |-----------------------|
  4   |   10      | nxt | pre |
      |-----------------------|
  5   |   20      | nxt | pre |
      |-----------------------|
      |           |     |     |
      |-----------------------|
      |           |     |     |
      |-----------------------|
  q   |    Head   | nxt | pre |    \
      |-----------------------|     |-> readylist
 q+1  |    Tail   | nxt | pre |    /
      |-----------------------|
      |           |     |     |
       ...
```

This example shows that there are only **two process P4 and P5 which have priority 10 and 20 in the 'readylist'**.

`Head <=> P4 <=> P5 <=> TAIL`


```
      |-----------------------|
  4   |   10      |  q  |  5  |
      |-----------------------|
  5   |   20      | q+1 |  4  |
      |-----------------------|
      |           |     |     |
      |-----------------------|
      |           |     |     |
      |-----------------------|
  q   |    Head   |  4  | pre |    \
      |-----------------------|     |-> readylist
 q+1  |    Tail   | nxt |  5  |    /
      |-----------------------|
```


</br>
</br>

**Traverse the queue**

```
# pseudo code, not the real code.
for i = Head.qnext; i != Tail; i = queuetab[ i ].qnext :
    print "p%d has priority %d\n"%(proctab[i].priority, i) # process node here
```

```
p4 has priority 10
p5 has priority 20
```

</br>
For lab1, implement your own 'traverse', 'pop', 'push' functions to readylist.
</br>
</br>


<!--
------------------------------------------------

</br>

### 4.1 Double-linked-list
basic operations of double-linked-list.
* `traverse`
* `insert`
* `remove`

</br>
</br>

-->
-----------------------------------------

</br>

### 5. Timer Interrupt 
* clkhandler.c
* clkinit.c
* clkdisp.S

`clkhandler()` by default would be triggered per 1ms. It would call `resched()` to "context-switch" processes depend on the `QUANTUM`.

</br>
</br>

-----------------------------------------

</br>

### 6. Float-point operations, XINU and Fixed-point library 

* Float-point operations are not unit operations which means they need special treatment. 
* Fixed-point library uses integer operations instead.
  - demo how to download, read and execute.
  - under XINU
      read `system/main.c` and `system/fix16test.c`
  i.e. to do 
  
  `load_avg := (59/60) * load_avg + (1/60) * n_ready_processes` 

```
    int n_ready_processes = 3;
    fix16_t load_avg = fix16_from_int(0);
    
    load_avg = fix16_div( 
                    fix16_add( 
                          fix16_mul(fix16_from_int(59), load_avg)
                          , fix16_from_int(n_ready_processes) )
                     , fix16_from_int(60)
                );
    
    fix16_to_str(load_avg,output,6);
    char output[128];
    kprintf("load_avg = %s\n", output);
```
  
------------------------------------

Materials 
* fix16.tgz source code
* xinu *system/main.c* and *system/fix16test.c*
</br>
</br>


