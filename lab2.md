**Task 1: Manipulating Environment Variables**

In this task, we study the commands that can be used to set and unset environment variables. We are using Bash in the seed account. The default shell that a user uses is set in the `/etc/passwd` file (the last field of each entry). You can change this to another shell program using the command `chsh` (please do not do it for this lab). Please do the following tasks:

* Use `printenv` or `env` command to print out the environment variables. If you are interested in some particular environment variables, such as `PWD`, you can use `printenv PWD` or `env | grep PWD`.

![Task 1](images/lab2/t1a)

* Use `export` and `unset` to set or unset environment variables. It should be noted that these two commands are not separate programs; they are two of the Bash’s internal commands (you will not be able to find them outside of Bash).

![Task 1](images/lab2/t1b)

**Task 2: Passing Environment Variables from Parent Process to Child Process**

In this task, we study how a child process gets its environment variables from its parent. In Unix, `fork()` creates a new process by duplicating the calling process. The new process, referred to as the child, is an exact duplicate of the calling process, referred to as the parent; however, several things are not inherited by the child (please see the manual of `fork()` by typing the following command: `man fork`. In this task, we would like to know whether the parent’s environment variables are inherited by the child process or not.

**Step 1.** Please compile and run the following program, and describe your observation. The program can be found in the Labsetup folder; it can be compiled using "`gcc myprintenv.c`", which will generate a binary called `a.out`. Let’s run it and save the output into a file using "`a.out > file`".

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

extern char **environ;
void printenv()
{
    int i = 0;
    while (environ[i] != NULL) {
        printf("%s\n", environ[i]);
        i++;
    }
}

void main()
{
    pid_t childPid;
    switch(childPid = fork()) {
        case 0: /* child process */
            printenv();
            exit(0);
        default: /* parent process */
            //printenv();
            exit(0);
    }
}

```
![output](images/lab2/t2s1b)

The program creates a child process using `fork()`, and then prints out all the environment variables of that process to stdout. In this case, the environment variables are printed to the file `file`

**Step 2.** Now comment out the `printenv()` statement in the child process case (Line ①), and uncomment the `printenv()` statement in the parent process case (Line ②). Compile and run the code again, and describe your observation. Save the output in another file.

![step 2](images/lab2/t2s2)

**Step 3.** Compare the difference of these two files using the `diff` command. Please draw your conclusion.

![step 3](images/lab2/t2s3)

According to the `diff` command, the output of the program before and after modification is identical. This means that the environment variables of the parent and child process are the same, which proves that the child process inherits the environment variables of the parent process upon being created using the `fork()` system call.

**Task 3:** Environment Variables and ``execve()``

In this task, we study how environment variables are affected when a new program is executed via ``execve()``. The function ``execve()`` calls a system call to load a new command and execute it; this function never re- turns. No new process is created; instead, the calling process’s text, data, bss, and stack are overwritten by that of the program loaded. Essentially, ``execve()`` runs the new program inside the calling process. We are interested in what happens to the environment variables; are they automatically inherited by the new program?

**Step 1.** Please compile and run the following program, and describe your observation. This program simply executes a program called `/usr/bin/env`, which prints out the environment variables of the current process.

```c
#include <unistd.h>

extern char **environ;
int main()
{
    char *argv[2];

    argv[0] = "/usr/bin/env";
    argv[1] = NULL;
    execve("/usr/bin/env", argv, NULL);

    return 0;
}
```

![output](images/lab2/t3s1b)

It appears when starting a new process using ``execve()``, the new process does not inherit the environment variables of the calling process, and has no environment variables by default.

**Step 2.** Change the invocation of ``execve()`` in Line ① to the following; describe your observation.

```c
    execve("/usr/bin/env", argv, environ);
```

![output](images/lab2/t3s2a)

![output](images/lab2/t3s2b)

It appears that the new process inherited the environment variables of the calling process.

**Step 3.** Please draw your conclusion regarding how the new program gets its environment variables.

The new program gets its environment variables from the third argument provided to the ``execve()`` call.

**Task 4:** Environment Variables and `system()`

In this task, we study how environment variables are affected when a new program is executed via the `system()` function. This function is used to execute a command, but unlike `execve()`, which di- rectly executes a command, `system()` actually executes "`/bin/sh -c command`", i.e., it executes `/bin/sh`, and asks the shell to execute the command. If you look at the implementation of the `system()` function, you will see that it uses `execl()` to execute `/bin/sh`; `execl()` calls `execve()`, passing to it the environment variables array. Therefore, using `system()`, the environment variables of the calling process is passed to the new program `/bin/sh`. Please compile and run the following program to verify this.

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    system("/usr/bin/env");
    return 0;
}
```

![output](images/lab2/t4)
