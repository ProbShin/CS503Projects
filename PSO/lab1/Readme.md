# Lab1 PSO materials

</br>

------------------------------------------
</br>

### 1. Handout go through
1. Aging Scheduler
1. Proportional Share Scheduler
1. Multilevel Feedback Queueu Scheduler
1. Process owernership

[lab1 handout](https://www.cs.purdue.edu/homes/pfonseca/teaching/cs503/21spring/labs/lab1.html)

</br>
</br>

-----------------------------------------

</br>

### 2. Context-Switch and `resched()`


![processes ctxsw example](https://raw.githubusercontent.com/ProbShin/myCS503ProjectsRepo/main/PSO/lab1/img1.png)

</br>
</br>

-----------------------------------------

</br>

### 3. Readylist

XINU store all the `PR_READY` processes in a prority-queue data structure called **readylist**. It utlizes XINU's queue system. 
You need to implement your own operation on this data structure. i.e. `traverse`, `insert`, `remove`.

The XINU's readylist related implementations.
* `insert` in *insert.c*
* `dequeue` in *queue.c*
* data structure and marco functions in *queue.h*

</br>
</br>

-----------------------------------------

</br>

### 4. Queue
XINU's queue system. Preassigned space for a **Head** and a **Tail** for per queue using table to implement double-linked-list.
* `dequeue`, `enqueue` in *queue.c*
* data structure and marco functions in *queue.h*
* assigned space in *initialize.c*

</br>
</br>


<!--
------------------------------------------------

</br>

### 4.1 Double-linked-list
basic operations of double-linked-list.
* `travers`
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

`clkhandler()` by default would be triggered per 1ms. It would call `resched()` to "context-switch" procesess depend on the `QUTANTUM`.

</br>
</br>



