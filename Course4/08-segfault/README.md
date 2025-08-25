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

