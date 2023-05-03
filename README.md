Download Link: https://assignmentchef.com/product/solved-cs-4420-5420-programming-assignment-1
<br>
<strong>Parts I and II </strong>Exploring the Proc Filesystem

INTRODUCTION

In assignment 1, you will develop a set of tools to explore the Linux proc filesystem

Each of those tools are based on the Unix proc filesystem. The proc filesystem is located under “/proc” on every Unix system. It is really a pseudo-filesystem: the contents of proc are not actually stored anywhere on any mass storage device like in a real filesystem, but its contents (directories, files) are only created (“on the fly”) when someone accesses them.

At its root (i.e., /proc), the proc filesystem (on Linux) contains a number of directories and files. The directories either have integers as names (in which case they represent Unix process ID’s (PIDs), and contain various information the kernel maintains about a process that is currently running in the system), or they have other names. For example, the directory “/proc/1234” contains various information (i.e., files and other directories) about the process with the PID 1234.

This is an example of a /proc directory on a Linux computer:







In this example, you an see the directories with numbers as directory names on the left hand side (they correspond to processes; in this case in the range between 1 and 14634). In addition, you can see various other directories and files. They are used by the operating system to report various system information to users and applications. Most of the files are ASCII files that can be read by simply opening them in a text editor. For example, the file “/proc/cpuinfo” contains a detailed description of the properties of the CPU.

You can obtain information about a process simply by opening the right file in the directory corresponding to the process you’re interested in. For example, if you need to obtain some basic profiling statistics for a given process (example, the process with a process ID (PID) of 14584), you need to access the file “/proc/14584/stat”. It contains a single line of tokens (ASCII characters) separated by spaces.

Here is an example of a /proc/[pid]/stat file:

14584 (bash) S 14562 14584 14562 34824 14598 4194393 1061 1325 0 1 5 0 0 0 20 0 1 0 33837297 …

Each token is separated by a space character. The meaning of the individual tokens is described in the Linux manpage for the proc filesystem. The man page entry for “proc” gives you a detailed description of the format of this file. You can display the man page entry by using the “shell&gt; man proc” command (<strong>Note: you may need to specify the man page section in order to get the correct man page entry. Try, e.g., “man -s5 proc”)</strong>. The information for the “stat” file can be found under “/proc/[pid]/stat” on the manpage.

In the above example, the first token is “14584” which is the process ID of the process. The second token, “(bash)” is the command/program name of the process (in parenthesis). In our example it happens to be the bash shell program. The next token, “S”, denotes the current state of the process (which, according to the information in the manpage, in this case is “S” = sleeping). The following token “14562” is the process ID (PPID) of the parent process of our bash process, etc.

<h2>Part I: The “4420_proctool_1” Tool</h2>

<strong>Overview: </strong>

The “4420_proc1” tool is a very simple tool that prints some basic information for all the processes running on a Linux system. The tool does not require any additional command arguments.  You should be able to run the tool simply by using the following command:

Shell&gt; 4420_proctool_1

It will generate the following information (columns) for every process:

<table width="623">

 <tbody>

  <tr>

   <td width="124">Process ID (PID)</td>

   <td width="125">Command (nameof the program)</td>

   <td width="125">Parent Process ID (PPID)</td>

   <td width="124">Process State</td>

   <td width="125">Virtual       MemorySize (in bytes)</td>

  </tr>

 </tbody>

</table>




In order to create this output, your tool will need to access all of the the “/proc/&lt;pid&gt;/stat” files (i.e, you will need to enter every directory corresponding to a PID). All the information required can be directly extracted from the corresponding “/proc/&lt;pid&gt;/stat” files.

The following table shows some sample output:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0c687e697b7f4c7c793d">[email protected]</a>:~/proc_project$ ./a.out

PID      Command               PPID   State    VM Size (bytes) ———————————————————————-

<ul>

 <li>systemd 0    S     185796</li>

 <li>kthreadd 0    S          0</li>

 <li>ksoftirqd/0 2    S          0          7      rcu_sched                2    S          0 …  // some processes removed</li>

 <li>jbd2/sdb5-8 2    S          0</li>

 <li>ext4-rsv-conver 2    S          0</li>

</ul>

300      rpciod                   2    S          0

308      systemd-journal          1    S      43892

<ul>

 <li>kauditd                 2    S          0</li>

 <li>lvmetad 1    S      94772</li>

</ul>

323      systemd-udevd            1    S      39040        385      kvm-irqfd-clean          2    S          0

…   // some processes removed

875      ext4-rsv-conver          2    S          0

1072      rpcbind                  1    S      47624

1091      ModemManager             1    S     413440

1095      cron                     1    S      22680

1099      atd                      1    S      19716

1105      rsyslogd                 1    S     320012

1111      bluetoothd               1    S      31956

1115      dbus-daemon              1    S     109008

1210      avahi-daemon             1    S     110916

1213      acpid                    1    S       4396

1220      systemd-logind           1    S      20572

1233      snapd                    1    S     537232

