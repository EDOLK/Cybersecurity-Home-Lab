
1. When you first ssh and open your virtual machine, you should be told how many updates that can be applied. Enter the command to list the updates available.

`sudo apt list --upgradable`

![Question 1](images/lab1/q1)

2. Update and Upgrade your system.

`sudo apt update && sudo apt upgrade -y`

![Question 2](images/lab1/q2)

3. Reboot your system.

`shutdown -r now`

**User Tasks:**

4. Change the current user to root using the command sudo su root. What does the prompt look like?

`sudo su root`

![Question 4](images/lab1/q4)

5. Create a new user with the name bobby using the command useradd. Next, create another user with the name sally using the command adduser. What is the difference between the two?

```bash
useradd bobby
adduser sally
```

The useradd command created a basic user without a password or home directory, while the adduser command created a home directory, and was interactive, asking me for input on the password and other information.

![Question 5](images/lab1/q5)

6. Change the current user to sally. What does the prompt look like now?

`su sally`

![Question 6](images/lab1/q6)

7. While you’re logged in as sally still, try to create a new user with the name earl. What happens? Why?

`useradd earl`

Permission is denied, as creating a new user means modifying the passwd file, which only the root user has editing permissions for.

![Question 7](images/lab1/q7)

8. Enter exit until you are the original user, ubuntu, again. Delete the user earl. I didn’t show you the command, but Google it! “Googling” skills are a great skill in CS; It’s impossible to know everything.

`userdel earl`

![Question 8](images/lab1/q8)

9. Change the password of sally to something you can remember using sudo passwd sally.

`sudo passwd sally`

![Question 9](images/lab1/q9)

10. For the rest of the tasks, use the ubuntu user. Even though it’s easier to complete tasks/commands, why is it bad practice to stay logged in as root?

It's bad practice to stay logged in as root because root has permissions to modify almost any file on the machine. If you use a command you're unfamiliar with, it might make irreversible changes to the system.

11. Enter the command to see what your user id is.

`id`

![Question 11](images/lab1/q11)

**Group Tasks:**

12. What groups does ubuntu belong to?

`groups`

![Question 12](images/lab1/q12)

13. Give sally the ability to execute sudo commands. Next, try to create a new user while logged in as sally.

```bash
sudo usermod -aG sudo sally
sudo su sally
sudo useradd anotheruser
```

![Question 13](images/lab1/q13)

14. Create a new group called cybersec.

`sudo groupadd cybersec`

15. Add sally to the group, cybersec

`sudo usermod -aG cybersec sally`

16. Check to see which groups sally belongs.

`groups sally`

![Question 16](images/lab1/q16)

**Permissions and Access Control Lists**

17. Create a new directory called lab1. Enter the command to find the permissions of the directory. Who is the owner and group owner of this directory? What permissions does the owner, group and other have?

```bash
mkdir lab1
ls -ld lab1
```

![Question 17](images/lab1/q17)

The owner of this directory is olkhovskye, and the group owner is the olkhovskye group. The owner and group has permissions to read, write, and execute files in the directory. Other has permissions to read and execute files from the directory.

18. Change your directory to lab1. Create a new bash file called, helloWorld. When ran, your program should just print “Hello World!”. (Don’t forget to make your bash file executable).

```bash
cd lab1
touch helloWorld.sh
vi helloWorld.sh
chmod +x helloWorld.sh
./helloWorld.sh
```

![Question 18](images/lab1/q18)

19. Enter the command ls -la helloWorld. What are the reading, writing, and executing permissions for the owner, group and other?

`ls -la helloWorld.sh`

`-rwxrwxr-x 1 olkhovskye olkhovskye 40 Oct  3 20:12 helloWorld.sh`

Owner and group has read, write, and execute permissions. Other only has execute permissions.

a. Change the permissions so the group also has w and x permissions.

`chmod g+wx helloWorld.sh`

20. Use the getfacl command to view the ACL of the file.

`getfacl helloWorld.sh`

```
# file: helloWorld.sh
# owner: olkhovskye
# group: olkhovskye
user::rwx
group::rwx
other::r-x
```

21. Using the setfacl command, allow the user, sally, the ability to read and write to the file.

`setfacl -m sally:rw helloWorld.sh`

