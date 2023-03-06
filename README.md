# Starve-Free-Reader-Writer-Problem
## Enrolment no : 21114020

## Solution

### Variables

```cpp
mutex = Semaphore(1) // binary semaphore for mutual exclusion
readers_waiting = Semaphore(1) // binary semaphore for readers waiting
writers_waiting = Semaphore(1) // binary semaphore for writers waiting
readers = 0 // number of active readers
```


### Starve Free Pseudo code for : READER

```cpp
Reader():
    while (true)
        // wait for permission to read
        wait(mutex)
        wait(readers_waiting)
        // update number of active readers
        readers = readers + 1
        if readers == 1
            // first reader, wait for all writers to finish
            wait(writers_waiting)
        signal(readers_waiting)
        signal(mutex)
        
        // CRITICAL SECTION
        

        wait(readers_waiting)
        readers = readers - 1
        if readers == 0:
            // last reader, release the writers_waiting
            signal(writers_waiting)
        signal(readers_waiting)
```

### Starve Free Pseudo code for : WRITER

```cpp
Writer():
    while (true)
        // wait for permission to write
        wait(mutex)
        wait(writers_waiting)
        
        //Critical Section
        
        // Release mutex and writers_waiting
        signal(writers_waiting)
        signal(mutex)
```

## Explaination

- Declare and initialize three semaphores: readers_waiting, writers_waiting, and mutex. readers_waiting is used to protect the counter variable, writers_waiting is used to ensure that only one writer can access the critical section at a time, and mutex is used to prevent any new readers or writers from entering the critical section while another process is already inside.
### Readers
- In start of the Readers Loop, acquire/wait the mutex and readers_waiting semaphores to prevent any new processes from entering the critical section while another process is inside. Increment the 'readers' variable to indicate that a new reader is entering the critical section. If this is the first reader to enter (i.e., readers == 1), acquire/wait the writers_waiting semaphore to prevent any writers from entering the critical section. And release mutex and readers_waiting

- In the end of the loop i.e: after Critical section, acquire/wait the readers_waiting semaphore to protect the counter variable, then decrement the 'readers' variable to indicate that a reader is leaving the critical section. If this is the last reader to leave (i.e., 'readers' == 0), release the writers_waiting semaphore to allow any waiting writers to enter the critical section. Release the readers_waiting semaphore to allow other readers to enter the critical section.

### Writers
- In the start of the Writers Loop, acquire/wait the mutex and writers_waiting semaphores to prevent any new processes from entering the critical section while a writer is inside, and to ensure that only one writer can enter the critical section at a time.
- In the end of the loop, release the writers_waiting semaphore to allow other processes to enter the critical section. Release the mutex semaphore to allow other readers or writers to enter the critical section.