1241      avahi-daemon          1210    S     110556       1308      polkitd                  1    S     338460

… // some processes removed

<strong> </strong>

<strong>Specific Requirements: </strong>

<ul>

 <li>Implement the above described tool “4420_proctool_1”. The tool has no required command argument and should display the above described columns for each Linux process.</li>

 <li>You have to implement the tool either in C or C++. You are only allowed to obtain the information via the proc filesystem (as explained above) for this project. You should only use functions to open and read files and directories. Do not use any other functions (example: system(), getrusage(), etc.).</li>

 <li>The screenshot above shows a sample output of my reference tool.</li>

 <li>You will need to submit the source file(s), header files (if necessary), and you WILL NEED TO SUBMIT A <strong><u>MAKEFILE</u></strong>. I will grade the project on “pu1.cs.ohio.edu”. I will type “make” to build the tool. If “make” fails (due to an error), the GRADE FOR THIS PART OF ASSIGNMENT 1 WILL BE AN “F”!</li>

 <li>You will need to submit the program from “<strong><u>cs.ohio.edu</u></strong>” (NOTE: THIS IS IMPORTANT. THE SUBMISSION WILL NOT WORK FROM ANY OTHER (LAB)MACHINE OR SERVER!). The submission is simple: “cd” into your project folder and type the following command (exactly as is shown here):</li>

</ul>

“<strong>bash&gt; /home/drews/bin/442submit 1</strong>”. Should you need to re-submit your project (due to changes), you will need to add an override switch “-f” to the submission: “<strong>bash&gt; /home/drews/bin/442submit -f 1</strong>”.

<strong> </strong>

<strong>Deadline: </strong>

The project is <strong><u>due by Fridy, 9/27/2019 (11.59pm)</u></strong>. I will <strong><u>not accept any late submissions</u></strong>.

<strong>Grading: </strong>

Total possible points for Part I of the assignment: <strong>10 points </strong>

Breakdown:

<ul>

 <li>Each of the above columns yields 2 points * 5 columns = 10 points</li>

</ul>

<strong> </strong>

<strong>Additional Help and Resources: </strong>

This project is fairly straightforward. You can use any file access functions available in C/C++. I would recommend you use basic Linux system calls: open(), read(), close() (see “man -s2 open”, “man -s2 read”, man -s close”, etc.). But you could also use C standard I/O: fopen(), fread(), fclose(), or C++ standard I/O.

The only thing that may be a little unusual is that you will have to determine (and later enter) all the directories corresponding to process IDs under “/proc” (i.e., all the “/proc/&lt;pid&gt;” directories). The following sample code shows how to accomplish this in C:

#include &lt;stdio.h&gt;

#include &lt;sys/types.h&gt;

#include &lt;dirent.h&gt;

#include &lt;stdlib.h&gt;




int pid;

struct dirent *ep;

DIR* dp;

dp = opendir (“/proc”);

if (dp != NULL)

{

while (ep = readdir (dp))

{

pid = strtol(ep-&gt;d_name, NULL, 10);         if( ( ep-&gt;d_type == DT_DIR ) &amp;&amp; ( pid &gt; 0) )

{

puts (ep-&gt;d_name);

}       }

closedir(dp);

}   else

perror (“Couldn’t open the directory”);

<strong> </strong>

<h2>Part II: The “4420_proctool_2” Tool</h2>

<strong>Overview: </strong>

In part II of this assignment, you will start with the solution to part I and create a tool that allows drawing a process tree consisting of all processes in your Linux system. The nodes of this tree represent processes and each (directed) edge between two processes represents a parent/child relationship.

In Unix, every process that is created, is created by a so-called “parent process”. A parent process is a process that created one (or more) child processes. Each process in Unix has a unique parent process. The only process that does not have a parent process is the root of the tree. Some UNIX processes introduce a process with PID 0 (e.g., SOLARIS sched process), but this is not a real process like all the other processes. Specifically, there is no process with PID 0 in Linux (hence, there is also no directory “/proc/0”). In Linux, there are processes, however, that list a process with the parent process ID (PPID) of 0 (example: PID 1 and PID 2).

For the sake of this project, we assume that this “artificial” or “dummy” process with the PID 0 exists and represents the root of the Linux process tree.

