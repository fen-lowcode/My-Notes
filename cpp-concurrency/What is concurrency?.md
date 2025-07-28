

### Simple Explanation of Concurrency

Concurrency in a simplest explanation is that two or more event happens at the same time, i can drink a coffee while you read, someone can sail a boat while the other is a look out, someone is struggling while the other is happy all happening at the same time.

### Concurrency in Computer Systems

In Computer Systems, concurrency means a single system operates and performs multiple task in all concurrently rather than one by one.

This isn't new Multitasking Operating  Systems that allow a single desktop computer to run  multiple applications at the same time  through task switching have been around for many years.

Some even have high-end machines that have multiple processors that enable genuine concurrency

What new is that increased prevalence of computers that can genuinely run multiple tasks in parallel rather than giving the illusion of doing so.


> **Single-processor**

Most desktop computers only have a single processor with single processing unit or core, and this remains true for many desktop today.

This machine can only perform one task at a time but can switch between many task many times very quickly, with this a bit of one task and then a bit of another and so on, it happens that  the tasks is all being performed at the same time. 

This is called ***[[Task-Switching]]***, We still talk about concurrency with such systems like this because the task switches are so fast, you can't tell when a program is suspended as the processor switches from one to another. 

>[!important]
The task switching provides the illusion of  concurrency for the user and also for the application themselves. Because this is only an illusion of concurrency, the behavior of the application may be subtly different when executing in a single-processor task-switching compare to when  executing  in an environment with true concurrency.


>**Multi core-processors**


Computers containing multiple processors have been used for servers and high-performing computing task for many years. and computers based on processor with more than one cores on a single chip (also known as **[[Multi core-processors]]**) are becoming increasingly common as desktop machines.

>[!important]
Computers with multiple processors or multiple cores in a single processor (can be also both) are capable of true concurrency that can run multiple task in parallel. this is called **Hardware concurrency**






