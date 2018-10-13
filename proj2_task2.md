## Task 2: Process Control Syscalls
### Data Structures and Functions
#### In process.h
``` c++
struct parent {
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
  struct parent* parent;
  struct list children;
  #endif
  ...
}

```
### Algorithms
* halt
adfa
* exec
adf
* wait
* practice
