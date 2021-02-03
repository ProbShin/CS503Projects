# Lab1 

----------------------------
Handout
1.1 Aging Scheduler
1.2 Proportional Share Scheduler
1.3 Multilevel Feedback Queueu Scheduler
1.4 Process owernership


-----------------------------------------

## process context switch and `resched()`
![https://github.com/probshin/myCS503Project/PSO/lab1/img1.png](https://github.com/probshin/myCS503Project/PSO/lab1/img1.png)


-----------------------------------------

## readylist

readylist is a prority queue, using XINU's queue system. It stores all the process that is `PR_READY` to be context-switched in. 
* insert
* dequeue


-----------------------------------------
# queue
XINU's queue system. Preassigned space for a **Head** and a **Tail** for per queue using table to implement link list.

-----------------------------------------
# XINU time interrupt related
* clkhandler.c
* clkinit.c
* clkdisp.S

`clkhandler()` by default would be triggered per 1ms. It would call `resched()` to "context-switch" procesess depend on the `QUTANTUM`.




