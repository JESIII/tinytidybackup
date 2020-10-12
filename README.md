#Lab 3: Tiny Tidy Backup
The lab furthers our knowledge of how to use commands in a shell script. The goal is to create a script where a source directory is specified and it is backed up to a destination directory as a tar archive. As an added bonus, the script will process command line options and have an option to write the backup to a remote computer via ssh.

Goals
Use the built-in commands in BASH to write a simple program using loops, conditions, and parameters
Use shell variables to verify that directories and files exist; verify that programs executed and exited without error
Learn to use getopt to process command line options
Using ssh and dd, write the backup to a remote directory
Save the SHA checksum to a file
Commit your edits and push your changes to the GitHub hosted repository
Prerequisites
For this assignment, you must have:

a GitHub account linked to your university email address
a Tuffix system or VM
Lab instructions
Cloning the Git Repository
Like the previous exercise, once you are logged into your GitHub account, open the assignment URL that your instructor has shared with you. (Most likely it will be on the class Titanium page.) Clone your repository and begin working on your laboratory assignment.

Create a Shell Script
Using your preferred text editor, create a new file and name it tinytidybackup. The script tinytidybackup shall accept a source directory, a destination directory and process any command line options.

The core operation of the script is to first print out a diagnostic message stating the start time of the backup job, then take the contents of the source directory and create a bzipped tar archive and save it to the destination directory. The tar archive shall have a structured filename where the date is prefixed to the source directory's name and suffixed with .tar.bz2. For example, if the source directory is named my-project and the backup is taking place on January 17, 2019 at 8:30pm, then the destination archive file name is 2019-01-17-20-30-my-project.tar.bz2. Once that operation has completed, an SHA-256 checksum is calculated using the created tar archive and the sha256sum command. The checksum is printed to standard out and appended to a file called SHA256SUMS which is in the destination directory. Finally, a diagnostic message is printed stating the end time of the backup job.

The script will retain N copies of the backup. For example if N is 3 and the source directory is my-project and the backup script is run on a daily basis there will be 3 files in the destination directory that are named similar to 2019-01-17-20-30-my-project.tar.bz2, 2019-01-18-20-30-my-project.tar.bz2, 2019-01-19-20-30-my-project.tar.bz2. A subsequent backup on the 21st would delete 2019-01-17-20-30-my-project.tar.bz2 and in its place 2019-01-20-20-30-my-project.tar.bz2 would exist. Additionally, the checksum file SHA256SUMS is edited to only include the checksums of the remaining backups.

The script understands the following options:

-n Dry run; A dry run is where the script prints out what it would do, including all the files it would backup, where the files will go, the name of the tar archive, yet does not do any operations that would modify the file system.
-r Remote backup; The destination is specified as an SSH style path. The path is specified as user@remotehost:/fully/qualified/path. The backup archive shall be sent over the network and stored on the remote host.
-c N the number of backups to retain; this argument takes an option, N, which is the number of backups to retain.
-v Verbose mode; Make the script very verbose by printing out every step that it does and every file that is being backed up.
-h Help; print out a usage message
Note that the options are not mutually exclusive which means that any of the above options can be used in any combination with one another. Command line arguments can easily be processed using the getopts command.

In order to complete a remote backup, you will need to use the ssh command and the dd command or split command. If the output of the backup shall be small enough to fit in a single file, dd is a good utility to use. If the backup files will be too large, then use split to create many smaller files which can be reassembled to restore the computer.

Additionally, if the program is run without any parameters it prints out a usage message indicating the correct usage of the script. The usage message is like an online help message explaining to the user how to correctly invoke the script.

Remember to add a hash-bang (#!) and include a header at the top of the file with your full name, email address, class, and section number. Include a brief one or two line description of the shell script and any software requirements the script has beyond what is included with Tuffix.

Pushing code to Git (submitting your assignment)
To submit your code, you will need to add the files that you want to submit, commit your changes, and push them into the GitHub repository.

Rubric (10 points)
(5 points) tinytidybackup is named correctly, operates correctly according to the specification, and can create backups locally.
(3 points) The script can perform remote backups.
(1 points) The script has an appropriate header and help message.
(1 points) The script appends checksums correctly to the appropriate file.
Assignments that are not submitted through GitHub shall not be graded.
