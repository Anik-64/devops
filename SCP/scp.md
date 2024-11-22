### SCP

It is a command-line tool used for securely transferring files between a local and a remote host or between two remote hosts using SSH for encryption and authentication.

1. Linux server 1 to server 2

   Send local file to remote 
    ``
    scp file1.txt anik@192.168.40.128:/home/anik/Documents/
    ``

   file1.txt -> Local server file
   anik@192.168.40.128 -> username@ip (remote server)
   /home/anik/Documents/ -> Directory for remote server where the file1.txt is copied
   
   Receive remote file 
   ``
   scp anik@192.168.40.128:/home/anik/Documents/remote_file.txt /tmp/
   ``

