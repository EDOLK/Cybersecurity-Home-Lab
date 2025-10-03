
1. When you first ssh and open your virtual machine, you should be told how many updates that can be applied. Enter the command to list the updates available.

`sudo apt list --upgradable`

![Question 1](q1)

2. Update and Upgrade your system.

`sudo apt update && sudo apt upgrade -y`

![Question 2](q2)

3. Reboot your system.

`shutdown -r now`

**User Tasks:**

4. Change the current user to root using the command sudo su root. What does the prompt look like?

`sudo su root`

![Question 4](q4)

5. Create a new user with the name bobby using the command useradd. Next, create another user with the name sally using the command adduser. What is the difference between the two?

```bash
useradd bobby
adduser sally
```

The useradd command created a basic user without a password or home directory, while the adduser command created a home directory, and was interactive, asking me for input on the password and other information.

![Question 5](q5)

6. Change the current user to sally. What does the prompt look like now?

`su sally`

![Question 6](q6)

7. While you’re logged in as sally still, try to create a new user with the name earl. What happens? Why?

`useradd earl`

permission is denied, as creating a new user means modifying the passwd file, which only the root user has editing permissions for.

![Question 7](q7)

8. Enter exit until you are the original user, ubuntu, again. Delete the user earl. I didn’t show you the command, but Google it! “Googling” skills are a great skill in CS; It’s impossible to know everything.

`userdel earl`

![Question 8](q8)

9. Change the password of sally to something you can remember using sudo passwd sally.

`sudo passwd sally`

![Question 9](q9)

10. For the rest of the tasks, use the ubuntu user. Even though it’s easier to complete tasks/commands, why is it bad practice to stay logged in as root?

It's bad practice to stay logged in as root because root has permissions to modify almost any file on the machine. If you use a command you're unfamiliar with, it might make irreversible changes to the system.

11. Enter the command to see what your user id is.

`id`

![Question 11](q11)

**Group Tasks:**

12. What groups does ubuntu belong to?

`groups`

![Question 12](q12)

13. Give sally the ability to execute sudo commands. Next, try to create a new user while logged in as sally.

14. Create a new group called cybersec

15. Add sally to the group, cybersec

16. Check to see which groups sally belongs.

