qUESTION:
Create a Process Id (PID) manager that keeps track of free PIDs and ensures that no two active processes are having the same pid. Once a process terminates the PID manager may assigns its pid to new process.
Use the following constants to identify the range of possible pid values: 
#define MIN PID 100 
#define MAX PID 1000 

You may use any data structure of your choice to represent the availability of process identifiers. One strategy is to adopt what Linux has done and use a bitmap in which a value of 0 at position i indicates that a process id of value i is available and a value of 1 indicates that the process id is currently in use. 
Implement the following API for obtaining and releasing a pid: 
• int allocate map(void)—Creates and initializes a data structure for representing pids; returns—1 if unsuccessful, 1 if successful • int allocate pid(void)—Allocates and returns a pid; returns— 1 if unable to allocate a pid (all pids are in use) • void release pid(int pid)—Releases a pid  
 
Modify the above problem by writing a multithreaded program that tests your solution. You will create a number of threads—for example, 100—and each thread will request a pid, sleep for a random period of time, and then release the pid. (Sleeping for a random period of time approximates the typical pid usage in which a pid is assigned to a new process, the process executes and then terminates, and the pid is released on the process’s termination.) 


INSTRUCTION:
1.	Before we implement the code first we have to take some instruction
•	In this project, first of all we have to implement a process IDE (PID) manager in c for linux. An operating System PID manager is responsible for managing process identifier . when  a process is first created . It is  assigned a unique PID by PID manager 
•	The PID is returned to the PID manager When the process complete Execution and the manager may later reassign this PID
•	In this we have to recognize most important that
	Process identifier must be unique
	No two active  process can have same PID
2.	 The alogorith for proposed solution of the assigned problem
Step-1: Implement the following function for optaining and releasing PID:
1.	allocate_map()
 			function initializes the pid statuses.
*(Creates and initializes data structure for representing pids;
 returns -1, 1 if successful)
			int allocate_map(void)        //initializing pid statuses
{
  int k;
 				 for(k=0;k<ARR_SIZE;k++)
 				 {
  					  pid_status[k]=0;
 				 }
  return 0;               //return 0 indicating statuses have initialized.
}
2.	allocate_pid() 
function returns a free pid from the data structure and changes the status                       of  that pid to 1 on calling  from a thread,  otherwise i.e., if no pid is free, it returns 1.
	*(Allocates and returns the largest PID available; returns -1 if unable to allocate a PID)
		int allocate_pid(void)
{
 			   int k,count=1;
   			   for(k=0; k<ARR_SIZE; k++)
{
       			 if(pid_status[k]==0)
				{
    				    pid_status[k]=1;
      			      count=0;
           		                  break;
       			 }
    			       }
 				   return count?-1:k;
}

3.	release_pid() 
function takes back the pid on calling from the thread. i.e., it changes the pid status of that pid back to 0.
void release_pid(int pid)
{  
    pid_status[pid]=0;
}
4.	display() function 
shows all the pids which are in use when it is called.
			void *display(void *arg)
{
 				int thread_id =  *(( int* )arg);
int pid = allocate_pid();
if(pid==-1)
{
printf("No PID available.");
   				 }
 				   Else
{
       				 printf("Thread [%3d] PID [%3d] Allocated\n",thread_id,pid+MIN_PID);
				int r=1+rand()%30;
				release_pid(pid);
       				 printf("Thread [%3d] PID [%3d] Released after %d sec\n",thread_id,pid+MIN_PID,r);    
   				 }
  				  pthread_exit(NULL);
}
