 Users and Groups

 In any traditional operating system, there are users and groups. They exist solely for access and permissions. When running a process, it will run as the owner of that process whether that is Jane or Bob. File access and ownership is also permission dependent. You wouldn't want Jane to see Bob's documents and vice versa.

Each user has their own home directory where their user specific files get stored, this is usually located in /home/username, but can vary in different distributions.

The system uses user ids (UID) to manage users, usernames are the friendly way to associate users with identification, but the system identifies users by their UID. The system also uses groups to manage permissions, groups are just sets of users with permission set by that group, they are identified by the system with their group ID (GID).

In Linux, you'll have users in addition to the normal humans that use the system. Sometimes these users are system daemons that continuously run processes to keep the system functioning. One of the most important users is root or superuser, root is the most powerful user on the system, root can access any file and start and terminate any process. For that reason, it can be dangerous to operate as root all the time, you could potentially remove system critical files. Luckily, if root access is needed and a user has root access, they can run a command as root instead with the sudo command. The sudo command (superuser do) is used to run a command with root access, we'll go more in depth on how a user receives root access in a later lesson.

Go ahead and try to view a protected file like /etc/shadow:

$ cat /etc/shadow
Notice how you get a permission denied error, look at the permissions with:

$ ls -la /etc/shadow


-rw-r----- 1 root shadow 1134 Dec 1 11:45 /etc/shadow


We haven't gone through permissions yet, but what's happening here is that root is the owner of the file and you'll need root access or be part of the shadow group to read the contents. Now run the command with sudo:

$ sudo cat /etc/shadow
Now you'll be able to see the contents of the file!

root

We've looked at one way to get superuser access using the sudo command. You can also run commands as the superuser with the su command. This command will "substitute users" and open a root shell if no username is specified. You can use this command to substitute to any user as long as you know the password.

$ su
There are some downsides to using this method: it's much easier to make a critical mistake running everything in root, you won't have records of the commands you use to change system configurations, etc. Basically, if you need to run commands as the superuser, just stick to sudo.

Now that you know what commands to run as the superuser, the question is how do you know who has access to do that? The system doesn't let every single Joe Schmoe run commands as the superuser, so how does it know? There is a file called the /etc/sudoers file, this file lists users who can run sudo. You can edit this file with the visudo command.

 /etc/passwd
 Remember that usernames aren't really identifications for users. The system uses a user ID (UID) to identify a user. To find out what users are mapped to what ID, look at the /etc/passwd file.

$ cat /etc/passwd
This file shows you a list of users and detailed information about them. For example, the first line in this file most likely looks like this:

root:x:0:0:root:/root:/bin/bash
1:2:3:4:5:6:7

Each line displays user information for one user, most commonly you'll see the root user as the first line. There are many fields separated by colons that tell you additional information about the user, let's look at them all:

1. Username
2. User's password - the password is not really stored in this file, it's usually stored in the /etc/shadow file. We'll discuss more in the next lesson about /etc/shadow, but for now, know that it contains encrypted user passwords. You can see many different symbols that are in this field, if you see an "x" that means the password is stored in the /etc/shadow file, a "*" means the user doesn't have login access and if there is a blank field that means the user doesn't have a password.
3. The user ID - as you can see root has the UID of 0
4. The group ID
5. GECOS field - This is used to generally leave comments about the user or account such as their real name or phone number, it is comma delimited.
6. User's home directory
7. User's shell - you'll probably see a lot of user's defaulting to bash for their shell


Normally in a user's setting page, you would expect you see just human users. However, you'll notice /etc/passwd contains other users. Remember that users are really only on the system to run processes with different permissions. Sometimes we want to run processes with pre-determined permissions. For example, the daemon user is used for daemon processes.

Also should note that you can edit the /etc/passwd file by hand if you want to add users and modify information with the vipw tool, however things like these are best left to the tools we will discuss in a later lesson such as useradd and userdel.

If a user doesn't have login access how is that denoted in /etc/passwd? By a *

 /etc/shadow
 The /etc/shadow file is used to store information about user authentication. It requires superuser read permissions.

$ sudo cat /etc/shadow


root:MyEPTEa$6Nonsense:15000:0:99999:7:::
1:2:3:4:5:6:7:8:9


You'll notice that it looks very similar to the contents of /etc/passwd, however in the password field you'll see an encrypted password. The fields are separated by colons as followed:

1. Username
2. Encrypted password
3. Date of last password changed - expressed as the number of days since Jan 1, 1970. If there is a 0 that means the user should change their password the next time they login
4. Minimum password age - Days that a user will have to wait before being able to change their password again
5. Maximum password age - Maximum number of days before a user has to change their password
6. Password warning period - Number of days before a password is going to expire
7. Password inactivity period - Number of days after a password has expired to allow login with their password
8. Account expiration date - date that user will not be able to login
9. Reserved field for future use
In most distributions today, user authentication doesn't rely on just the /etc/shadow file, there are other mechanisms in place such as PAM (Pluggable Authentication Modules) that replace authentication.

/etc/group
Another file that is used in user management is the /etc/group file. This file allows for different groups with different permissions.

$ cat /etc/group


root:*:0:pete


Very similar to the /etc/password field, the /etc/group fields are as follows:

1. Group name
2. Group password - there isn't a need to set a group password, using an elevated privilege like sudo is standard. A "*" will be put in place as the default value.
3. Group ID (GID)
4. List of users - you can manually specify users you want in a specific group

$ groups
Print group memberships for each USERNAME or, if no USERNAME is specified, for
the current process 


User Management Tools
Most enterprise environments are using management systems to manage users, accounts and passwords. However, on a single machine computer there are useful commands to run to manage users.

Adding Users

You can use the adduser or the useradd command. The adduser command contains more helpful features such as making a home directory and more. There are configuration files for adding new users that can be customized depending on what you want to allocate to a default user.

$ sudo useradd bob
You'll see that the above command creates an entry in /etc/passwd for bob, sets up default groups and adds an entry to the /etc/shadow file.

Removing Users

To remove a user, you can use the userdel command.

$ sudo userdel bob
This basically does its best to undo the file changes by useradd.

Changing Passwords

$ passwd bob
This will allow you to change the password of yourself or another user (if you are root).