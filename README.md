Download Link: https://assignmentchef.com/product/solved-cs2106-tutorial-4-ipc-and-threads
<br>
<ul>

 <li>processes accessing and modifying data at the same time. Compile and run the code in <strong>c</strong>. When running the executable, you can specify two command line arguments,<strong> n</strong> and <strong>nChild</strong>; if no command line arguments are given, the default values will be used: n is initialized to 100 and nChild is initialized to 1.</li>

</ul>




The code in shm.c does a simple job: the parent creates nChild processes and then each process, the parent included, increases a shared memory location by n times. After all processes have finished incrementing the shared value, the parent prints the shared result and exits. Because the value of the shared memory is initialized to 0, we expect the printed value to be equal to (1 + nChild) * n at the end. Is this always the case?




Try running the program multiple times with increasingly higher values for n and nChild (n = 10000 and nChild = 100 should do the trick). Explain the results.




<ul>

 <li>(Message Passing) In the lecture we talked about message passing between processes. Here we are implementing <strong>Direct Communication</strong> beween master and worker processes.</li>

</ul>




As a continuation of Q3 from Tutorial 2, let us investigate how to implement the same prime factorization program using message passing. Fill in the pseudo code for the master and worker processes below. Your pseudocode must ensure that the master process receives results from workers in the same order as the user input is sent to workers by the master.




For each of the message passing operations, indicate whether it is blocking or non-blocking.




For simplicity, you can assume the following: We have 1 master process and N worker processes. Master process is responsible for distributing workload among worker processes. Worker processes have been already spawned via a fork call by master process.







<table width="606">

 <tbody>

  <tr>

   <td width="606"><strong>Master Process:</strong></td>

  </tr>

  <tr>

   <td width="606"><strong>workerPid[] = PID array for 1..N worker processes</strong>  <strong>N = number of inputs</strong>  <strong>inputs[] = N user inputs</strong> <strong>//Give user input to the corresponding worker to work on</strong>  <strong>//Wait for each worker to finish and get back the result</strong>  </td>

  </tr>

 </tbody>

</table>




<table width="607">

 <tbody>

  <tr>

   <td width="607"><strong>Each Worker Process:</strong></td>

  </tr>

  <tr>

   <td width="607"><strong>//Get the user input to work on from master</strong><strong>result = PrimeFactorization(____________);</strong>  <strong>//Send back the result to master</strong>  </td>

  </tr>

 </tbody>

</table>

<ul>

 <li>(Thread vs Process)

  <ol>

   <li>For each of the following scenarios, discuss whether <strong>multithreading </strong>or <strong>multiprocessing </strong>is the best implementation model. If you think that both models have merits for a particular scenario, briefly describe additional criteria you would use to choose between the models.

    <ol>

     <li>Implementing a command line shell.</li>

     <li>Implementing the “tabbed browsing” in a web browser, i.e., each tab visits an independent webpage.</li>

    </ol></li>

  </ol></li>

</ul>

<ul>

 <li>Implementing a complex multi-player game with dynamic environments and sophisticated.</li>

</ul>

<ol>

 <li>Suppose task A is CPU-intensive and task B is I/O intensive (reading from a file), which of the following statement(s) is/are TRUE if the two implementations are executed on a single CPU (single core)?

  <ol>

   <li>If the threads are implemented as user threads, then multithreaded implementation will finish execution in shorter amount of time compared to the sequential implementation.</li>

   <li>If the threads are implemented as kernel threads, then multithreaded implementation will finish execution in shorter amount of time compared to the sequential implementation.</li>

  </ol></li>

</ol>







<ul>

 <li>(Threads)

  <ol>

   <li>Suppose we have the following multi‐threaded processes on a <strong>hybrid thread </strong>model:</li>

  </ol></li>

</ul>

<table width="602">

 <tbody>

  <tr>

   <td colspan="2" width="200">Process ID</td>

   <td width="201">Number of User Threads</td>

   <td colspan="2" width="201">Number of Kernel Threads</td>

  </tr>

  <tr>

   <td colspan="2" width="200">P1</td>

   <td width="201">U1</td>

   <td colspan="2" width="201">K1</td>

  </tr>

  <tr>

   <td colspan="2" width="200">P2</td>

   <td width="201">U2</td>

   <td colspan="2" width="201">K2</td>

  </tr>

  <tr>

   <td colspan="2" width="200">P3</td>

   <td width="201">U3</td>

   <td colspan="2" width="201">K3</td>

  </tr>

  <tr>

   <td width="62"></td>

   <td width="138">i)     If all threads are</td>

   <td colspan="2" width="361"> CPU intensive and run forever, what is the approximate</td>

   <td width="41"></td>

  </tr>

  <tr>

   <td width="62"></td>

   <td width="138"></td>

   <td width="201"></td>

   <td width="160"></td>

   <td width="41"></td>

  </tr>

 </tbody>

</table>

proportion of CPU time utilized by the process P3 after running for a long time? You can assume a pre-emptive and fair process scheduler is used by the OS. You can express your answer in terms of U1-U3 and K1-K3.

<ol>

 <li>ii) When [U1 = 4, K1 = 3] [U2 = 3, K2 = 1] [U3 = 5, K3 = 2] what is the CPU utilization by one user thread of process P1? Does the number of user threads of other process affect this value? State your assumptions.</li>

 <li>Evaluate each of the following statements regarding Thread and POSIX Thread (pthread) as True or False.

  <ol>

   <li>All pthreads must execute the same function when they start.</li>

   <li>Multi-threaded program using pure user threads can never exploit multi-core processors.</li>

  </ol></li>

</ol>

<ul>

 <li>If we use a 1-to-1 binding in a hybrid thread model, the end result is the same as pure-user thread model.</li>

</ul>

<ol>

 <li>On a single-core processor that supports simultaneous multithreading, we can execute more than one thread at the <strong>same time</strong>.</li>

</ol>




<strong>Questions for your own exploration</strong>




<ul>

 <li>(Protecting the Shared Memory) Allowing processes to access and modify shared data whenever they please can be problematic! Therefore, we would like to modify the code in <strong>c</strong> such that the output is deterministic regardless of how large n and nChild are. More precisely, we want the processes to take turns when modifying the result value that resides in the shared memory. To this end, we will add another field in the shared memory, called the order value, that specifies which process’ turn it is to increment the shared result. If the order value is 0, then the parent should increase the shared result, if the order value is 1, then the first child should increase it, if the order value is 2, then the second child and so on. Each process has an associated pOrder and checks whether the value order is equal to its pOrder; if it is, then it proceeds to increment the shared result. Otherwise, it waits until the order value is equal to its pOrder.</li>

</ul>




Your task is to modify the code in <strong>shm_protected.c</strong> to achieve the desired result. You will have to:

<ol>

 <li>Create a shared memory region with two locations, one for the shared result, and the other one for the order value;</li>

 <li>Write the logic that allows a process to modify the shared result only if the order value is equal to its pOrder (the skeleton already takes care of assigning the right pOrder to each process);</li>

 <li>Print the result value and cleanup the shared memory.</li>

</ol>

Why is <strong>shm</strong> faster than <strong>shm_protected</strong>? Why is running <strong>shm_protected</strong> with large values for nChild particularly slow? (Hint: You may want to take a look at the output of <em>htop</em>)


