# Synchronization in Java

An object has mechanisms to secure mutual exclusion between threads by marking the processes with synchronized. From inside of a synchronized method one can wait a thread, giving away the lock. Notify wakes one thread, notifyAll wakes everyone in chronological order 

Synchronization is done via Java monitors, one can block threads inside, potentially create deadlocks

General syntax:

```Java
class object {
    private datatype shared_variable;

    synchronized return_type func(){

        notifyAll();
        notify();
        wait()
    }
}

```
Bounded buffer:

```Java

public synchronized void put(int item){
    while (numberInBuffer == size) wait();
    last = (last + 1) % size ;
    numberInBuffer++;
    buffer[last] = item;
    notifyAll();
} 

public synchronized int get(){
    while (numberInBuffer == 0) wait();
    first = (first + 1) % size ;
    numberInBuffer--;
    notifyAll();
    return buffer[first];
}
```

Barrier Java: 

```Java
class Barrier {

    private int count = 0; 

    synchronized void increment(){
        count++; 
        while (count != 3) wait(); 

    }

}