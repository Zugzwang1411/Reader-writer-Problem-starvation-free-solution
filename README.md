# Reader-writer-Problem-starvation-free-solution
OS assignment
We have to implement a solution for the reader-writers problem without any starvation.
In classical solutions it is seen that starvation for writers happens which can be allowed in certain scenarios but in our solution we'll be aiming on reaching a solution where starvation doesn't happen.
First let's understand what ar readers and writers here.
We have a shared database and readers, read data from the shared databse and writers write in the shared database.
Pseudo code for starve free solution goes as:- 

int reader_count=0
binary semaphore enter=1;
binary semaphore reader=1;
binary semaphore writer=1;

void Reader(){
while(1){
P(enter)
 p(reader)
 reader_count++;
 if(reader_count==1)
   P(writer)
 V(reader)
V(enter)
// Enter THE CRITICAL SECTION
P(reader)
 reader_count--
V(reader)
if(reader_count==0)
v(writer)
}
}
**********************
void Writer(){
while(1){
P(enter)
 p(writer)
 V(enter)
   // ENTERS CRITICAL SECTION
V(writer)
}
}
*******************************
Now we can understand how we are tackling the issue of starvation here.
To mantain equality smong readers and writers we are introducing a new semaphore with name enter. This enter semaphore is not biased to any of the reader or writer.
So if readers keep on coming then they will also get blocked and the writer which got blocked  earlier will get chance to execure hence overcoming the issure of starvation of writers.
Now let's understand the logic of reader and writer here.
In entry code of reader it first checks if another reader/writer is in entry code, if yes then it gets blocked and if no it proceeds.
then it checks if  reader is in exit code(because entry code condition has already been checked) if yes it gets bloked else the counter for the number of readers is increased and for the first reader entering we down the semaphore of writer because till even 1 reader is in critical section we cannot let writers enter the critical section.
After this reader semaphore is signalled and enter semaphore is also signalled allowing new reader/writer to enter.
After this our reader process enters the critical section(given it doesnt gets blocked in a early stage)
in exit section of readers problem.
First, we down the reader semaphore as we are changing the value of counter becaue otherwise we may get errors if a proces gets pre-empted unexpectedly and this may lead to weing results.
Then we drop the counter by 1 and signal the reader semaphore. Now we check if counter is zero because then we allow writers to enter by signallign the writer semaphore.

Now we understand the writers code.
In entry section we see first we cheeck if anny reader or writer is in entry section of their codes if yes we block the request else the query proceeds(we check enter semaphore here), now we down the writer semaphore so that no other writer can enter the critical section to avoid writer-writer(W-W) problem. Now our writer is ready to enter the critial section we release the enter semaphore and our writer enters the critical section.
In exit section of writer we free the writer semaphore to allow other writers to enter the critical section.

Now we can check the correctness of our solution as:-

At any point of time we re avoiding any of our reader-writer, writer-writer and writer reader problems as this is ensured. Entry is assigned with the help of a non biased semaphore and then entry into the critical section is monitored with the help of semaphores "reader" and "writer"  allowing only one writer or multiple readers(possibly one) process(processes) in critical section.
This ensures mutual exclusion of both our reader-writer processes.

No suh case happens when a process is blocked and waiting for a resource to be release and other process enters and also gets blocked having the resource which earlier process wanted but getting blocked t=due to resource which is taken byt he earlier blocked process i.e. no condition of deadlock caan happen in our code.
Hence no deadlock occurs i.e. progress is satisfied.

In the classical solution there is problem of infinite witing for writers but here the is bounded wait for both readers and writers as "enter" semaphore makes sure that no biase happens between reader and writer queries. Hence bounded wait criteria is satisfied.

Hence we can satisfactorily say that the given solution is correct and starve free.
2
 

 
