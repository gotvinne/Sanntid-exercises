#   Rendezvous / Barrier /Reusable barrier 
    * All threads must finish the rendezvous before proceeding
    * Turnstile 
#   Queue / Exclusive queue
    * One thread can only proceed if another one is waiting. Two queues
    * The exclusive version is that the one thread that woke up the queue proceeds simultaneously 
#   Producer-consumer / finite buffer 
    * One thread must wait untill another thread has returned 
#   Reader-writer
    1) no-starve
    Mutiple readers can access shared variable, writers must have mutual exclusion
    2) writer priority 
    A writer present implies readers must be blocked. 
#   No-starve mutex
    * Setup barrier such that every process get to run, without any thread starving
#   Dining philosophers 
    There are a shared variable everyone wants, however one must have to resources requierd from other threads to proceed.
    1) No starve 
    There is at least one lefty and righty present
    2) Tanenbaums
#   Cigaretts smokers problem 
    There is an agent supply resources to the smokers. To avoid deadlock only one thread can proseed based on the avaliable resources. 
    This is solved by helper threads  
    2) Generalized Cigarett smokers: 
    There are multiple resources avaliable

#   Dining savages 
    Savages empty the resource avaliable one after one, when there are no more resources a cook is requested. 

#   The barbershop 
    A barbershop consist of a waiting room with n chairs and a barber room containing one chair. If there are no costumers the barber goes to sleep. If a costumer enter and there are no avaliable chairs, the costumer leaves. But if chairs avaliable, the barber awake. 

#   The FIFO barbershop 
    Barbershop where costumers are served in the order they apperared 

#   Hilzer barbershop - complicated version
    more advanced waiting room

#   The SantaClaus problem 
    Santa Claus can only be woked if three elves are having problem, or all reindeers are present. If both this conditions are met the reindeers trumph. 
#   Building H2O
    The thread passes the barrier by complete set. both oxygen and hydrogen. 

#   River crossing problem 
    Only one combination of hackers, serfs and a total number on the ferry is allowed to pass the river. 

#   The roller coaster problem 
    N passengers and one car with C capasity. C < n. The car can only go around when it is full. 

#   The search-insert delete problem 
    Three kinds of threads share access to a singly linked list: searchers, inserters and deleters. Searchers only examine: multiple serchers can access the list. Inserters add and must be mutually excluded. One insert can proceed with searchers. At most one deleter can access at a time, excludeed from the others 
#   the unisex bathroom problem 
    There cannot be men and women present in same bathroom. there should never be more than three employed present 

#   Baboon crossing problem 
    Up to 5 babones can access a rope without breaking. If two baboons go in the opposite direction they will be dead. 

#   The Modus Hall problem 
    Path is not wide enough to go side by side. If to Mods persons meet, they will gladly step aside. Same will happen for ResHall. However if one Mods and ResHall meet they will become violent. 

#   The suchi bar problem 
    Suchi bar with 5 seats. If they are avaliable you can sit, if they are no avaliavble, you must wait untill everyone is finished. 
#   The child care problem 
    There is always one adult present for three children. 

#   The room party problem 
    Any number of student can be present in the same room. The Dean can only enter if there are no students or more than 50 students. While Dean is present, no other can attend. The Dean may not leave before every other has left. 

#   The Senate Bus problem 
    Riders come to bus stop. When a bus arrives, all waiting thread can attend, but everyone arriving has to wait for next bus. The capasity is 50 persons. And if there are more waiting, they must wait. 

#   The Faneuil Hall problem 
    Three threads: immigrants, spectators and judge. 

#   The dining hall problem 

