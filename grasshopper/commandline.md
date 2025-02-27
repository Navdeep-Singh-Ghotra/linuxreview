The shell is basically a program that takes your commands from the keyboard and sends them to the operating system to perform.
shell program bash (Bourne Again shell)
username@hostname:current_directory$
$ is for a normal user using Bash

The echo command just prints out the text arguments to the display.
$ echo Hello
$ date
$ whoami 

pwd (Print Working Directory)
$ pwd

cd (Change Directory)
Absolute path: This is the path from the root directory. The root is the head honcho. The root directory is commonly shown as a slash. Every time your path starts with / it means you are starting from the root directory. For example, /home/pete/Desktop.
Relative path: This is the path from where you are currently in filesystem. If I was in location /home/pete/Documents and wanted to get to a directory inside Documents called taxes, I don’t have to specify the whole path from root like /home/pete/Documents/taxes, I can just go to taxes/ instead.

. (current directory). This is the directory you are currently in.
.. (parent directory). Takes you to the directory above your current.
~ (home directory). This directory defaults to your “home directory”. Such as /home/pete.
- (previous directory). This will take you to the previous directory you were just at.

Run the cd command without any flags, it take you to home directory

 ls (List Directories)
  The ls command will list directories and files in the current directory by default, however you can specify which path you want to list the directories of.

$ ls
$ ls /home/pete
Include hidden files
$ ls -a

-l for long, this shows a detailed list of files in a long format. This will show you detailed information, starting from the left: file permissions, number of links, owner name, owner group, file size, timestamp of last modification, and file/directory name.
$ ls -l

ls -R: recursively list directory contents
ls -r: reverse order while sorting
ls -t: sort by modification time, newest first

touch
Touch allows you to the create new empty files. Touch is also used to change timestamps on existing files and directories.
$ touch mysuperduperfile

file
in Linux, filenames aren’t required to represent the contents of the file. To find out what kind of file a file is, you can use the file command.
$ file abc.html 
abc.html: HTML document, ASCII text


cat
cat command, short for concatenate, it not only displays file contents but it can combine multiple files and show you the output of them. It’s not great for viewing large files and it’s only meant for short content. 

less
If you are viewing text files larger than a simple output, less is more. The text is displayed in a paged manner, so you can navigate through a text file page by page.
q - Used to quit out of less and go back to your shell.
Page up, Page down, Up and Down - Navigate using the arrow keys and page keys.
g - Moves to beginning of the text file.
G - Moves to the end of the text file.
/search - You can search for specific text inside the text document. Prefacing the words you want to search with /
h - If you need a little help about how to use less while you’re in less, use help.

history
find and run a command you used previously without actually typing
$ history

 run the previous command without typing it again? Use !!. 
 history shortcut is ctrl-R, this is the reverse search command, if you hit ctrl-R and you start typing parts of the command you want it will show you matches and you can just navigate through them by hitting the ctrl-R key again. Once you found the command you want to use again, just hit the Enter key.

cp (Copy)
mycoolfile is the file you want to copy and /home/pete/Documents/cooldocs is where you are copying the file to
$ cp mycoolfile /home/pete/Documents/cooldocs

You can copy multiple files and directories as well as use wildcards. A wildcard is a character that can be substituted for a pattern based selection, giving you more flexibility with searches. You can use wildcards in every command for more flexibility.

* the wildcard of wildcards, it's used to represent all single characters or any string.
? used to represent one character
[] used to represent any character within the brackets
$ cp *.jpg /home/pete/Pictures

A useful command is to use the -r flag, this will recursively copy the files and directories within a directory. You can use the -i flag (interactive) to prompt you before overwriting a file

mv (Move)
Used for moving files and also renaming them.
$ mv oldfile newfile

move a file to a different directory:
$ mv file2 /home/pete/Documents

move more than one file:
$ mv file_1 file_2 /somedirectory

rename directories
$ mv directory1 directory2

Let’s say you did want to mv a file to overwrite the previous one. You can also make a backup of that file and it will just rename the old version with a ~.
$ mv -b directory1 directory2

mkdir (Make Directory)
The mkdir command (Make Directory) is useful for that, it will create a directory if it doesn’t already exist. You can even make multiple directories at the same time.

$ mkdir books paintings
You can also create subdirectories at the same time with the -p (parent flag).
$ mkdir -p books/hemmingway/favorites

rm (Remove)
he rm (remove) command is used to delete files and directories.

$ rm file1
-f or force option tells rm to remove all files, whether they are write protected or not, without prompting the user
$ rm -f file1
Adding the -i flag like many of the other commands, will give you a prompt on whether you want to actually remove the files or directories.
$ rm -i file
You can’t just rm a directory by default, you’ll need to add the -r flag (recursive) to remove all the files and any subdirectories it may have.
$ rm -r directory
$ rmdir directory

Create a file with name -f
touch -- -file
touch ./-file
echo "content" > ./-file
rm -- -file


find
to find a specific one
$ find /home -name puppies.jpg
$ find /home -type d -name MyFolder

Find a file from the root directory that has the word net in it.
find / -type f -name "*net*"

Explanation:
/: Start searching from the root directory.
-type f: Look for files (not directories).
-name "*net*": Match filenames that contain the word "net". The * wildcard matches any characters before or after "net".
Use -iname instead of -name to ignore case

help

built-in bash command that provides help for other bash commands
$ help echo

$ echo --help

man
see the manuals for a command with the man command
$ man ls

whatis
 feeling doubtful about what a command does, you can use the whatis command. The whatis command provides a brief description of command line programs.
 $ whatis cat
 to see a small description of a command


alias
Sometimes typing commands can get really repetitive, or if you need to type a long command many times, it’s best to have an alias you can use for that. To create an alias for a command you simply specify an alias name and set it to the command.

$ alias foobar='ls -la'
command won't save your alias after reboot, so you'll need to add a permanent alias in:

~/.bashrc
You can remove aliases with the unalias command:
$ unalias foobar

exit
To exit from the shell
$ exit
Or the logout command:

$ logout