The process three that your program should draw should be created using standard I/O print functions (such as “printf()” in C or “cout &lt;&lt; …” in C++.

Consider the following simple example process tree, consisting of 8 processes + 1 dummy root process:




<table width="228">

 <tbody>

  <tr>

   <td width="84">ProcessName</td>

   <td width="72">PID</td>

   <td width="72">PPID</td>

  </tr>

  <tr>

   <td width="84">Dummy</td>

   <td width="72">0</td>

   <td width="72">N/A</td>

  </tr>

  <tr>

   <td width="84">A</td>

   <td width="72">1</td>

   <td width="72">0</td>

  </tr>

  <tr>

   <td width="84">B</td>

   <td width="72">2</td>

   <td width="72">0</td>

  </tr>

  <tr>

   <td width="84">C</td>

   <td width="72">3</td>

   <td width="72">1</td>

  </tr>

  <tr>

   <td width="84">D</td>

   <td width="72">4</td>

   <td width="72">1</td>

  </tr>

  <tr>

   <td width="84">E</td>

   <td width="72">5</td>

   <td width="72">1</td>

  </tr>

  <tr>

   <td width="84">F</td>

   <td width="72">6</td>

   <td width="72">4</td>

  </tr>

  <tr>

   <td width="84">G</td>

   <td width="72">7</td>

   <td width="72">4</td>

  </tr>

  <tr>

   <td width="84">H</td>

   <td width="72">8</td>

   <td width="72">2</td>

  </tr>

 </tbody>

</table>




The above parent/child relationships give rise to the following process tree graph (drawn in traditional fashion):




Note that in your solution to part I of this assignment, you already extracted all the information corresponding to the above table or proesses (namely, the PID, command name, PPID, etc.) for all process in Linux system.

Your tool should create a graph using ASCII characters. Edges are visualized using “-“ and “|” characters. Processes are represented by their PID followed by the command name (in parenthesis).

The output for the above example should look as follows:




—-&gt;    0(dummy)             —-&gt;    1(A)  —-&gt;    3(C)

|              |

|              —-&gt;    4(D)  —-&gt;    6(F)                |              |              |

|              |              —-&gt;    7(G)

|              |                |              —-&gt;    5(E)                |

—-&gt;    2(B)  —-&gt;    8(H)

A complete output file for a real Linux system (pu1.cs.ohio.edu) will be provided on blackboard.







<strong>Specific Requirements: </strong>

<ul>

 <li>Implement the above described tool “4420_proctool_2”. The tool has no required command argument and should display the above described process tree.</li>

 <li>You have to implement the tool either in C or C++. You are only allowed to obtain the process information via the proc filesystem (as explained in part 1 above) for this part of the assignment. To do that, you should only use functions to open and read files and directories.  Do not use any other functions (example: system(), getrusage(), etc.). Of course, this restriction is only for extracting process information.</li>

 <li>You will need to submit the source file(s), header files (if necessary), and you WILL NEED TO SUBMIT A <strong><u>MAKEFILE</u></strong>. I will grade the project on “pu1.cs.ohio.edu”. I will type “make” to build the tool. If “make” fails (due to an error), the GRADE FOR THIS PART OF ASSIGNMENT 1 WILL BE AN “F”!</li>

 <li>You will need to submit the program from “<strong><u>cs.ohio.edu</u></strong>” (NOTE: THIS IS IMPORTANT. THE SUBMISSION WILL NOT WORK FROM ANY OTHER (LAB)MACHINE OR SERVER!). The submission is simple: “cd” into your project folder and type the following command (exactly as is shown here):</li>

</ul>

“<strong>bash&gt; /home/drews/442submit 2</strong>”. Should you need to re-submit your project (due to changes), you will need to add an override switch “-f” to the submission: “<strong>bash&gt; </strong>

<strong>/home/drews/442submit -f 2</strong>”.

<ul>

 <li><strong>Do not accidentally overwrite your submission for part 1 of this assignment. </strong></li>

</ul>

<strong> </strong>

<strong>Additional Help and Resources: </strong>

<ul>

 <li>As stated above, start with the code for part 1 of the assignment</li>

 <li>Create some sort of array (or other suitable data structure) to store the information about all processes (including command name, PID, PPID).</li>

 <li>I would suggest you manually add a dummy process with PID 0 to this data structure</li>

 <li>I recommend you implement the function to display the process tree as a <strong>recursive function</strong>. <strong>Note: this function is not really that difficult but may take a little bit of trying to get done right. </strong></li>

</ul>

<strong>Hint: I implemented this function using around 15-20 lines of C code.</strong>

<ul>

 <li>I recommend using the C function “printf()” to created formatted output o I also suggest you make use of the tab character ‘t’ to format the output of the tree o One weird thing about the tab character is that it is interpreted by your terminal, and thus depends on your terminal configuration. You may end up having to change the tab width manually for this project. This should be done “outside” of your program by using the Unix command “tabs” (see man tabs for details). For example typing: “tabs 20” sets the tab width to 20 characters.</li>

 <li>You can change the tabs programmatically (i.e., from within your program, as opposed to by a separate shell command) by adding the following function call somewhere right at the beginning of main():</li>

</ul>

system(“tabs 25”);




If you “mess up” your tab width and wishto reset it to default settings, use the call




system(“tabs -8”);

<ul>

 <li>One weird thing about tabs is that each tab is simply a ‘t’ ASCII code that is sent to your terminal. The terminal then interprets it based on the terminal configuration. This also means that when you redirect the output of your program to a file (e.g., “./4420_proctool_2 &gt; outfile”), and later open the redirected file, the format of the output may vary. Note that this is normal and expected.</li>

 <li>On blackboard, I will provide you with a test directory of (pseudo) proc files that correspond to the above example (the one with processes “A” through “H”). You can use this directory to test your program.</li>

</ul>