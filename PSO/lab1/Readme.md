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

### 2. Context-Switch and `resched()`
</br>

![processes ctxsw example](https://raw.githubusercontent.com/ProbShin/myCS503ProjectsRepo/main/PSO/lab1/img1.png)

</br>
</br>
-----------------------------------------

### 3. Readylist
</br>

readylist is a prority queue, using XINU's queue system. It stores all the process that is `PR_READY` to be context-switched in. 
* insert
* dequeue

</br>
</br>

-----------------------------------------
</br>

## 4. Queue
XINU's queue system. Preassigned space for a **Head** and a **Tail** for per queue using table to implement link list.
* dequeue.c
* enqueue.c
* queue.h

</br>
</br>

-----------------------------------------
</br>

# Timer Interrupt 
* clkhandler.c
* clkinit.c
* clkdisp.S

`clkhandler()` by default would be triggered per 1ms. It would call `resched()` to "context-switch" procesess depend on the `QUTANTUM`.

</br>
</br>



