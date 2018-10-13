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
#### In userprog/syscall.c
``` c++
static void
syscall_handler(struct intr_frame *f UNUSED)
{
  ...
  switch(args[0]){
    ...
    case SYS_HALT:
    case SYS_EXEC;
    case SYS_WAIT;
    case SYS_PRACTICE;
      check_ptr(&args[1], sizeof(uint32_t));
    ...
  }
  switch(args[0]){
    ...
    case SYS_EXEC:
      check_string((char*)args[1]);     /* check whether the pointer of the string is legal to access */
      f->eax = process_execute((char*)args[1]);
    case SYS_PRACTICE:
      f->eax = args[1]+1;
    case SYS_HALT:
      shutdown_power_off();
    case SYS_WAIT:
      f->eax = process_wait(args[1]);
    ...
  }
  ...
  bool isValid(void *uaddr){
    /* Need to be implemented to check whether the `uadder` is a valid address that we can access. */
  }
  void check_ptr(void *ptr, size_t size){
    /* Need to be implemented to check whether both `ptr` and `ptr + size` are valid address we can access. */
  }
  void check_string(char* ustr){
    /* Need to be implemented to check whether both `ustr` and `ustr + strlen(ustr)` are valid address we can access. */
  }
}
```

### Algorithms
* halt  
We use `shutdown_power_off()`.
* exec  
We use `process_execute()` to run the executable whose name is given by `args[1]`, and return the new process’s program id (pid).  
In `thread_create()`, we allocate a `pro` struct for the new thread, and add it to the `children` list of the parent thread. And we initialize `pro->sema` to zero.  
In `start_process()`, when the thread finishes loading a program, we call `sema_up()` and assign `true` to `pro->load`.  
Before the `exec` system call returns, we call `sema_down()`.  
If the `pro->loaded` of the child thread is `true`, we will return the pid of the child; Otherwise, we reutn -1.
* wait  
First, in `process_wait()`, we look for a `pro` in the current thread's `children` list whose `pid` equals the given `pid`.  
If this `pro` doesn't exit, return -1; Otherwise, we call `sema_down()` on that `pro`'s semaphore.
Then, return its `exit_status`, remove its list_elem from `children`, and free the `pro`.
To futher expalin why in this way we can implement `wait`:  
A thread will call `sema_up()` at the end of `process_exit()`. So the parent thread will wait until the child thread ends.  
* practice  
Increment `args[1]` by 1 and store it in the `eax` register of the interrupt frame.
