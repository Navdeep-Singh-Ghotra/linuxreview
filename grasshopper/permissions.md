File Permissions

$ ls -l Desktop/

drwxr-xr-x 2 pete penguins 4096 Dec 1 11:45 .


There are four parts to a file's permissions. 
The first part is the filetype, which is denoted by the first character in the permissions, in our case since we are looking at a directory it shows d for the filetype. Most commonly you will see a - for a regular file.

The next three parts of the file mode are the actual permissions. The permissions are grouped into 3 bits each. 
The first 3 bits are user permissions, then group permissions and then other permissions. 
I've added the pipe to make it easier to differentiate.

d | rwx | r-x | r-x 
Each character represent a different permission:

r: readable
w: writable
x: executable (basically an executable program)
-: empty
So in the above example, we see that the user pete has read, write and execute permissions on the file. The group penguins has read and execute permissions. And finally, the other users (everyone else) has read and execute permissions.

Modifying Permissions
Changing permissions can easily be done with the chmod command.
Adding permission bit on a file

$ chmod u+x myfile
The above command reads like this: change permission on myfile by adding executable permission bit on the user set.
Removing permission bit on a file

$ chmod u-x myfile
Adding multiple permission bits on a file

$ chmod ug+w

There is another way to change permissions using numerical format. This method allows you to change permissions all at once. Instead of using r, w, or x to represent permissions, you'll use a numerical representation for a single permission set. So no need to specify the group with g or the user with u.

The numerical representations are seen below:

4: read permission
2: write permission
1: execute permission
Let's look at an example:

$ chmod 755 myfile
Can you guess what permissions we are giving this file? Let's break this down, so now 755 covers the permissions for all sets. The first number (7) represents user permissions, the second number (5) represents group permissions and the last 5 represents other permissions.

Ownership Permissions
In addition to modifying permissions on files, you can also modify the group and user ownership of the file as well.

Modify user ownership

$ sudo chown patty myfile
This command will set the owner of myfile to patty.

Modify group ownership

$ sudo chgrp whales myfile
This command will set the group of myfile to whales.

Modify both user and group ownership at the same time
If you add a colon and groupname after the user you can set both the user and group at the same time.

$ sudo chown patty:whales myfile

Umask
Every file that gets created comes with a default set of permissions. If you ever wanted to change that default set of permissions, you can do so with the umask command. This command takes the 3 bit permission set we see in numerical permissions.

Instead of adding these permissions though, umask takes away these permissions.

$ umask 021
In the above example, we are stating that we want the default permissions of new files to allow users access to everything, but for groups we want to take away their write permission and for others we want to take away their executable permission. The default umask on most distributions is 022, meaning all user access, but no write access for group and other users.

When you run the umask command it will give that default set of permissions on any new file you make. However, if you want it to persist you'll have to modify your startup file (.profile), but we'll discuss that in a later lesson.


Setuid
The Set User ID (SUID) allows a user to run a program as the owner of the program file rather than as themselves.
$ ls -l /usr/bin/passwd


-rwsr-xr-x 1 root root 47032 Dec 1 11:45 /usr/bin/passwd
You'll notice a new permission bit here s. This permission bit is the SUID, when a file has this permission set, it allows the users who launched the program to get the file owner's permission as well as execution permission, in this case root. So essentially while a user is running the password command, they are running as root.

Modifying SUID

Just like regular permissions there are two ways to modify SUID permissions.

Symbolic way:

$ sudo chmod u+s myfile
Numerical way:

 sudo chmod 4755 myfile
As you can see the SUID is denoted by a 4 and pre-pended to the permission set. You may see the SUID denoted as a capital S this means that it still does the same thing, but it does not have execute permissions.

What number represents the SUID? 4

Setgid
Similar to the set user ID permission bit, there is a set group ID (SGID) permission bit. This bit allows a program to run as if it was a member of that group.

Let's look at one example:

$ ls -l /usr/bin/wall

-rwxr-sr-x 1 root tty 19024 Dec 14 11:45 /usr/bin/wall

We can see now that the permission bit is in the group permission set.

Modifying SGID

$ sudo chmod g+s myfile

$ sudo chmod 2555 myfile

The numerical representation for SGID is 2.

Process Permissions
Let's segue into process permissions for a bit, remember how I told you that when you run the passwd command with the SUID permission bit enabled you will run the program as root? That is true, however does that mean since you are temporarily root you can modify other user's passwords? Nope fortunately not!

This is because of the many UIDs that Linux implements. There are three UIDS associated with every process:

When you launch a process, it runs with the same permissions as the user or group that ran it, this is known as an effective user ID. This UID is used to grant access rights to a process. So naturally if Bob ran the touch command, the process would run as him and any files he created would be under his ownership.

There is another UID, called the real user ID this is the ID of the user that launched the process. These are used to track down who the user who launched the process is.

One last UID is the saved user ID, this allows a process to switch between the effective UID and real UID, vice versa. This is useful because we don't want our process to run with elevated privileges all the time, it's just good practice to use special privileges at specific times.

Now let's piece these all together by looking at the passwd command once more.

When running the passwd command, your effective UID is your user ID, let's say its 500 for now. Oh but wait, remember the passwd command has the SUID permission enabled. So when you run it, your effective UID is now 0 (0 is the UID of root). Now this program can access files as root.

Let's say you get a little taste of power and you want to modify Sally's password, Sally has a UID of 600. Well you'll be out of luck, fortunately the process also has your real UID in this case 500. It knows that your UID is 500 and therefore you can't modify the password of UID of 600. (This of course is always bypassed if you are a superuser on a machine and can control and change everything).

Since you ran passwd, it will start the process off using your real UID, and it will save the UID of the owner of the file (effective UID), so you can switch between the two. No need to modify all files with root access if it's not required.

Most of the time the real UID and the effective UID are the same, but in such cases as the passwd command they will change.

The Sticky Bit
One last special permission bit I want to talk about is the sticky bit.

This permission bit, "sticks a file/directory" this means that only the owner or the root user can delete or modify the file. This is very useful for shared directories. Take a look at the example below:

$ ls -ld /tmp

drwxrwxrwxt 6 root root 4096 Dec 15 11:45 /tmp

You'll see a special permission bit at the end here t, this means everyone can add files, write files, modify files in the /tmp directory, but only root can delete the /tmp directory.

Modify sticky bit

$ sudo chmod +t mydir


$ sudo chmod 1755 mydir

The numerical representation for the sticky bit is 1