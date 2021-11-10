

# Rendezvous, every thread is waiting to the other to proceed
```
aArrived = Semaphore(0)
bArrived = Semaphore(0)

Thread A:
statement a1 
aArrived.notify()
bArrived.wait()
statement a2

Thread B:
statement b2
bArrived.notify()
aArrived.wait()
statement b2
```

Mutex: only one thread can enter 
Multiplex: buffered mutex 

# Barrier - multiple rendezvous 
A thread cannot enter the critical section before every thread are rendezvous

```
n = the number of threads 
count = 0 
mutex = Semaphore(1)
barrier = Semaphore(0) //Turnstile

# rendezvous

mutex.wait()
    count = count + 1
mutex.signal()

if count == n: barrier.signal()

barrier.wait()
barrier.signal()

# critical section

```

//Here we assures everyone is passed redezvous before entering critical section
```
- Turnsile composition
barrier.wait()
barrier.signal()
```
// What do this composition do? This composition can wait n threads, by checking a condition, we can release one thread relesing the rest. 

# Reusable barrier - everyone is finisched before entering a new section 
```
turnstile = Semaphore(0)
turnstile2 = Semaphore(1)
mutex = Semaphore(1)

#rendezvous 

mutex.wait() 
    count += 1 
    if count == n:
        turnstile2.wait()
        turnstile.signal()
mutex.signal()

turnstile.wait()
turnstile.signal()

//Turnstile2 has value 0, turnstile has value 1

# critical point

mutex.wait() 
    count -= 1 
    if count == 0:
        turnstile.wait()
        turnstile2.signal()
mutex.signal()

turnstile2.wait()
turnstile2.signal()
```

This can be implemented as java monitor

# Queue 
*NB! Two types of threads*

one thread cannot be signaled untill another thread is waiting. Thread must be paired
 ```
leaderQueue = Semaphore(0)
followerQueue = Semaphore(0)

Leader thread: 
followerQueue.signal()
leaderQueue.wait()
dance()

follower thread:
leaderQueue.signal()
followeQueue.wait()
dance()
```

# Exclusive Queue 
*NB! Two types of threads* 
The same thread that signaled you enters the section with you 

```
leaders = followers = 0 
mutex = Semaphore(1)
leaderQueue = Semaphore(0)
followerQueue = Semaphore(0)
rendezvous = Semaphore(0)

leader thread: 
-------------
mutex.wait()
if followers > 0:
    followers--
    followerQueue.signal()
else: 
    leaders++
    mutex.signal()
    leaderQueue.wait()

dance()
rendezvous.wait()
mutex.signal()

follower thread:
--------------- 
mutex.wait()
if followers > 0:
    followers--
    followerQueue.signal()
else: 
    followers++
    mutex.signal()
    followerQueue.wait()

dance()
rendezvous.signal()
```

# Producer-consumer 
*NB! Two types of threads*
Throw ball. A thread must wait untill the other one have returned. 

```
mutex = Semaphore(1)
items = Semaphore(0)

Producer thread: 
event = waitForEvent()
mutex.wait()
    buffer.add(event)
mutex.signal
items.signal()

Consumer thread: 

items.wait()
mutex.wait()
    event = buffer.get()
mutex.signal()
event.process()
```

# Finite buffer producer-consumer **
*NB! two types of threads*
Here the producer cannot produce if an consumer did not appear
Restricted number of producers

```
mutex = Semaphore(1)
items = Semaphore(0)
spaces = Semaphore(buffer.size()) //Initialized with the size of buffer

producer thread: 

event = waitForEvent()

spaces.wait()
mutex.wait()
    buffer.add(event)
mutex.signal()
items.signal()

consumer thread: 

items.wait() 
mutex.wait()
    event = buffer.get()
mutex.signal()
spaces.signal()

event.process() 
```

# Readers-writers problem - categorical mutual exclusion **
*NB two types of thread*

Several readers can read a shared variable, however a writer can only enter if readers is not present
```
int readers = 0
mutex = Semaphore(1)
roomEmpty = Semaphore(1)

writers thread:

roomEmpty.wait()
    critical section
roomEmpty.signal()

# readers thread: 

mutex.wait()
    readers += 1
    if readers == 1:
        roomEmpty.wait()
mutex.signal() :: LOCK, we define functions based on mutex locked and signaled (AA)

critical section

mutex.wait()
    readers -= 1
    if readers == 0:
        roomEmpty.signal()
mutex.signal() :: UNLOCK
```

// We can collect the readers solution in an object. We call this lightSwitch 

# Readers-writers problem without starvation **
*NB! two types of thread*

We secure that if a writer apper we will block new readers.
```
readSwitch = LightSwitch() //initialize object 
roomEmpty = Semaphore(1)
turnstile = Semaphore(1)

writers thread: 
--------------
turnstile.wait()
    roomEmpty.wait()
    # critical section
turnstile.signal()

readers thread: 
---------------
turnstile.wait()
turnstile.signal()

readSwitch.lock(roomEmpty)
// Critical section
readSwitch.unlock(roomEmpty)
```

