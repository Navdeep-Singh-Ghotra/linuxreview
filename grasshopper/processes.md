ps (Processes)
Processes are the programs that are running on your machine. They are managed by the kernel and each process has an ID associated with it called the process ID (PID). This PID is assigned in the order that processes are created.

$ ps
    PID TTY          TIME CMD
1206075 pts/2    00:00:00 bash
1224914 pts/2    00:00:00 ps

This shows you a quick snapshot of the current processes:

PID: Process ID
TTY: Controlling terminal associated with the process (we'll go in detail about this later)
STAT: Process status code
TIME: Total CPU usage time
CMD: Name of executable/command

If you look at the man page for ps you'll see that there are lots of command options you can pass, they will vary depending on what options you want to use - BSD, GNU or Unix. In my opinion the BSD style is more popular to use, so we're gonna go with that. If you are curious the difference between the styles is the amount of dashes you use and the flags.


$ ps aux
The a displays all processes running, including the ones being ran by other users. The u shows more details about the processes. And finally the x lists all processes that don't have a TTY associated with it, these programs will show a ? in the TTY field, they are most common in daemon processes that launch as part of the system startup.

You'll notice you're seeing a lot more fields now, no need to memorize them all, in a later course on advanced processes, we'll go over some of these again:

USER: The effective user (the one whose access we are using)
PID: Process ID
%CPU: CPU time used divided by the time the process has been running
%MEM: Ratio of the process's resident set size to the physical memory on the machine
VSZ: Virtual memory usage of the entire process
RSS: Resident set size, the non-swapped physical memory that a task has used
TTY: Controlling terminal associated with the process
STAT: Process status code
START: Start time of the process
TIME: Total CPU usage time
COMMAND: Name of executable/command
The ps command can get a little messy to look at, for now the fields we will look at the most are PID, STAT and COMMAND.

Another very useful command is the top command, top gives you real time information about the processes running on your system instead of a snapshot. By default you'll get a refresh every 10 seconds. Top is an extremely useful tool to see what processes are taking up a lot of your resources.

$ top

Controlling Terminal
We discussed how there is a TTY field in the ps output. The TTY is the terminal that executed the command.

There are two types of terminals, regular terminal devices and pseudoterminal devices. A regular terminal device is a native terminal device that you can type into and send output to your system, this sounds like the terminal application you've been launching to get to your shell, but it's not.

We're gonna segue so you can see this action, go ahead and type Ctrl-Alt-F1 to get into TTY1 (the first virtual console), you'll notice how you don't have anything except the terminal, no graphics, etc. This is considered a regular terminal device, you can exit this with Ctrl-Alt-F7.

A pseudoterminal is what you've been used to working in, they emulate terminals with the shell terminal window and are denoted by PTS . If you look at ps again, you'll see your shell process under pts/*.

Ok, now circling back to the controlling terminal, processes are usually bound to a controlling terminal. For example, if you were running a program on your shell window such as find and you closed the window, your process would also go with it.

There are processes such as daemon processes, which are special processes that are essentially keeping the system running. They often start at system boot and usually get terminated when the system is shutdown. They run in the background and since we don't want these special processes to get terminated they are not bound to a controlling terminal. In the ps output, the TTY is listed as a ? meaning it does not have a controlling terminal.


Process Details
Before we get into more practical applications of processes, we have to first understand what they are and how they work. This part can get confusing since we are diving into the nitty gritty, so feel free to come back to this lesson if you don't want to learn about it now.

A process like we said before is a running program on the system, more precisely it's the system allocating memory, CPU, I/O to make the program run. A process is an instance of a running program, go ahead and open 3 terminal windows, in two windows, run the cat command without passing any options (the cat process will stay open as a process because it expects stdin). Now in the third window run: ps aux | grep cat. You'll see that there are two processes for cat, even though they are calling the same program.

The kernel is in charge of processes, when we run a program the kernel loads up the code of the program in memory, determines and allocates resources and then keeps tabs on each process, it knows:

The status of the process
The resources the process is using and receives
The process owner
Signal handling (more on that later)
And basically everything else
All processes are trying to get a taste of that sweet resource pie, it's the kernel's job to make sure that processes get the right amount of resources depending on process demands. When a process ends, the resources it used are now freed up for other processes.