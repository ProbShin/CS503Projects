# Lab3 PSO materials

</br>
</br>

Index

1. handout go through
1. in memory file system
1. interrupt
1. asm function, inline asm and function arguments
1. caller callee convention
1. return values
1. elf system



</br>
</br>
</br>
</br>

------------------------------------------
</br>

</br>
</br>

### 0. Project setup
1. Accept the project [CLICK Github Classroom Lab3 Link]();
2. [Read the Lab3 handout](https://www.cs.purdue.edu/homes/pfonseca/teaching/cs503/21spring/labs/lab3.html) multiple times before start.


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

Basic in-memory file system (30pt)

</br>

Implement a system call interface based on software interrupts (30 pts)

</br>

Create a new system call that dynamically loads a program (60 pts)

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


### 2. In memory file system

allocate a chunk of memory to store the input arguments.

</br>
</br>
</br>

------------------------------------------
</br>
</br>
</br>

### 3. interrupt system and function argument

Some useful files.
* evec.c
* intra.S

</br>
</br>
</br>
</br>

Function call case:
```
int fun(int a1, int a2){
  int y=a1+a2;
  return y;
}

int main(){
   int x=0;
   x++;
   fun1(3,5);
   x=5;
   return;
}

```




</br>
</br>
</br>
</br>

Interrupt case:
```
int fun(int a1, int a2){
  int y=a1+a2;
  return y;
}

int main(){
   int x=0;
   x++;
   // an interrupt happens here. i.e. asm("int $0x80");
   x=5;
   return;
}

```






</br>
</br>
</br>



------------------------------------------
</br>
</br>
</br>


### 4. asm function, inline asm 

some examples.
ctxsw.S
syscall_interface.c

</br>
</br>
</br>




------------------------------------------
</br>
</br>
</br>

### 5. caller callee convention


</br>
</br>
</br>




------------------------------------------
</br>
</br>
</br>

### 6. return values


</br>
</br>
</br>




------------------------------------------
</br>
</br>
</br>

### 7. elf system


</br>
</br>
</br>




------------------------------------------
</br>
</br>
</br>



