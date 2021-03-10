# Lab2 PSO materials

Index

1. handout go through
1. processes and semaphore
1. semaphore system
1. queue and semaphore queue
1. lock and unlock
1. variable number of arguments


</br>
</br>

------------------------------------------
</br>

### 0. Project setup
1. Accept the project [CLICK Github Classroom Lab2 Link]();
2. [Read the Lab2 handout](https://www.cs.purdue.edu/homes/pfonseca/teaching/cs503/21spring/labs/lab2.html)



</br>
</br>

------------------------------------------
</br>



### 1. Handout

Basic lock system
* linit, lcreate,  (add linit to initialize.c)
* lock
* releaseall
* ldelete

Priority Inheritance
* change process table
* change lock table
* modify linit, lcreate
* modify lock
* modify releaseall
* modify ldelete



</br>
</br>

------------------------------------------
</br>


### 2. Process and Semaphore
```c
int x=0;

process p(){
    for(int i=0; i<1000,000,000; i++)
        x+=1;
    return OK;
}

process main(){
    resched_cntl(DEFER_START);
    resume(create (p, 1024, 100, "p1", 0));  //process p1
    resume(create (p, 1024, 100, "p2", 0));  //process p2
    resched_cntl(DEFER_STOP);
    
    kprintf("x=%d",x);
    return OK;
}

```

</br>
</br>
reasons:
1. multiple process do operation on a shared resources.
1. the operation is not unit operation.

steps:
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
How to solve it? Since the issue is caused by "resource-share". Shall we using a "lock" or "flag" show the **shareness**?



</br>
</br>

------------------------------------------
</br>

### xinu semaphore system

* `semaphore.h`
* `semacreate.c`
* `wait.c`
* `signal.c`
* `semadelete.c`

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

`signal.c`
```c

```



1. queue and semaphore queue
1. lock and unlock
1. variable number of arguments

###3. fun(int nargs, ...);

for various number of arguments, "nargs" represents number of arguments. we can use `va_arg` to the next argument.

i.e. 
```
void fun(int nargs, ...){
    va_list ap;
    va_start(ap, nargs);
    for(int i=0; i<nargs; i++){
        int a = va_arg(ap, int);
        kprintf("arg%d '%d'\n", i, a);
    }
    va_end(ap);
}
```


### 4. array








