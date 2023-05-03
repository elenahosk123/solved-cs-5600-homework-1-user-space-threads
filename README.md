Download Link: https://assignmentchef.com/product/solved-cs-5600-homework-1-user-space-threads
<br>
In this homework you will write a user-space thread system (named “qthreads”, vs. the standard “pthreads” library), which creates threads, performs context switches, and includes implementations of mutexes and condition variables. You will use your code in two applications – your own test procedures, and a simple threaded webserver provided as part of the example.

Programming Assignment materials and resources

You will download the skeleton code for the assignment from the CCIS repository server, github.ccs.neu.edu, using the ‘git clone’ command:

git clone <u><a href="https://github.ccs.neu.edu/cs5600-03-sp2018/hw1">https://github.ccs.neu.edu/cs5600-03-sp2018/hw1</a></u>

You should <strong>copy</strong> this code to a subdirectory in your github team repository under <strong>hw1</strong>. For example if you are on team 1

git clone <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7c1b15083c1b150814091e521f1f0f5212190952191809">[email protected]</a>/cs5600-03-sp2018/team-1      cd team-1      mkdir hw1      cp -a /path/to/hw1/* hw1 So that your team repository looks like

ls team-1

You should be able to complete this homework on any x86 Linux system however, the official environment is the CS-5600 virtual machine which you used for Homework 0, and which may be downloaded from  <u><a href="http://www.ccs.neu.edu/~jrafkind/cs5600/CS5600-f17-Ubuntu32.ova">http://www.ccs.neu.edu/~jrafkind/cs5600/CS5600-f17-Ubuntu32.ova</a></u>

You can also use the NEU servers typically accessed via ssh

Note that there will be a video on Blackboard explaining (in considerable detail) how to go about implementing your solution.

<h1>Repository Contents</h1>

The repository you clone contains the following files:

<strong>qthread.c, qthread.h</strong> – these are the files you will be implementing. They have some comments in them describing the functions and types you have to implement.

<strong>switch.s, stack.c –</strong> these contains the context switch function (switch_from_to) and the stack initialization function (init_stack). You should not change these files. The functions are more complex than the the ones discussed in class because they contain protection code designed to abort into the debugger if you try to switch to an invalid stack.

<strong>test1.c –</strong> this is where your test code should go. (well, you can add additional files and update the makefile if you do) <strong>server.c –</strong> a simple multi-threaded webserver using qthreads.

<strong>Makefile</strong> – this is set up to build executables for ‘test1’ and ‘server’. Compile your code by typing ‘make’.

Deliverables

The following files from your repository will be examined, tested, and graded:

<strong>qthread.c, qthread.h test1.c</strong>

Additional information

A video is being posted to Blackboard which will provide a lot of suggestions about how to complete this assignment. Note that it’s possible to implement qthreads in less than 300 lines of code – it doesn’t have to be huge and complex.

Qthreads interface

First, two function pointer typedefs – one for a function taking two arguments (with no return value) and the other taking one argument and returning ‘void*’. These will make the following definitions easier to read.

typedef void (*f_2arg_t)(void*, void*); typedef void * (*f_1arg_t)(void*);

You will need to implement the following functions – first, the basic functions to start and exit a thread, run the thread scheduler, and wait for a thread to finish (“join”).

qthread_t qthread_start(f_2arg_t f, void *arg1, void *arg2); qthread_t qthread_create(f_1arg_t f, void *arg1);

void qthread_run(void); void qthread_exit(void *arg); void *qthread_join(qthread_t thread);

Mutex and condition variable functions:

void qthread_mutex_init(qthread_mutex_t *m); void qthread_mutex_lock(qthread_mutex_t *m); void qthread_mutex_unlock(qthread_mutex_t *m); void qthread_cond_init(qthread_cond_t *c);

void qthread_cond_wait(qthread_cond_t *c, qthread_mutex_t *m); void qthread_cond_signal(qthread_cond_t *c); qthread_cond_broadcast(qthread_cond_t *c);

And finally functions to replace some of the blocking sleep and I/O calls in the Unix interface:

void qthread_usleep(long int microsecs); ssize_t qthread_read(int fd, void *buf, size_t len); ssize_t qthread_write(int fd, void *buf, size_t len);

int qthread_accept(int sock, struct sockaddr *addr, socklen_t *len);