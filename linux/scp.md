# Retaining Directory Permissions While Using SCP



SCP is a secure file transfer protocol that is used in Linux. SCP is similar to FTP that it allows you to connect to a remote server and transmit your files via the connection. However there is one thing that SCP will allow you to do that FTP will not let you and that’s the ability to retain the original directory permissions when in transport. This article will briefly explain how to do this.

* Log into your Linux server via SSH / Shell
* Locate the file or directory that you are waiting to transfer.
* Now you will connect to the remote server will you normally would using SCP, however now you will at a new attribure to the connection command. An example would be if you wanted to transfer a directory while keeping the permissions you would type the following;
`“scp -rp –preserve /your_directory_path root@remote_server:/parent_directory”`
* The addition of the “-p” attribute allows you to retain permissions while in transfer while the “-r” tells the system to recurisvely copy the entire contents of the directory.
* Using this command will allow you to transfer files quickly without the issue or worrying about reapplying permissions on the directories.

Modified from:
https://www.web24.com.au/tutorials/retaining-directory-permissions-using-scp
