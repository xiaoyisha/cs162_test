## Task 2: Process Control Syscalls
### Data Structures and Functions
#### In process.h
``` c++
struct pro {
  pid_t pid;
  bool loaded;
  struct list_elem elem;
  struct semaphore sema;
  int exit_status;               /* default: -1*/
}
```
#### In thread.h
``` c++
struct thread {
  ...
  #ifdef USERPROG
  struct pro* pro;
  struct list children;
  #endif
  ...
}

```
### Algorithms
* halt  
We use `shutdown_power_off()`.
* exec  
We use `process_execute()` to run the executable whose name is given by `args[1]`, and return the new process’s program id (pid).  
When we create a new thread, we allocate a `pro` struct for it, and add it to the `children` list of the parent thread. And we initialize `pro->sema` to zero.  
When the thread finishes loading a program, we increment `pro->sema`.  
Before the `exec` system call returns, we call `sema_down()`.  
If the `pro->loaded` of the child thread is true, we will return the pid of the child; Otherwise, we reutn -1.
* wait  
First we look for a `pro` in the current thread's `children` list whose `pid` equals the given `pid`.  
If this `pro` doesn't exit, return -1; Otherwise, we call `sema_down()` on that `pro`'s semaphore.
Then, return its `exit_status`, remove its list_elem from `children`, and free the `pro`.
To futher expalin why in this way we can implement `wait`:  
A thread will call `sema_up()` at the end of `process_exit()`. So the parent thread will wait until the child thread ends.  
* practice  
Increment `args[1]` by 1 and store it in the `eax` register of the interrupt frame.
