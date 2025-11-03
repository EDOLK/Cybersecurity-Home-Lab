**Task 1: Manipulating Environment Variables**

In this task, we study the commands that can be used to set and unset environment variables. We are using Bash in the seed account. The default shell that a user uses is set in the `/etc/passwd` file (the last field of each entry). You can change this to another shell program using the command `chsh` (please do not do it for this lab). Please do the following tasks:

* Use `printenv` or `env` command to print out the environment variables. If you are interested in some particular environment variables, such as `PWD`, you can use `printenv PWD` or `env | grep PWD`.

![Task 1](images/lab2/t1a)

* Use `export` and `unset` to set or unset environment variables. It should be noted that these two commands are not separate programs; they are two of the Bash’s internal commands (you will not be able to find them outside of Bash).

![Task 1](images/lab2/t1b)

**Task 2: Passing Environment Variables from Parent Process to Child Process**

In this task, we study how a child process gets its environment variables from its parent. In Unix, `fork()` creates a new process by duplicating the calling process. The new process, referred to as the child, is an exact duplicate of the calling process, referred to as the parent; however, several things are not inherited by the child (please see the manual of `fork()` by typing the following command: `man fork`. In this task, we would like to know whether the parent’s environment variables are inherited by the child process or not.

**Step 1.** Please compile and run the following program, and describe your observation. The program can be found in the Labsetup folder; it can be compiled using "`gcc myprintenv.c`", which will generate a binary called `a.out`. Let’s run it and save the output into a file using "`a.out > file`".

![myprintenv.c](images/lab2/t2s1a)

![output](images/lab2/t2s1b)
