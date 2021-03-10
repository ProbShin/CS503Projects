# Lab2 PSO materials

</br>
</br>

Index

1. handout go through
1. processes and semaphore
1. semaphore system
1. queue and semaphore queue
1. lock and unlock
1. variable number of arguments
1. link-list v.s. hash-map


</br>
</br>
</br>
</br>

------------------------------------------
</br>

</br>
</br>

### 0. Project setup
1. Accept the project [CLICK Github Classroom Lab2 Link]();
2. [Read the Lab2 handout](https://www.cs.purdue.edu/homes/pfonseca/teaching/cs503/21spring/labs/lab2.html) multiple times before start.


</br>
</br>
</br>
</br>
</br>

</br>
</br>

------------------------------------------
</br>



### 1. Handout

</br>

Basic lock system (100pts)
* linit, lcreate,  (add linit to initialize.c)
* lock
* releaseall
* ldelete

</br>

Priority Inheritance (80pts)
* change process table
* change lock table
* modify linit, lcreate
* modify lock
* modify releaseall
* modify ldelete


</br>
</br>
</br>
</br>

</br>
</br>

------------------------------------------
</br>
</br>
</br>


### 2. Process and the Global Shared Resources
```c
volatile int x=0;

process p(){
    for(int i=0; i<1 000 000; i++)
        x++;
    return OK;
}

process main(){
    resched_cntl(DEFER_START);
    resume(create (p, 1024, 100, "p1", 0));  //process p1
    resume(create (p, 1024, 100, "p2", 0));  //process p2
    resched_cntl(DEFER_STOP);
    
    kprintf("x=%d expect %d",x, 2 000 000);
    return OK;
}

```

</br>
</br>
</br>
</br>

![processes ctxsw example](https://raw.githubusercontent.com/ProbShin/myCS503ProjectsRepo/main/pso-materials/lab2/lab2ex1.png)

</br>
</br>
</br>
</br>
</br>

</br>
</br>
reasons:

1. multiple process do operation on a shared resources.
1. the operation is not unit operation.

details:
```
P1: read x=100 from memory into register.
interrupt raise. P1 context switch to P2
P2: read x=100 from memory into register=100.
P2: register value increase by 1
P2: write the register value=101 into memory.
...
interrupt raise. P2 context switch to P1. 
P1: (continue previous jobs). **Assume** register contains the value of x=100. 
P1: register value increase by 1
P1: write the register value=101 into memory. **which overwrite the x value in memory**
```

</br>
</br>
</br>

</br> 
How to solve it?

 Since the issue is caused by "resource-share". Shall we using a "lock" or "flag" to indicate or control the **shareness**?


i.e. `test_and_set`
```c
int lock=0    // 0: unlocked.  1: locked

int test_and_set(lock):
    if ( lock == 0 ) return lock++;      // <===
    return lock;

int x=0
int some_job_function():
    while ( test_and_set( lock ) == 1 )
        ;
    x++;        // job on the global resources 
    lock = 0 ;  // unset the lock
```

</br>
</br>
</br>
</br>
</br>
</br>
</br>
</br>

Issues?  
1. "busy check" waste cpu.
2. lock is another global shared variable.

</br>
</br>
</br>
</br>
</br>
</br>

------------------------------------------
</br>
</br>
</br>
</br>


### 3. xinu semaphore system

</br>

</br>


*The same example with xinu semaphore*
```c
volatile int x=0;
process p(sid32 s){
    for(int i=0; i<1 000 000; i++) {
        wait(s);
        x++;
        signal(s);
    }
    return OK;
}

process main(){
    sid32 s = semcreate(1);

    resched_cntl(DEFER_START);
    resume(create (p, 1024, 100, "p1", 1, s));  //process p1
    resume(create (p, 1024, 100, "p2", 1, s));  //process p2
    resched_cntl(DEFER_STOP);
    
    kprintf("x=%d",x);
    return OK;
}
```


</br>
</br>
</br>
</br>
</br>


* Files

    * `semaphore.h`
    * `semacreate.c`
    * `wait.c`
    * `signal.c`
    * `semadelete.c`

</br>
</br>
</br>

`semaphore.h`
```c
struct	sentry	{
	byte	sstate;		/* Whether entry is S_FREE or S_USED	*/
	int32	scount;		/* Count for the semaphore		*/
	qid16	squeue;		/* Queue of processes that are waiting	*/
				/*     on the semaphore			*/
};
```
</br>
</br>

`semacreate.c`
```c
sid32	semcreate( int32 count ) {
    ...
    sem=newsem()
    ...
    semtab[sem].scount = count;	/* Initialize table entry	*/
    ...
	return sem;
}
```
</br>
</br>

`wait.c`
```c
syscall	wait( sid32 sem ) {
    ...
    semptr = &semtab[sem];
	...
    if (--(semptr->scount) < 0) {		/* If caller must block	*/
		prptr = &proctab[currpid];
		prptr->prstate = PR_WAIT;	/* Set state to waiting	*/
		prptr->prsem = sem;		/* Record semaphore ID	*/
		enqueue(currpid,semptr->squeue);/* Enqueue on semaphore	*/
		resched();			/*   and reschedule	*/
	}
    ...
}
```
</br>
</br>

`signal.c`
```c
syscall	signal( sid32 sem ) {
    ...
	semptr= &semtab[sem];
	...
	if ((semptr->scount++) < 0) {	/* Release a waiting process */
		ready(dequeue(semptr->squeue));
	}
    ...
}

```

</br>
</br>
</br>

`semadelete`
```c
syscall	semdelete( sid32 sem ) {
    ...
	semptr = &semtab[sem];
	...
    semptr->sstate = S_FREE;

	resched_cntl(DEFER_START);  // <==
	while (semptr->scount++ < 0) {	/* Free all waiting processes	*/
		ready(getfirst(semptr->squeue));
	}
	resched_cntl(DEFER_STOP);  // <==
    ...
}
```
</br>
</br>
</br>
</br>

</br>
</br>

-----------------------

</br>
</br>
</br>

### 4. queue and semaphore queue
</br>

</br>

For xinu semaphore, the queue operations used are "insert, pop, peek".
They are `enqueue()`, `dequeue()`, `getfirst()`


```
      |<--32bit-->|<8bit|8bit>|
rowIdx|    Key    | nxt | pre |
      |-----------------------|
0     |           | nxt | pre |   -> 1st process
      |-----------------------|
1     |           | nxt | pre |   -> 2nd process
              ...
      |           |     |     |
      |-----------------------|
      |   10      | nxt | pre |
      |-----------------------|
      |   20      | nxt | pre |
      |-----------------------|
      |           |     |     |
      |-----------------------|
      |           |     |     |    -> NPROC process
      |-----------------------|
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> readylist
      |    Tail   | nxt | pre |    /
      |-----------------------|
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> sleeplist
      |    Tail   | nxt | pre |    /
      |-----------------------|
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> 1st semaphore queue
      |    Tail   | nxt | pre |    /
      |-----------------------|
             ... 
      |-----------------------|     
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> NSEM semaphore queue
      |    Tail   | nxt | pre |    /
      |-----------------------|
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> 1st lock queue
      |    Tail   | nxt | pre |    /
      |-----------------------|
             ... 
      |-----------------------|     
      |    Head   | nxt | pre |    \
      |-----------------------|     |-> NLOCKs lock queue
      |    Tail   | nxt | pre |    /
      |-----------------------|

```
</br>
</br>

</br>
</br>

----------------------

</br>
</br>
</br>



### 5. lock and unlock.
</br>
</br>
</br>

The lab2 asks you to create a ***new*** *semaphore system* from scratch called *lock system*. 


Advantages:
* read lock, write lock.
* **priority heriatge method** to prevent **deadlock**.

</br>
</br>
</br>

p.s. Implement your own way, and do **NOT** use exist semaphore.


</br>
</br>
</br>

</br>
</br>

----------------------------------------
</br>
</br>
</br>
</br>

### 6. variable number of arguments `fun(int nargs, ...);`

</br>
</br>

For function with various number of arguments, by convention, the last explicity specified argument is `nargs` represents number of following arguments. we can use `va_arg` to get the following argument. 

</br>

i.e. 
```c
void fun(int nargs, ...){
    va_list ap;
    va_start(ap, nargs);
    for(int i=0; i<nargs; i++){
        int a = va_arg(ap, int);
        kprintf("arg%d is '%d'\n", i, a);
    }
    va_end(ap);
}
```

</br>
</br>
</br>
</br>
</br>
</br>

-----------------------------------

</br>

### 7. hash-map v.s. link-list

For *basic lock* part, you would need a queue (per lock) to hold the *blocked* proceses (on this lock). This queue could be just the *xinu queue* (modify if needed) for simplicity.

For *priority heritage* part:
1. a process needs to do operations on "all locks that this process possessed".
1. a lock needs to do operations on "all processes that possessed this lock"


`process.h`
```c
struct procent{              |  struct procent {
    ...                      |      ...
    struct youNode* Head;    |      char L[NLOCK]; 
    ...                      |      ...
}                            |  }
```

`lock.h`
```c
struct locent{               |  struct locent {
    ...                      |      ...
    struct youNode* Head;       |      char P[NPROC];
    ...                      |      ...
}                            |  }
```


</br>