# Reader writers problem - writers have priority 
*NB! Two types of threads*


```
readSwitch = LightSwitch()
writeSwitch = LightSwitch()
noReaders = Semaphore(1) //We wait this if there are readers
noWriters = Semaphore(1)

reader thread:
-------------
noReaders.wait()
    readSwitch.lock(noWriter)
noReaders.signal()

    #Critical section

readSwitch.unlock(nowriter)

writer thread: 
-------------

writeSwitch(noReader)
    noWriters.wait()
        #critical section
    noWriters.signal()
writeSwitch(noReader)
```

Here we see that we give writer access to more semaphores than readers. 

# No starve mutex
Here we utilize weak semaphores (Semaphores that might allow starvation) to get a strong Semaphore. 

-> Guarantees that every thread gets to process. 

```
room1 = room2 = 0
mutex = Semaphore(1)
t1 = Semaphore(1)
t2 = Semaphore(0)

mutex.wait()
    room1 += 1 
mutex.signal()

t1.wait()
    room2 += 1 
    mutex.wait()
    room1 -= 1 

    if room1 == 0:
        mutex.signal()
        t2.signal()
    else: 
        mutex.signal()
        t1.signal()
t2.wait()
    room2 -= 1 

    #critical section 

    if room2 == 0:
        t1.signal()
    else: 
        t2.signal()
```

# Dining philosophers
There are 5 spots for philosopers to eat. There are two resources required to eat, and they are next to each philosopher. 

Basic philosopher loop: 
```
while True:
    think() 
    get_forks()
    eat()
    put_forks() 
```

**At least one philosopher must be lefty, and one other must be righty**. 

*Tanenbaums solution* 

```
state = ['thinking']*5
sem = [Semaphore(0) for i in range(5)] //To avoid a thread to proceed
mutex = Semaphore(1)   //To secure mutual exclusion of shared variable 


def get_fork(i)
    mutex.wait()
    state[i] = 'hungry'
    test(i)
    mutex.signal()
    sem[i].wait()

def put_fork(i)
    mutex.wait()
    state[i] = 'thinking'
    test(right(i))
    test(left(i))
    mutex.signal()

def test(i)
    if state[i] == 'hungry' and state(right(i)) != 'eating' and state(left(i)) != 'eating':
        state[i] = 'eating'
        sem[i].signal()
 ```

 However this solution does not prevent starvation

 # Cigaret smokers problem
 One agent has infinite supplies of resources and chooses to allocate two of this resources at a time. The three agents has only one of the sources avaliable. If the agent is woken, the right process (smoker) must be chosen to proceed. 

 ```
 agentSem = Semaphore(1)
 tobacco = Semaphore(0)
 paper = Semaphore(0)
 match = Semaphore(0)

 There are three different threads representing the separate operation of an agent

 agentSem.wait()
 tobacco.signal()
 paper.signal()

 agentSem.wait()
 tobacco.signal()
 match.signal()

 agentSem.wait()
 paper.signal()
 match.signal()

 isTobacco = isPaper = isMatch = False 
 tobaccoSem = Semaphore(0)
 paperSem = Semaphore(0)
 matchSem = Semaphore(0)

 we use three helper threads puchers 

 pusher A 

 tobacco.wait()
 mutex.wait()
    if isPapaer: 
        isPapaer = False 
        matchSem.signal()
    elif isMatch
        isMatch = False 
        paperSem.signal()
    else: 
        isTobacco = True 
mutex.signal()

Smoker with tobacco: 
tobaccoSem.wait()
makeCigarette()
agentSem.signal()
smoke()
```

# Generalized smokers problem 
Here we assume that the agent does not wait after it has allocated resouces. Hence there can be more than one resource avaliable

```
numTobacco = numPaper = numMatch = 0

Puscher A 
tobacco.wait()
mutex.wait()
    if numPaper: 
        numPaper -= 1 
        matchSem.signal()
    elif numMatch: 
        numMatch -= 1
        paperSem.signal()
    else: 
        numTobacco += 1 
mutex.signal()
```

# The dining savages problem
There is a huge pot of meals, each savages enters and take a meal. When there is no meals left, they wake the cook.

```
servings = 0
mutex = Semaphore(1)
emptyPot = Semaphore(0)
fullPot = Semaphore(0)

The solution utilizes a combination of rendezvous and scoreboard

cook thread: 
-----------
while True:
    emptyPot.wait()
    putServingsInPot(M)
    fullPot.signal()

savage thread:
-------------
while True: 
    mutex.wait()
        if servings == 0:
            emptyPot.signal()
            fullPot.wait()
            servings = M 
        servings -= 1 
        getServingsFromPot()
    mutex.signal()

    eat() 
```
deadlock free solution!







 
