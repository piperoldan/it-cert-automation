## Example 8: Code that crashes with a segmentation fault

The code in this directory is a very simple C program.  This program crashes
with a segmentation fault when executed.

To make the program work in your mac, you have to compile the code: 
In the Terminal, navigate to the directory where you saved the file and run the following command. The -o flag specifies the output file name, which will be the executable program.
```
gcc example.c -o example
```

You can build and run the program with:

```
make
./example 
```

To get a coredump, run:
```
ulimit -c unlimited
./example 
```

Then debug the failure with:
```
gdb -c core example
```
## Notes for Mac Users:
MacOS handles core dumps differently than Linux. 
The **ulimit -c unlimited** command works, but the problem likely lies with where macOS places the core dump file and what it names it.

Where macOS Stores Core Dumps
Unlike Linux, which often creates a core file in the current working directory, macOS stores core dump files in a system-wide directory, typically /cores. The file is also named differently, using the process ID (PID) to prevent conflicts.

So, instead of ls -l core, look for a file named core.<PID> in the /cores directory. 
For example, if your program's PID was 12345, the file would be named core.12345.

Troubleshooting Steps
Check the /cores directory: The first thing to do is check the /cores directory for the file.

Bash
ls -l /cores
You might not see this directory at all, or it may exist but you don't have permission to write to it.

Verify permissions: The /cores directory often requires elevated permissions to write to it. If you don't have the correct permissions, the core file won't be created. You can check the permissions with ls -ld /cores. To fix this, you may need to grant write permissions to all users:

Bash
sudo chmod 1777 /cores
The 1777 permission set ensures that all users can write to the directory, but only the file's owner can delete it.

Check the core file pattern: You can use the sysctl command to see where macOS is configured to write core dump files.

Bash
sysctl kern.corefile
The output should show a path like /cores/core.%P. The %P is a placeholder that is replaced with the process ID of the crashed program.

Try an alternative location: If you're still having trouble, you can tell the system to write core files to the current directory. To do this, you can temporarily change the kern.corefile setting:

Bash
sudo sysctl kern.corefile=./core.%P
Now, when you run your segfault_maker program, the core file should appear in your current directory, named something like core.12345.

Keep in mind that these changes are often temporary and reset after a reboot.

To DEBUG THE FILE in the MacOS, instead of GDB (Suggested by Google Course)

LLDB is already on the system, it's the easiest and most direct tool to use. It works similarly to GDB.

To debug your segfault_maker program with LLDB, open a new Terminal window and run:

Bash
lldb ./example

This will open the LLDB interactive console. Then, type run and press Enter. LLDB will run the program and tell you exactly where the segmentation fault occurred.


