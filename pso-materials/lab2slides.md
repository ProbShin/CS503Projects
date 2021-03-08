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
* modify resched?
* modify linit, lcreate
* modify lock
* modify releaseall
* modify ldelete



</br>
</br>

------------------------------------------
</br>


### 2. Process and Semaphore
```
int x=0;

process p(){
    for(int i=0; i<10000000; i++)
        x+=1;
    return OK;
}

process main(){
    resume(create (p, 1024, 15, "p1", 0));
    resume(create (p, 1024, 15, "p2", 0));
    sleep(3);
    kprintf("x=%d",x);
    return OK;
}

```


</br>
</br>

------------------------------------------
</br>

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








