# Laboratory 3

This laboratory consists of designing a C program to serve as a shell interface that accepts user commands and then executes each command in a separate process.

A shell interface gives the user a prompt, after which the next command is entered. The example below illustrates the prompt osh> and the user’s next command: cat prog.c. (This command displays the file prog.c on the terminal using the UNIX cat command.)

```shell
osh> cat prog.c
```

One technique for implementing a shell interface is to have the parent process first read what the user enters on the command line (in this case, cat prog.c), and then create a separate child process that performs the command. Unless otherwise specified, the parent process waits for the child to exit before continuing. This is similar in functionality to the new process creation illustrated in the slides of the class. However, UNIX shells typically also allow the child process to run in the background, or concurrently. To accomplish this, we add an ampersand (&) at the end of the command. Thus, if we rewrite the above command as:

```shell
osh> cat prog.c &
```

the parent and child processes will run concurrently.

The separate child process is created using the `fork()` system call, and the user's command is executed using one of the system calls in th `exec()` family.

You can use the following code as a starting point:

```c
#include <stdio.h>
#include <unistd.h>

#define MAX LINE 80 /* The maximum length command */

int main(void) {
    char *args[MAXLINE / 2 + 1]; /* command line arguments */
    int should_run = 1; /* flag to determine when to exit program */
    while (should)_run) {
        printf("osh>");
        fflush(stdout);
        /**
        * After reading user input, the steps are:
        * (1) fork a child process using fork()
        * (2) the child process will invoke execvp()
        * (3) if command included &, parent will invoke wait() */
    }
    
    return 0;
}
```

The `main()` function presents the prompt `osh->` and outlines the steps to be taken after input from the user has been read. The `main()` function continually loops as long as should run equals 1; when the user enters exit at the prompt, your program will set should run to 0 and terminate.

## Exercise 1

The first task is to modify the `main()` function so that a child process is forked and executes the command specified by the user. This will require parsing what the user has entered into separate tokens and storing the tokens in an array of character strings. For example, if the user enters the command `ps -ael` at the osh> prompt, the values stored in the args array are:
`args[0] = "ps" args[1] = "-ael" args[2] = NULL`.
This args array will be passed to the execvp() function, which has the following prototype:
```c
execvp(char *command, char *params[]);
```

Here, command represents the command to be performed and params stores the parameters to this command. For this, the execvp() function should be invoked as `execvp(args[0], args)`. Be sure to check whether the user included an & to determine whether or not the parent process is to wait for the child to exit.

## Exercise 2

The next task is to modify the shell interface program so that it provides a history feature that allows the user to access the most recently entered commands. The user will be able to access up to 10 commands by using the feature. The commands will be consecutively numbered starting at 1, and the numbering will continue past 10. For example, if the user has entered 35 commands, the 10 most recent commands will be numbered 26 to 35.

The user will be able to list the command history by entering the command `history` at the osh> prompt. As an example, assume that the history consists of the commands (from most to least recent): `ps, ls -l, top, cal, who, date `. 

The command history will output:
```shell
osh> 1 date
2 who
3 cal
4 top
5 ls -l
6 ps
```
